**Command-Line Arguments and Environment Variables in Python for DevOps**

**Introduction**

In DevOps, automation is key. Whether you're writing scripts for CI/CD pipelines, managing cloud resources, or interacting with APIs, understanding **command-line arguments** and **environment variables** is essential.

This article explains these concepts in Python with real-world **DevOps use cases, best practices**, and **practical examples.**

**Why Do DevOps Engineers Need Command-Line Arguments?**

Consider a **calculator script** in Python. If the script **hardcodes values** (e.g., 5 + 10), it lacks flexibility. Instead, we want to allow users to input values dynamically. This is where **command-line arguments** come in.

In DevOps, **command-line arguments** are widely used. For example:

•	Running **AWS CLI** to list S3 buckets:

```sh
aws s3 ls
```

o	aws is the **program**

o	s3 is the **command**

o	ls is the **argument**

By passing values via the command line, we make scripts reusable without modifying the source code.

---

**Implementing Command-Line Arguments in Python**

Python provides the sys module to handle **command-line arguments** dynamically.

**Example: Basic Calculator with Command-Line Arguments**

Here’s how to modify a Python script to take inputs dynamically:

```sh
import sys

# Ensure correct number of arguments
if len(sys.argv) != 4:
    print("Usage: python calculator.py <num1> <operation> <num2>")
    sys.exit(1)

# Read command-line arguments
num1 = float(sys.argv[1])  # First number
operation = sys.argv[2].lower()  # Operation (add, sub, mul)
num2 = float(sys.argv[3])  # Second number

# Perform the operation
if operation == "add":
    result = num1 + num2
elif operation == "sub":
    result = num1 - num2
elif operation == "mul":
    result = num1 * num2
else:
    print("Invalid operation. Use add, sub, or mul.")
    sys.exit(1)

# Print the result
print(f"Result: {result}")
```

**Running the Script**

```sh
python calculator.py 5 add 3
# Output: Result: 8.0
```

**Key Takeaways:**

•	sys.argv stores command-line arguments as a list.

•	sys.argv[0] is always the script name.

•	We convert string inputs into **float** for mathematical operations.

•	Use **error handling** to validate arguments.

---

**Why Use Environment Variables?**

**When Not to Use Command-Line Arguments**

For sensitive information such as **API keys, passwords, and secrets**, command-line arguments are **not safe**. Anyone running history or viewing logs can see these values.

**Environment Variables in DevOps**

In **CI/CD pipelines** or **automation scripts**, we often need credentials. Instead of **hardcoding** them, we use **environment variables.**

For example, in AWS CLI:

```sh
export AWS_ACCESS_KEY_ID="your_access_key"
export AWS_SECRET_ACCESS_KEY="your_secret_key"
```

Now, these values are securely stored and can be accessed in Python.

---

**Using Environment Variables in Python**

**Example: Reading Environment Variables**

```sh
import os

# Read environment variables
api_key = os.getenv("API_KEY")

if api_key:
    print(f"Your API Key: {api_key}")
else:
    print("API Key not found. Set the API_KEY environment variable.")
```

**Setting an Environment Variable**

**Linux/macOS:**

```sh
export API_KEY="your-secret-api-key"
python script.py
```

**Windows (PowerShell):**

```sh
$env:API_KEY="your-secret-api-key"
python script.py
```

**Why Use** os.getenv()?

**•	Prevents hardcoding sensitive data**

**•	Works across environments** (local, CI/CD, production)

**•	Enhances security** (not exposed in logs)

---

**Comparison: Command-Line Arguments vs. Environment Variables**

| Feature                 | Command-Line Arguments          | Environment Variables         |
|-------------------------|--------------------------------|------------------------------|
| **Use Case**            | Dynamic inputs for programs    | Storing sensitive data       |
| **Persistence**         | Temporary (per execution)      | System-wide (session-based)  |
| **Security**            | Visible in logs/history        | Hidden from logs             |
| **Example Usage**       | `python script.py arg1 arg2`  | `export VAR=value`           |

---

**Best Practices for DevOps Engineers**

✅ **Do’s:**

•	Use **command-line arguments** for non-sensitive inputs.

•	Store **secrets** using **environment variables.**

•	Use **error handling** to validate inputs.

•	Document required **environment variables** in a README.md file.

❌ **Don'ts:**

• ❌ **Hardcode sensitive information** in source code.

•	❌ **Store secrets in CI/CD YAML files** as plain text.

•	❌ **Use environment variables for temporary data** (use command-line args instead).

---

**Real-World DevOps Use Cases**

**1.	AWS Authentication in CI/CD Pipelines**

o	Store AWS credentials as **environment variables** to prevent leaks.

**2.	Automated Database Backups**

o	Pass database credentials as **environment variables** instead of command-line arguments.

**3.	Containerized Applications (Docker)**

o	Use ENV **variables** in Dockerfile for configuration.

```sh
ENV APP_PORT=8080
```

---

**Conclusion**

Understanding **command-line arguments** and **environment variables** is essential for **writing flexible and secure Python scripts** in DevOps.

•	Use **command-line arguments** for passing dynamic inputs.

•	Use **environment variables** for handling sensitive information securely.

•	Follow **best practices** to avoid security risks.

By implementing these techniques, you ensure **scalability, security, and maintainability** in your **DevOps automation scripts.**
