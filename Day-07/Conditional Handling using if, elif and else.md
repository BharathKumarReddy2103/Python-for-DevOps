**Conditional Handling using if, elif and else**

**Introduction**

Conditional handling is a fundamental concept in programming, allowing code execution based on specific conditions. In the **DevOps** world, Python scripts are often used to automate tasks, manage cloud resources, and process data. Understanding conditional handling helps DevOps Engineers write efficient and dynamic scripts.

This article explains **conditional handling in Python**, including if, else, and elif statements. It also includes **practical use cases**, real-world DevOps examples, and best practices.

**What is Conditional Handling?**

Conditional handling allows a program to execute a specific block of code based on a given condition. It enables decision-making within scripts, making automation more intelligent and responsive.

**Why is it Important?**

•	Helps in decision-making logic

•	Reduces redundant code execution

•	Automates cloud and DevOps tasks efficiently

•	Enhances error handling in scripts

---

**Conditional Statements in Python**

if **Statement**

The if statement executes a block of code **only if** a condition is True.

**Syntax:**

```sh
if condition:
    # Code block to execute if the condition is True
```

**Example:**

```sh
instance_type = "t2.micro"

if instance_type == "t2.micro":
    print("Instance type is t2.micro - Proceeding with creation.")
```

---

else **Statement**

The else statement executes a block of code when the if condition is False.

**Syntax:**

```sh
if condition:
    # Code block executed if condition is True
else:
    # Code block executed if condition is False
```

**Example:**

```sh
instance_type = "t2.medium"

if instance_type == "t2.micro":
    print("Instance type is t2.micro - Proceeding with creation.")
else:
    print("Instance type is not t2.micro - Cannot proceed.")
```

---

elif **Statement**

The elif (short for "else if") statement checks multiple conditions sequentially.

**Syntax:**

```sh
if condition1:
    # Code block for condition1
elif condition2:
    # Code block for condition2
else:
    # Default code block if none of the conditions are met
```

**Example:**

```sh
instance_type = "t2.large"

if instance_type == "t2.micro":
    print("This instance type costs $2 per day.")
elif instance_type == "t2.medium":
    print("This instance type costs $4 per day.")
elif instance_type == "t2.xlarge":
    print("This instance type costs $8 per day.")
else:
    print("Invalid instance type. Please select a valid option.")
```

---


**Real-World Use Case: AWS EC2 Instance Selection**

Let’s apply conditional handling to a **real-world DevOps scenario** where a user selects an **AWS EC2 instance type** via a Python script.

**Use Case:**

•	The script takes **user input** (instance type) via **command-line arguments.**

•	It validates the input and provides feedback on instance pricing.

•	It restricts users from creating instances beyond AWS Free Tier.

**Implementation:**

```sh
import sys

# Read user input from command line arguments
if len(sys.argv) < 2:
    print("Usage: python script.py <instance_type>")
    sys.exit(1)

instance_type = sys.argv[1]  # Taking instance type from command-line input

# Conditional handling for instance type selection
if instance_type == "t2.micro":
    print("Instance type: t2.micro - Free Tier Eligible ($2 per day).")
elif instance_type == "t2.medium":
    print("Instance type: t2.medium - Cost: $4 per day.")
elif instance_type == "t2.xlarge":
    print("Instance type: t2.xlarge - Cost: $8 per day.")
else:
    print("Invalid instance type. Please provide a valid AWS instance type.")
```

**Execution:**

Run the script with a command-line argument:

```sh
python script.py t2.micro
```

**Output:**

```sh
Instance type: t2.micro - Free Tier Eligible ($2 per day).
```

---

**Best Practices for Conditional Handling in Python**

**1.	Use Meaningful Conditions:**

o	Avoid hardcoded values directly inside conditions.

o	Use **constants** or **config variables** instead.

**2.	Follow Python Indentation Rules:**

o	Python uses indentation instead of {} brackets.

o	Keep indentation **consistent** to avoid IndentationError.

**3.	Use elif Instead of Multiple if Statements:**

o	Avoid unnecessary if conditions to **optimize performance.**

**4.	Handle Invalid Inputs:**

o	Use a final else block to handle unexpected conditions.

o	Provide **user-friendly error messages.**

**5.	Modularize Code for Reusability:**

o	Use **functions** for better code readability and maintainability.

```sh
def get_instance_cost(instance_type):
    if instance_type == "t2.micro":
        return "$2 per day"
    elif instance_type == "t2.medium":
        return "$4 per day"
    elif instance_type == "t2.xlarge":
        return "$8 per day"
    else:
        return "Invalid instance type"
```

**6.	Optimize for Performance:**

o	Use **dictionaries** for mapping instead of multiple if-elif conditions.

```sh
instance_costs = {
    "t2.micro": "$2 per day",
    "t2.medium": "$4 per day",
    "t2.xlarge": "$8 per day"
}

print(instance_costs.get(instance_type, "Invalid instance type"))
```

---

**Summary**

**Key Takeaways:**

•	Conditional handling (if, else, elif) is **essential for decision-making** in Python.

•	**Real-world DevOps applications** include AWS EC2 instance selection.

•	**Best practices** improve script performance and maintainability.

•	**Optimized approaches** like **dictionaries** can replace multiple if-elif blocks.

**Next Steps:**

**•	Practice writing Python scripts** with conditional handling.

**•	Apply conditions** in CI/CD pipelines, automation scripts, and cloud configurations.

**•	Explore advanced topics** like exception handling and loops.

---

**Contribute to This Repository**

If you found this guide helpful, feel free to:

•	⭐ **Star this GitHub repository**

•	🛠 **Open a pull request** with additional examples

•	💬 **Provide feedback** in the discussions

Your contributions help improve open-source learning for DevOps engineers worldwide.
