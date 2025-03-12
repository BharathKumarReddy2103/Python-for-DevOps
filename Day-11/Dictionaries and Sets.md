**Dictionaries and Sets**

**Introduction**

In **DevOps** and **Cloud Engineering**, automation is key, and **Python** plays a significant role in writing scripts for managing infrastructure, interacting with APIs, and automating repetitive tasks. Two **important data structures** in Python that DevOps engineers frequently use are **dictionaries** and **sets.**

**Why Are Dictionaries Important in DevOps?**

Dictionaries in Python are widely used to store **structured data** in key-value format. Many **cloud platforms and DevOps tools**, such as **AWS, GitHub, and Jira**, return responses in JSON format, which can be easily converted into Python dictionaries for further processing.

For example, if a DevOps engineer queries AWS EC2 instances using the **Boto3 library**, the response is in **JSON format**, which is converted into a dictionary to extract meaningful information.

In this guide, we will cover:

**•	Dictionaries in Python**: How they work and their importance in DevOps.

**•	Real-World Example**: Using Python to fetch pull request information from a GitHub repository.

**•	Sets in Python:** Their role in ensuring unique data storage.

**•	Practical Use Cases:** Where dictionaries and sets fit into DevOps workflows.

---

**Understanding Python Dictionaries**

A **dictionary** in Python is an **unordered collection of key-value pairs**, where:

•	Each **key** is unique.

•	Each **value** is associated with a key.

•	The syntax for defining a dictionary uses **curly braces** {}.

**Example: Storing EC2 Instance Details in a Dictionary**

A dictionary is the best data structure when storing **properties of an EC2 instance**, as it allows easy access to instance attributes like **ID, instance type, and public IP.**

```sh
# Define an EC2 instance dictionary
ec2_instance = {
    "InstanceID": "i-1234567890abcdef0",
    "InstanceType": "t2.micro",
    "PublicIP": "192.168.1.10"
}

# Access instance details
print(f"Instance ID: {ec2_instance['InstanceID']}")
print(f"Instance Type: {ec2_instance['InstanceType']}")
print(f"Public IP: {ec2_instance['PublicIP']}")
```

**Why Use Dictionaries Instead of Lists?**

Consider storing details of **multiple EC2 instances:**

**•	Using a list:** It becomes difficult to identify which value corresponds to which attribute.

**•	Using a dictionary:** We store data as key-value pairs, making it easy to retrieve specific values.

**Example: Storing Multiple EC2 Instances in a List of Dictionaries**

```sh
# List of dictionaries storing multiple EC2 instances
ec2_instances = [
    {"InstanceID": "i-123456", "InstanceType": "t2.micro", "PublicIP": "192.168.1.10"},
    {"InstanceID": "i-789012", "InstanceType": "t2.medium", "PublicIP": "192.168.1.11"},
    {"InstanceID": "i-345678", "InstanceType": "t2.large", "PublicIP": "192.168.1.12"}
]

# Access the instance type of the second EC2 instance
print(f"Instance Type of second EC2: {ec2_instances[1]['InstanceType']}")
```

---

**Real-World Example: Fetching GitHub Pull Requests Using Python**

A common DevOps task is **interacting with APIs** to fetch data. Let's demonstrate how to use **Python dictionaries** by querying the **GitHub API** to get pull request details of a repository.

**Steps:**

**1.	Use the** requests **module** to make API calls.

**2.	Fetch pull request data** from a GitHub repository.

**3.	Convert JSON response into a Python dictionary.**

**4.	Extract and print required details** (e.g., pull request author names).

**Install Required Module**

Ensure the requests module is installed before running the script:

```sh
pip install requests
```

**Python Script to Fetch GitHub Pull Requests**

```sh
import requests

# Step 1: Define the API URL (replace with your GitHub repository details)
repo_owner = "kubernetes"
repo_name = "kubernetes"
url = f"https://api.github.com/repos/{repo_owner}/{repo_name}/pulls"

# Step 2: Make an API request
response = requests.get(url)

# Step 3: Convert response JSON into a dictionary
pull_requests = response.json()

# Step 4: Extract and print pull request author names
print("List of contributors who created pull requests:")
for pr in pull_requests:
    print(f"- {pr['user']['login']}")
```

**Expected Output**

```sh
List of contributors who created pull requests:
- user1
- user2
- user3
```

**Explanation**

•	We use requests.get(url) to fetch pull requests from GitHub.

•	The API response is **JSON**, which is converted into a **Python dictionary.**

•	Using a for loop, we extract and print the **GitHub usernames** of contributors.

This example shows **how DevOps engineers use Python dictionaries to process API responses** efficiently.

---

**Introduction to Python Sets**

A **set** in Python is a **collection of unique elements**. Unlike lists, sets:

•	Do **not allow duplicate values.**

•	Are **unordered** (elements have no fixed positions).

•	Use **curly braces** {} or the set() constructor.

**Example: Using Sets to Store Unique S3 Bucket Names**

AWS S3 bucket names **must be globally unique**. A **set** is the perfect data structure to store and manage them.

```sh
# Define a set of S3 bucket names
s3_buckets = {"devops-logs", "production-data", "ml-models"}

# Add a new bucket (duplicates are not allowed)
s3_buckets.add("new-backup-bucket")

# Attempt to add a duplicate bucket (it will be ignored)
s3_buckets.add("devops-logs")

print(s3_buckets)
```

**Output**

```sh
{'devops-logs', 'production-data', 'ml-models', 'new-backup-bucket'}
```

Since devops-logs was already present in the set, it **was not added again**.

---

**Set Operations for DevOps Use Cases**

**1. Union (|) - Merging Two Sets**

```sh
team_a = {"Alice", "Bob", "Charlie"}
team_b = {"Charlie", "David", "Eve"}

combined_team = team_a | team_b
print(combined_team)  # Output: {'Alice', 'Bob', 'Charlie', 'David', 'Eve'}
```

**2. Intersection (&) - Finding Common Elements**

```sh
common_members = team_a & team_b
print(common_members)  # Output: {'Charlie'}
```

**3. Difference (-) - Finding Unique Elements**

```sh
unique_to_team_a = team_a - team_b
print(unique_to_team_a)  # Output: {'Alice', 'Bob'}
```

---

**Conclusion**

In this article, we explored:

✅ **Dictionaries** – Essential for handling structured data in DevOps (e.g., JSON responses from APIs).

✅ **GitHub API Example** – Demonstrated how to process pull request data using Python dictionaries.

✅ **Sets** – Useful for storing unique data (e.g., S3 bucket names, user lists).

**Next Steps**

•	**Practice** writing Python scripts that integrate with other DevOps tools like AWS, Jira, and GitHub.

•	**Contribute** to open-source projects by improving Python scripts used in automation.

•	**Explore Advanced Topics** – Learn about Python’s collections module, which provides advanced data structures.
