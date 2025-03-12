**List – Part 2**

**Introduction**

In **Python for DevOps**, handling lists is a crucial skill, as lists are widely used in automation scripts, configuration management, and infrastructure as code (IaC). This article is **Part 2** of our deep dive into lists, focusing on practical DevOps use cases.

In this tutorial, we will:

•	Explore a **real-world DevOps use case** for handling lists.

•	Learn how to **list all files in multiple folders** provided by the user.

•	Implement **Python loops, OS modules, and exception handling** effectively.

•	Follow a structured approach to **writing modular Python scripts.**

By the end of this guide, you'll have a **production-ready Python script** that can be used in **real DevOps automation tasks.**

---

**Understanding the Problem Statement**

**Objective:**

We will **write a Python program that lists all files in multiple folders** specified by the user.

**Expected Input:**

The user will provide a **list of folder paths** separated by spaces. Example:

```sh
/tmp /var/log /opt
```

**Expected Output:**

The script will **list all files** in each folder. Example:

```sh
Listing files for folder: /tmp
- file1.txt
- file2.log
- script.py

Listing files for folder: /var/log
- syslog
- kern.log
- auth.log
```

If a folder **does not exist or is inaccessible**, the script should handle it **gracefully** without failing.

---

**Breaking Down the Solution – Algorithm**

Before writing the Python script, let's **structure our approach:**

**1.	Read input from the user** – Accept folder paths as a space-separated string.

**2.	Convert input into a list** – Convert the space-separated string into a Python list.

**3.	Loop through each folder** – Process one folder at a time.

**4.	Use the OS module** – Fetch files inside each folder using Python’s os.listdir().

**5.	Handle errors gracefully** – Use try-except to manage:

**o	Non-existing folders** (FileNotFoundError).

**o	Permission issues** (PermissionError).

**6.	Print results clearly** – Ensure the output is readable and structured.

**7.	Modularize the script** – Use functions for better reusability.

---

**Implementing the Python Script**

**Step 1: Reading Input from the User**

Instead of expecting a **Python list format**, we ask the user to enter folder paths as a **space-separated string**. We then convert this into a list using .split().

```sh
# Read input from the user
folders = input("Please provide list of folder names separated by spaces: ").split()
```

Example input:

```sh
/tmp /var/log /etc
```

Output:

```sh
['/tmp', '/var/log', '/etc']
```

---

**Step 2: Loop Through Each Folder**

We iterate through the list of folders using a for loop.

```sh
for folder in folders:
    print(f"Listing files for folder: {folder}")
```

---

**Step 3: Use the OS Module to List Files**

Python’s os.listdir() helps list files in a folder.

```sh
import os

for folder in folders:
    try:
        files = os.listdir(folder)  # Fetch all files in the folder
        for file in files:
            print(f"- {file}")
    except FileNotFoundError:
        print(f"Error: The folder '{folder}' does not exist.")
    except PermissionError:
        print(f"Error: No access to the folder '{folder}'.")
```

---

**Final Optimized Python Script**

To improve code **modularity and readability**, let's use **functions**.

```sh
import os

def list_files_in_folder(folder):
    """Lists all files in a given folder and handles exceptions."""
    try:
        files = os.listdir(folder)
        print(f"\nListing files for folder: {folder}")
        for file in files:
            print(f"- {file}")
    except FileNotFoundError:
        print(f"Error: The folder '{folder}' does not exist.")
    except PermissionError:
        print(f"Error: No access to the folder '{folder}'.")

def main():
    """Main function to read input and list files in folders."""
    folders = input("Please provide list of folder names separated by spaces: ").split()
    
    for folder in folders:
        list_files_in_folder(folder)

if __name__ == "__main__":
    main()
```

---

**Handling Exceptions in DevOps Scripts**

**1. Handling Non-Existent Folders**

•	If a user provides a **wrong folder path**, handle it gracefully with:

```sh
except FileNotFoundError:
    print(f"Error: The folder '{folder}' does not exist.")
```

**2. Handling Permission Issues**

•	If the script runs without **sufficient permissions**, it should not fail:

```sh
except PermissionError:
    print(f"Error: No access to the folder '{folder}'.")
```

---

**Real-World Use Cases in DevOps**

This script can be **extended** to:

**1.	Monitor log files** across different servers.

**2.	Check for outdated files** in multiple directories.

**3.	Automate backup tasks** by listing and compressing files.

**4.	Integrate with CI/CD Pipelines** to validate log paths before deployment.

**Example: Running the Script**

```sh
$ python list_files.py
Please provide list of folder names separated by spaces: /tmp /var/log /xyz
Listing files for folder: /tmp
- file1.txt
- file2.log
- script.py

Listing files for folder: /var/log
- syslog
- kern.log
- auth.log

Error: The folder '/xyz' does not exist.
```

---

**Best Practices for Writing Python Scripts in DevOps**

**1.	Use Modular Functions** – Break code into reusable functions.

**2.	Handle Exceptions Gracefully** – Avoid abrupt failures.

**3.	Write Readable and Maintainable Code** – Use meaningful variable names.

**4.	Use Logging Instead of Print Statements** – For production-ready scripts.

**5.	Optimize for Performance** – Avoid unnecessary loops and operations.

**6.	Integrate with CI/CD Pipelines** – Make scripts deployment-ready.

**7.	Follow the Pythonic Way** – Use built-in modules whenever possible.

---

**Conclusion**

By following this guide, you now have a **DevOps-friendly Python script** that:

•	Takes user **input dynamically.**

**•	Lists files across multiple directories.**

•	Handles **common errors gracefully.**

•	Uses **modular functions** for better structure.

•	Can be **extended for real-world automation tasks.**

This script serves as a **building block** for more complex automation in **DevOps and Cloud environments**. If you're interested in **enhancing this project**, consider:

•	Adding **logging instead of print statements.**

•	Integrating with **AWS S3 to list files in S3 buckets.**

•	Converting it into a **CLI tool with argparse.**

**Next Steps**

🚀 **Fork this script on GitHub**, experiment with modifications, and contribute your improvements.
