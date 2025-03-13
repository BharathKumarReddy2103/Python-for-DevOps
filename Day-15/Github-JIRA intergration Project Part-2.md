**Automating GitHub and Jira Integration Using Python and Flask**

**Introduction**

Managing issues efficiently is a crucial aspect of **DevOps and Cloud Engineering**. In a real-world scenario, development teams often track issues using **GitHub Issues**, while project management teams rely on **Jira** to monitor and resolve them systematically.

However, manually **creating Jira tickets for GitHub issues** can be time-consuming, especially when dealing with hundreds of issues daily. To streamline this process, we can **automate GitHub and Jira integration using Python scripting and Flask.**

This guide walks you through an end-to-end implementation of **GitHub-Jira integration** using:

**•	GitHub Webhooks** to listen for issue comments

**•	Flask APIs** to process webhook data

**•	Jira APIs** to create Jira tickets automatically

By the end of this tutorial, you'll have a working integration that allows developers to comment "SLJ" on a GitHub issue, triggering an **automated Jira ticket creation.**

---

**Project Overview**

**Problem Statement**

•	Developers manually triage GitHub issues and decide whether they need to be tracked in Jira.

•	Creating Jira tickets manually is **time-consuming and prone to errors.**

•	There needs to be an automated way to track **only valid issues** in Jira.

**Solution**

**•	Use GitHub Webhooks** to monitor issue comments.

•	When a developer comments **"SLJ"**, trigger an API call to **Flask-based Python application.**

•	The Flask application will process the comment and **create a Jira ticket** using **Jira REST API.**

---

**Prerequisites**

Before we begin, ensure you have:

**•	An AWS EC2 instance** (or any server) with Python 3 installed

**•	A GitHub repository** with admin access to configure webhooks

**•	Jira Cloud account** with API access

**•	Python libraries:** flask, requests, json, and os

---

**Step 1: Setting Up the Flask API**

First, install Flask if you haven’t already:

```sh
pip install flask
```

Now, create a new file github_jira.py and write the following Flask API code:

```sh
from flask import Flask, request, jsonify
import requests
import json
import os

app = Flask(__name__)

# Jira credentials (Replace with your own values)
JIRA_URL = "https://your-jira-instance.atlassian.net/rest/api/2/issue"
JIRA_AUTH = ("your-email@example.com", "your-jira-api-token")
JIRA_PROJECT_KEY = "DEVOPS"

@app.route("/create-jira", methods=["POST"])
def create_jira_ticket():
    data = request.json  # Get the webhook payload

    # Extract issue details
    try:
        comment_text = data["comment"]["body"]
        github_issue_url = data["issue"]["html_url"]
        github_issue_title = data["issue"]["title"]
        github_issue_creator = data["issue"]["user"]["login"]

        # Check if the comment is "SLJ"
        if comment_text.strip().lower() == "slj":
            jira_payload = {
                "fields": {
                    "project": {"key": JIRA_PROJECT_KEY},
                    "summary": github_issue_title,
                    "description": f"Issue created from GitHub: {github_issue_url} \n\n Created by: {github_issue_creator}",
                    "issuetype": {"name": "Task"}
                }
            }

            headers = {"Content-Type": "application/json"}
            response = requests.post(JIRA_URL, auth=JIRA_AUTH, headers=headers, data=json.dumps(jira_payload))

            if response.status_code == 201:
                return jsonify({"message": "Jira ticket created successfully!", "ticket": response.json()}), 201
            else:
                return jsonify({"error": "Failed to create Jira ticket", "details": response.text}), 400
        else:
            return jsonify({"message": "Comment ignored. Not 'SLJ'."}), 200

    except KeyError as e:
        return jsonify({"error": f"Missing key in payload: {e}"}), 400

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000, debug=True)
```

**Explanation**

•	The API listens for **POST requests** at /create-jira.

•	Extracts the **GitHub issue details** from the webhook payload.

•	Checks if the comment **contains "SLJ".**

•	If yes, creates a **Jira ticket** with the issue title and link.

•	If not, it **ignores** the comment.

---

**Step 2: Deploying the Flask API**

Upload the github_jira.py file to your **EC2 instance** and run it:

```sh
python3 github_jira.py
```

This starts the Flask application on port **5000.**

---

**Step 3: Configuring GitHub Webhooks**

**1.**	Go to your GitHub **repository settings → Webhooks.**

**2.**	Click **Add webhook.**

**3.**	**Set Payload URL** to your Flask API:

```sh
http://<your-ec2-public-ip>:5000/create-jira
```

**4.**	Set **Content type** to application/json.
  
**5.**	Select **"Just the push event"** and then choose "Issue comments".

**6**.	Click **Add webhook.**

---

**Step 4: Testing the Integration**

1.	Go to any **GitHub issue** in your repository.
2.	Add a comment:

  ```sh
SLJ
```
3.	Check your **Jira backlog**. A new Jira ticket should be created automatically.

---

**Step 5: Enhancing the Integration**

**1. Handling Webhook Security**

GitHub allows you to **secure webhooks** with a secret key. Modify your Flask API to **validate signatures** before processing requests.

```sh
import hashlib
import hmac

GITHUB_SECRET = "your-webhook-secret"

def verify_github_signature(payload, signature):
    mac = hmac.new(GITHUB_SECRET.encode(), payload, hashlib.sha256).hexdigest()
    return hmac.compare_digest(f"sha256={mac}", signature)
```

**2. Deploying with Gunicorn and Nginx**

Instead of using the **Flask development server**, deploy it with **Gunicorn** and **Nginx** for production:

```sh
pip install gunicorn
gunicorn --bind 0.0.0.0:5000 github_jira:app
```

**3. Logging and Error Handling**

Improve logging by adding error handling with **Python logging module**:

```sh
import logging

logging.basicConfig(level=logging.INFO, filename="app.log", filemode="a", format="%(asctime)s - %(levelname)s - %(message)s")
```

---

**Conclusion**

By following this tutorial, you have successfully built an **automated GitHub-Jira integration** that:

**•	Monitors GitHub issue comments** using webhooks

**•	Processes the request with Flask**

**•	Creates Jira tickets automatically** via APIs

This **real-world automation** saves **manual effort** and is widely used in organizations to improve issue tracking.
