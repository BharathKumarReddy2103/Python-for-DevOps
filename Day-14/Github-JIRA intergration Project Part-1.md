**Automating Jira Ticket Creation from GitHub Issues Using Python**

**Introduction**

In modern DevOps workflows, **seamless integration between GitHub and Jira** is crucial for efficient bug tracking and project management. Developers and QA engineers frequently report issues in GitHub repositories, and these issues often need to be manually transferred to Jira for better tracking and prioritization.

This manual process is time-consuming and repetitive. **What if we could automate it?**

In this guide, you'll learn how to:

**•	Automate the creation of Jira tickets** from GitHub issues using Python.

•	Use **GitHub Webhooks** to trigger a Jira API request.

•	Host a **Flask-based Python application** on an AWS EC2 instance.

By the end, you'll have a fully functional automation pipeline that **listens for GitHub comments and automatically creates Jira tickets** when specific keywords are used.

---

**Problem Statement**

As a **DevOps Engineer**, your organization may have **hundreds of GitHub repositories**. Developers and QA teams frequently report bugs in these repositories. However, not all reported issues are genuine; some may be configuration errors.

Currently, the **manual process** involves:

1.	A developer reviews the GitHub issue.
  
2.	If it's a valid bug, they **manually log into Jira** and create a ticket.
  
3.	They copy-paste details from GitHub into Jira.

4.	The Jira ticket is then assigned and tracked.

This is **tedious, repetitive, and error-prone**. Instead, we need **automation** to:

•	Detect a specific **comment trigger** (e.g., slj) on a GitHub issue.

**•	Automatically create a Jira ticket** with relevant details.

•	Assign it to the appropriate team or individual.

---

**Solution Architecture**

The automation consists of three key components:

**1. GitHub Webhook**

•	Monitors comments on GitHub issues.

•	Triggers when a specific keyword (e.g., slj) is detected.

•	Sends issue details via **JSON payload** to a Python API.

**2. Python API (Flask Application)**

•	Hosted on an **AWS EC2 instance.**

•	Receives webhook data from GitHub.

•	Parses the JSON payload and extracts **issue details.**

•	Makes an API request to Jira to create a ticket.

**3. Jira API Integration**

•	Uses **Jira REST API** for ticket creation.

•	Maps GitHub issue details to Jira ticket fields.

•	Assigns the ticket to a relevant user/team.

Below is the complete implementation.

---

**Step 1: Setting Up Jira**

Before automating the process, you need:

1.	A **Jira Cloud account** (sign up at Atlassian Jira).
  
2.	A **Jira API token** for authentication.

**Creating a Jira API Token**

**1.	Go to Jira Profile Settings** → Security.

2.	Click on **Manage API Tokens.**

3.	Generate a new token and **save it securely.**

---

**Step 2: Configuring a GitHub Webhook**

1.	Navigate to your GitHub repository → **Settings → Webhooks.**
  
2.	Click **"Add Webhook"** and enter:

o	**Payload URL**: http://<your-ec2-ip>:5000/webhook

o	**Content type**: application/json

o	**Triggers**: Select **Issue comments.**

3.	Save the webhook.
Now, GitHub will **send an event** to your Flask API whenever an issue receives a comment.

---

**Step 3: Writing the Python Automation Script**

**Install Required Dependencies**

```sh
pip install flask requests
```

**Python Script to Process GitHub Webhooks**

```sh
from flask import Flask, request, jsonify
import requests
import json

app = Flask(__name__)

# Jira credentials
JIRA_URL = "https://your-jira.atlassian.net"
JIRA_API_TOKEN = "your-api-token"
JIRA_EMAIL = "your-email@example.com"
JIRA_PROJECT_KEY = "PROJ"  # Replace with your Jira project key

# Function to create a Jira issue
def create_jira_ticket(issue_title, issue_description):
    url = f"{JIRA_URL}/rest/api/2/issue/"
    auth = (JIRA_EMAIL, JIRA_API_TOKEN)
    headers = {"Content-Type": "application/json"}

    payload = {
        "fields": {
            "project": {"key": JIRA_PROJECT_KEY},
            "summary": issue_title,
            "description": issue_description,
            "issuetype": {"name": "Bug"},
        }
    }

    response = requests.post(url, auth=auth, headers=headers, json=payload)
    return response.json()

@app.route("/webhook", methods=["POST"])
def github_webhook():
    data = request.json

    # Extract issue details
    issue_title = data["issue"]["title"]
    issue_body = data["issue"]["body"]
    comment_text = data["comment"]["body"]

    # Trigger Jira creation only if a comment contains "slj"
    if "slj" in comment_text.lower():
        jira_response = create_jira_ticket(issue_title, issue_body)
        return jsonify(jira_response), 201

    return jsonify({"message": "No action taken"}), 200

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

**Step 4: Hosting the Flask Application on AWS EC2**

**1.	Launch an EC2 Instance** (Ubuntu or Amazon Linux).

**2.	Install Python and Dependencies**

```sh
sudo apt update && sudo apt install python3-pip -y
pip3 install flask requests
```

**1.	Run the Flask Application**

```sh
python3 script.py
```

**1.	Allow Inbound HTTP Traffic** (Port 5000) in the EC2 Security Group.

---

**Step 5: Testing the Automation**

**1.	Create an issue** in your GitHub repository.

**2**.	Add a **comment** containing slj.

**3.**	The Flask application receives the webhook.
  
4.	A Jira ticket is **automatically created.**

---

**Best Practices**

**•	Use Environment Variables** for sensitive credentials (JIRA_API_TOKEN).

**•	Deploy as a Docker Container** for better scalability.

**•	Enhance Logging** for debugging webhook events.

**•	Use GitHub Actions** to automate deployment of the Flask app.

---

**Conclusion**

In this tutorial, you learned how to **automate Jira ticket creation** from GitHub issues using Python, Flask, and webhooks. This reduces **manual effort, improves developer productivity, and enhances bug tracking.**

🚀 **Next Steps**

•	Deploy the Flask API on **AWS Lambda** for a serverless setup.

•	Extend functionality to **automatically assign Jira tickets.**

•	Integrate with **Slack notifications** for real-time alerts.

Would you like to contribute or improve this project? **Check out the repository and submit a PR**.
