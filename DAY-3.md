# Github copilot agenda - Day 3

## Template & Code Quality

* [Code Reviews with GitHub Copilot](#code-reviews-with-github-copilot)
* [Custom Coding Standards with GitHub Copilot](#custom-coding-standards-with-github-copilot)
* [Lint Error Fixes with GitHub Copilot: Python and Angular Edition](#lint-error-fixes-with-github-copilot-python-and-angular-edition)
* [GitHub Copilot Review Guidelines](#github-copilot-review-guidelines)


## Project Integration Simulation

* [Build "She Inspires" Quote Generator](#build-she-inspires-quote-generator)
* [Build an Angular Market Share Dashboard with Self-Healing AI](#build-an-angular-market-share-dashboard-with-self-healing-ai)
* [Crafting a Webpage with HTML & CSS](#crafting-a-webpage-with-html--css)
* [Building a Sales Dashboard using GitHub Copilot in VS Code](#building-a-sales-dashboard-using-github-copilot-in-vs-code)


## Responsible & Agentic AI

* [GitHub Copilot and Generative AI](#github-copilot-and-generative-ai)
* [Journey with AI Companion](#journey-with-ai-companion)
* [GitHub Copilot Coding Agent](#github-copilot-coding-agent)
* [Kubernetes Automation with Generative AI using GitHub Copilot](#kubernetes-automation-with-generative-ai-using-github-copilot)


### Cost Optimization & Summary

* [GitHub Copilot Billing 6 Nov 2023](#github-copilot-billing-6-nov-2023)


---

# **Code Reviews with GitHub Copilot**

## **Prerequisites**

Before we dive into Copilot-based reviews, make sure you have:

* GitHub Copilot enabled in your editor (e.g., VS Code)
* Your codebase open in the workspace
* Optionally, a **coding standards file** like `coding.yml` or `coding.json`

---

## **Step 1: Load Your Code File**

Start by loading the code file you want to review. For this example, we’ll use a JS file named `stringUtils.js` and a Python file `binary_search.py`.

**Example: `stringUtils.js`**

```javascript
function validateEmail(email) {
  var re = /\S+@\S+\.\S+/;
  return re.test(email);
}

function cleanStr(str) {
  return str.replace(/[^a-z0-9]/gi, '').toLowerCase();
}

function check(str1, str2) {
  const rc1 = cleanStr(str1);
  const rc2 = cleanStr(str2);
  return rc1.split('').sort().join('') === rc2.split('').sort().join('');
}
```

---

## **Step 2: Check Naming Conventions**

**Prompt to Copilot:**

> **“Do the names of my functions and variables clearly convey their purpose?”**

**Expected Copilot Suggestions:**

* `check` → `areAnagrams`: Better describes the intent
* `re` → `emailRegex`: More descriptive
* `rc1`, `rc2` → `cleanedStr1`, `cleanedStr2`: Improved clarity
* `cleanStr` is okay, but `cleanString` might be better in some teams

---

**Example After Copilot Suggestions:**

```javascript
function validateEmail(email) {
  var emailRegex = /\S+@\S+\.\S+/;
  return emailRegex.test(email);
}

function cleanString(str) {
  return str.replace(/[^a-z0-9]/gi, '').toLowerCase();
}

function areAnagrams(str1, str2) {
  const cleanedStr1 = cleanString(str1);
  const cleanedStr2 = cleanString(str2);
  return cleanedStr1.split('').sort().join('') === cleanedStr2.split('').sort().join('');
}
```

---

## **Step 3: Check Naming Convention Consistency Across Workspace**

**Prompt to Copilot:**

> **“Is the function name `areAnagrams` consistent with the naming conventions used across my workspace?”**

**Expected Copilot Behavior:**

* Copilot will scan other files (if allowed) and suggest whether your naming pattern matches others.
* E.g., it may suggest `isAnagram` if most functions use `isX` format.

---

## **Step 4: Use a Coding Standards File**

Suppose you have a file named `coding.yml` with the following standard:

```yaml
function_naming: snake_case
variable_naming: snake_case
max_line_length: 79
indentation: 4_spaces
comment_format:
  docstring: numpydoc
  inline: "# style"
```

### **Example Python Code (`binary_search.py`)**

```python
def checkPrime(n):
  if n <= 1:
    return False
  for i in range(2, n):
    if n % i == 0:
      return False
  return True
```

**Prompt to Copilot:**

> **“Review this Python file using coding standards defined in `coding.yml`.”**

**Expected Suggestions from Copilot:**

* `checkPrime` → `check_prime` (enforce `snake_case`)
* Add proper indentation (4 spaces)
* Add docstrings
* Add inline comments using `#`

---

**Final Suggested Version from Copilot:**

```python
def check_prime(n):
    """
    Check if a number is prime.

    Parameters
    ----------
    n : int
        The number to check.

    Returns
    -------
    bool
        True if n is prime, False otherwise.
    """
    if n <= 1:
        return False
    for i in range(2, n):
        if n % i == 0:
            return False
    return True  # Prime number
```

---

## **Additional Prompts You Can Use with Copilot**

Here are some more helpful prompts to fine-tune your code review:

### **General Naming Review**

```text
"Check if function and variable names follow camelCase or snake_case as per project standards."
```

### **Code Clarity and Legibility**

```text
"Are the function names and variable names readable and self-explanatory?"
```

### **Codebase Consistency**

```text
"Does the naming convention of this function match other similar functions in my project?"
```

### **Custom Coding Standards**

```text
"Review this code against the standards in 'coding.json' in my workspace."
```

---

## **Conclusion**

GitHub Copilot can **dramatically speed up code reviews**, especially for **coding standards and naming conventions**. Use prompts strategically to:

* Evaluate code clarity
* Enforce your team’s naming rules
* Ensure consistency across large codebases


---

# **Custom Coding Standards with GitHub Copilot**

## **Overview**

GitHub Copilot doesn’t "learn" your custom coding standards permanently. However, it is **context-aware** — meaning it will adapt its code suggestions based on:

* Open files and project structure
* Previous prompts in the chat
* Code you supply in your workspace (e.g., a JSON/YAML with rules)

---

## **Step-by-Step Guide: Custom Coding Standards with Copilot**

### **Step 1: Define Your Coding Standard**

You can define coding standards in a file using **JSON**, **YAML**, or even **inline comments**.

**JSON Example (`coding_standard.json`)**

```json
{
  "function_naming": "snake_case",
  "class_naming": "PascalCase",
  "constant_naming": "UPPER_CASE",
  "docstring_style": "numpy",
  "comment_style": "full_sentence_capitalized"
}
```


**YAML Example (`coding_standard.yaml`)**
```yaml
function_naming: camelCase
class_naming: PascalCase
constant_naming: UPPER_CASE
docstring_style: google
comment_style: full_sentence_capitalized
```

---

### **Step 2: Add This File to Your Workspace**

Place `coding_standard.json` or `coding_standard.yaml` in your open project/workspace where Copilot can "see" it.

This works **best** in **VS Code** or **GitHub Copilot Chat in VS Code Insiders**.

---

### **Step 3: Prompt Copilot with a Task + Mention the Coding Standard**

**Prompt Example:**
Use a clear prompt like this:

```python
# Use the coding standards defined in coding_standard.json
# Define a function to check if a string is a palindrome
```

---

**Copilot Suggestion (with `snake_case` from JSON)**

![alt text](../images/img279.png)


**Notice:**
* Function name: `snake_case`
* Docstring: NumPy-style
* Clear, capitalized comment structure

---

### **Step 4: Change the Coding Standard and Regenerate**

Update the YAML to use **camelCase** and regenerate.

**YAML:**

```yaml
function_naming: camelCase
```

**Prompt Again:**

```python
# Use coding standards defined in coding_standard.yaml
# Define a function to check if a string is a palindrome
```

---

**Copilot Suggestion (with `camelCase`)**

![alt text](../images/img280.png)

✔️ Adheres to:
* Function name: `camelCase`
* Docstring: Google-style
* Variables: `camelCase` remains intact

---

### **Step 5: Use Constants with Your Custom Standard**

**Prompt Example:**

```python
# Use coding standards from coding_standard.yaml
# Define a function to calculate simple interest
# Interest rate should be defined as a constant
```

---

### **Copilot Suggestion:**

```python
INTEREST_RATE = 0.05  # 5% interest

def calculateInterest(principal, time):
    """
    Calculate simple interest based on the principal and time.

    Args:
        principal (float): The principal amount.
        time (int): Time in years.

    Returns:
        float: Calculated interest.
    """
    return principal * INTEREST_RATE * time
```

✔️ `INTEREST_RATE` follows UPPER\_CASE format
✔️ `calculateInterest` is camelCase
✔️ Google-style docstring

---

### **Step 6: Reminder — Context Fades Over Time**

After 10–15 prompts, Copilot may **lose the coding standard context**. You should:

* Re-add a reference to the JSON/YAML file in your prompt
* Or re-open the file in your editor

**Best Practice:**

```python
# Refer to coding_standard.json for naming conventions
```

---

### **Inline Standard Without External File**

If you don’t want to use a JSON/YAML file, define standards inline:

**Prompt:**

```python
# Follow these coding standards:
# - Function names: camelCase
# - Class names: PascalCase
# - Constants: UPPER_CASE
# - Docstring: Google style
# Define a class for BankAccount with methods to deposit and withdraw
```

---

**Copilot Suggestion:**

```python
class BankAccount:
    """
    A class representing a bank account.
    """

    def __init__(self, initialBalance=0):
        self.balance = initialBalance

    def deposit(self, amount):
        """
        Deposit money into the account.

        Args:
            amount (float): The amount to deposit.
        """
        self.balance += amount

    def withdraw(self, amount):
        """
        Withdraw money from the account.

        Args:
            amount (float): The amount to withdraw.
        """
        self.balance -= amount
```

---

## **Summary Table**

| Feature             | Supported via Copilot Context          |
| ------------------- | -------------------------------------- |
| Function Naming     | ✅ (snake\_case, camelCase)             |
| Class Naming        | ✅ (PascalCase)                         |
| Constant Formatting | ✅ (UPPER\_CASE)                        |
| Docstring Style     | ✅ (Google, NumPy)                      |
| Reusability         | ⚠️ (Needs reminder every \~15 prompts) |
| File-based Standard | ✅ JSON/YAML                            |
| Learning from Past  | ❌ (Not persistent across sessions)     |


---

# **Lint Error Fixes with GitHub Copilot: Python and Angular Edition**


## **What Is Linting?**

Linting is a **static code analysis** technique used to identify code issues such as:

* Style violations (e.g., naming conventions)
* Missing documentation
* Unused variables
* Incorrect formatting
* Best practice violations

---

## **Section 1: Fixing Lint Errors in Python using GitHub Copilot**

### **Tools Used:**
* `pylint` (or `flake8`, `black`, `ruff`)
* GitHub Copilot Chat
* Install **Pylint**  

```bash
pip install pylint
```

---

### **Step-by-Step for Python**

#### **Step 1: Run the Linter**

**Sample.py**
```python
import subprocess

def run_pylint_on_sample():
    """Runs pylint on sample.py and prints the output."""
    result = subprocess.run(['pylint', 'sample.py'], capture_output=True, text=True)
    print(result.stdout)

if __name__ == "__main__":
    run_pylint_on_sample()
```


**Run python Script**

```bash
pylint sample.py
```

**Sample Output:**
![alt text](../images/img281.png)

---

#### **Step 2: Ask Copilot to Explain the Error**

**Prompt to Copilot Chat**:
```plain text
what does C0114: Missing module docstring (missing-module-docstring) mean?
```

**Copilot Suggestion**:
![alt text](../images/img282.png)


---

#### **Step 3: Ask Copilot to Fix the Issue**

**Prompt to Copilot Chat**:
```plain text
Fix all lint errors in this Python code:
```

**Example Before Fix:**

```python
num = 5

def say_hello():
    print("Hello")
```

**Copilot Output**:

```python
NUM = 5

def say_hello():
    """Prints a greeting message."""
    print("Hello")
```

---

#### **Step 4: Add Missing Docstrings**

**Prompt**:
```plain text
Add module-level and function-level docstrings to this code.
```

**Copilot Output**:

```python
"""This module provides a simple greeting example."""

NUM = 5

def say_hello():
    """Prints a greeting message."""
    print("Hello")
```

---

#### **Result:**

Run `pylint sample.py` again and verify that issues are resolved.

---

## **Section 2: Fixing Lint Errors in Angular using GitHub Copilot**

### **Tools Used:**

* `ng lint`
* GitHub Copilot Chat

---

### **Step-by-Step for Angular**

#### **Step 1: Run the Angular Linter**
**Sample.ts**
```ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-line-chart',
  templateUrl: './line-chart.component.html',
})
export class LineChartComponent implements OnInit {
  chart: any;

  createChart() {
    // chart logic
  }
}
```

**angular.json**
```json
{
  "$schema": "./node_modules/@angular/cli/lib/config/schema.json",
  "version": 1,
  "defaultProject": "demo-repo",
  "projects": {
    "demo-repo": {
      "projectType": "application",
      "root": "",
      "sourceRoot": "src",
      "architect": {
        "lint": {
          "builder": "@angular-eslint/builder:lint",
          "options": {
            "lintFilePatterns": [
              "src/**/*.ts",
              "src/**/*.html"
            ]
          }
        }
      }
    }
  },
  "cli": {
    "analytics": "07f2b5fe-c46b-4569-9c83-3ddafff5f2fe",
    "schematicCollections": [
      "angular-eslint"
    ]
  }
}
```

**Run the script**
```bash
ng lint
```


**Sample Output:**

![alt text](../images/img283.png)

![alt text](../images/img284.png)

---

#### **Step 2: Ask Copilot to Explain the Error**

**Prompt to Copilot Chat**:
```plain text
Explain why this Angular code reports "Missing lifecycle hook implementation: OnInit"
```

**Copilot Output**:
![alt text](../images/img285.png)

> This component declares that it implements `OnInit` but does not define the `ngOnInit()` method. You should implement `ngOnInit()` when using `OnInit`.

---

#### **Step 3: Ask Copilot to Fix the Issue**

**Example Before Fix:**

```ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-line-chart',
  templateUrl: './line-chart.component.html',
})
export class LineChartComponent implements OnInit {
  chart: any;

  createChart() {
    // chart logic
  }
}
```

**Prompt to Copilot Chat**:
```plain text
Fix all lint errors in this Angular code.
```


**Copilot Output**:

```ts
import { Component, OnInit } from '@angular/core';
import { Chart } from 'chart.js';

@Component({
  selector: 'app-line-chart',
  templateUrl: './line-chart.component.html',
})
export class LineChartComponent implements OnInit {
  public chart!: Chart;

  ngOnInit(): void {
    this.createChart();
  }

  createChart(): void {
    // chart logic
  }
}
```

---

### **Step 4: Rerun the Linter**

```bash
ng lint
```

**Expected Result:**
Errors drop from 24 to 18 or fewer. Repeat the Copilot-assisted fixes until you're lint-free!

---

## **Final Tips to Maximize Copilot for Lint Fixes**

| Action                           | Prompt Example                                         |
| -------------------------------- | ------------------------------------------------------ |
| Explain specific lint error      | "What does `Missing module docstring` mean?"           |
| Fix one issue                    | "Fix the naming convention issue in this code"         |
| Fix all lint issues              | "Fix all lint errors in this file"                     |
| Add docstrings                   | "Add function and module-level docstrings"             |
| Enforce strict typing in Angular | "Replace `any` with a specific type in this component" |


---

# **GitHub Copilot Review Guidelines**

## **Step 1: Install GitHub Copilot Extension in VS Code**

1. **Open VS Code:** Launch your Visual Studio Code application.
2. **Go to Extensions:** Click on the Extensions icon (🧩) on the left sidebar.
3. **Search for GitHub Copilot:** Type "GitHub Copilot" in the search bar.
4. **Install the Extension:** Click the "Install" button to add GitHub Copilot to your VS Code.

---

## **Step 2: Access GitHub Copilot Settings**

**1. Open Settings:** Go to `File > Preferences > Settings` or use the shortcut (`Ctrl + ,`).
**2. Search for Copilot Settings:** Type "GitHub Copilot" in the search bar.
**3. Open Copilot Settings:** You will see multiple settings for GitHub Copilot. Locate **"GitHub Copilot Chat Review Instructions"**.

---

## **Step 3: Set Up Review Guidelines**

* **Locate Review Settings:**

   * Go to **"GitHub Copilot Chat Review Selection Instructions."**
   * This is where you will define review guidelines for code.

* **Add Review Instructions:**

   * You can directly enter text instructions for Copilot to follow when reviewing code.
   * Example Review Instructions:

     * For **C# Code:**

       * Use **underscore `_`** for field names.
       * Use **XML documentation comments** for all public methods.
     * For **Python Code:**

       * Use **snake\_case** for function names.
       * Use **docstrings** for documentation.

* **Sample Settings Configuration:**

   ```plaintext
   - For C# code, use underscore for field names.
   - Use XML documentation for all public methods.
   - For Python code, use snake_case for function names.
   - Use docstrings for Python method documentation.
   ```
  ![alt text](../images/img129.png)

---

## **Step 4: Review Code with GitHub Copilot in VS Code**

* **Open a Code File:** Open any C# or Python file you want to review.
  ![alt text](../images/img130.png)

* **Select Code for Review:** Highlight the code segment you want Copilot to review.
  
* **Right-Click and Choose Review:**

   * Right-click the selected code.
   * Select **"Copilot: Review and Comment"**.
     ![alt text](../images/img131.png)

* **Review Suggestions:**
  ![alt text](../images/img132.png)

   * GitHub Copilot will analyze the code and provide review comments based on the instructions you set.
   * It can suggest:

     * Correcting naming conventions (snake\_case, underscore).
     * Adding missing documentation (XML for C#, docstring for Python).
     * Improving code structure.

---

## **Step 5: Apply or Discard Copilot Suggestions**

* **Review Each Suggestion:**
  ![alt text](../images/img133.png)

   * You can either **Accept, Modify, or Discard** each suggestion.
   
   ![alt text](../images/img134.png)
   * If a suggestion is helpful, click **"Accept"**.
   * If not, click **"Discard"**.

* **Multiple Suggestions Handling:**
   * If Copilot provides multiple suggestions, you can choose to apply them one by one or discard all.
     
---

## **Step 6: Customize Review Guidelines as Needed**

* **Refine Instructions:**

   * If the review instructions do not align with your expectations, return to the settings.
   * Add or modify the instructions based on your coding standards.

* **Advanced Settings:**

   * You can specify different instructions for different languages.
   * Example:

     * For **JavaScript:** Use camelCase for variable names.
     * For **Java:** Use PascalCase for class names.

---

## **Step 7: Review Python Code with Copilot**

* **Select Python Code:** Highlight a Python function or class.
* **Right-Click and Select "Review and Comment."**
* **Observe Suggestions:**

   * Function names should follow snake\_case.
   * Missing docstrings will be suggested.
   * Variable names can be optimized.

---

## **Step 8: Review C# Code with Copilot**

* **Select C# Code:** Highlight a C# method or class.
* **Right-Click and Select "Review and Comment."**
* **Observe Suggestions:**

   * Field names should use an underscore `_`.
   * Missing XML documentation will be suggested for public methods.
   * Inconsistent naming will be corrected.

---

## **Step 9: Save and Manage Your Review Settings**

* **Save Settings:** Ensure you save any changes you make in the GitHub Copilot settings.
* **Backup Settings:** Consider copying your review guidelines to a separate file for backup.

---

## **Step 10: Explore Advanced Review Scenarios**

* **Try Reviewing Commit Changes:**

   * In the Source Control panel, right-click a commit and select **"Review with Copilot."**
* **Review Multiple Files:**

   * You can also select multiple files or a folder and perform a bulk review.

---

## **Tips and Best Practices**

* Keep review guidelines consistent with your team’s coding standards.
* Regularly update the guidelines to match evolving best practices.
* Use clear and specific instructions for better Copilot review performance.
* Test your settings with different languages (Python, C#, JavaScript) to ensure they work as expected.

---

# **Build "She Inspires" Quote Generator**

Here's a **step-by-step guide** to help you recreate the **"She Inspires" Quote Generator Web App** using **HTML**, **JavaScript**, **Tailwind CSS**, and **GitHub Copilot Agent**.

---

### **Step 1: Prerequisites**

**Make sure you have the following installed:**

* [VS Code (Insiders version)](https://code.visualstudio.com/insiders/) for Copilot Agent
* [GitHub Copilot Chat](https://github.com/features/copilot) with agent mode enabled
* A basic understanding of HTML, JS, and Tailwind CSS

---

###  **Step 2: Set Up Your Project Folder**

**Create a folder:**

   ```bash
   mkdir shequote
   ```

---

### **Step 3: Enable GitHub Copilot Agent**

1. Open the **Copilot Chat sidebar**.
2. Click on **“Enable Agent Mode”**.
3. Type your prompt:

   ```
   Generate an HTML and JavaScript-based quote generator web app named 'She Inspires.' The app should have a visually stunning landing screen with an elegant UI, using Tailwind CSS for styling. It should fetch motivational quotes from an API featuring inspiring women in history, tech, and leadership. The app should also include: A beautiful landing page with animations. A 'Get Inspired' button that fetches and displays a new quote dynamically. A chatbot-like assistant that can share a daily quote and fun facts. Smooth UI transitions and responsiveness for mobile and desktop. A modern and clean design that enhances user experience. Ensure that the JavaScript fetches quotes from a public API, manages UI updates efficiently, and provides a delightful user experience with animations.
   ```

---

### 🧾 **Step 4: Let Copilot Generate the Core Files**

**Copilot Agent will automatically create:**

![alt text](../images/img78.png)

* `index.html` – for structure
* `app.js` – for quote fetching logic
* `style.css` – for additional styling if needed

> If it doesn't, create them manually and ask Copilot for help.

---

### **Step 5: Customize `index.html`**

**Here’s a basic structure you can use:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>She Inspires</title>
  <script src="app.js" defer></script>
  <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
</head>
<body class="bg-gradient-to-r from-pink-300 via-purple-300 to-indigo-400 text-center h-screen flex flex-col justify-center items-center">
  <h1 class="text-4xl font-bold mb-4">She Inspires 💫</h1>
  <button id="getQuote" class="bg-white text-black font-semibold px-6 py-2 rounded-full shadow-lg hover:bg-pink-200 transition">
    Get Inspired
  </button>
  <p id="quote" class="mt-6 max-w-xl text-lg font-medium italic"></p>
</body>
</html>
```

---

### **Step 6: Add Logic in `app.js`**

**Here’s a basic JS file to fetch and display quotes:**

```javascript
const quotes = [
  "The future belongs to those who believe in the beauty of their dreams. – Eleanor Roosevelt",
  "I have learned over the years that when one's mind is made up, this diminishes fear. – Rosa Parks",
  "Grace Hopper invented the first compiler and coined the term ‘debugging’.",
  "We need to accept that we won’t always make the right decisions. – Michelle Obama"
];

document.getElementById("getQuote").addEventListener("click", () => {
  const quote = quotes[Math.floor(Math.random() * quotes.length)];
  document.getElementById("quote").innerText = quote;
});
```

> You can replace this with an API call later. For now, it's offline-friendly.

---

### **Step 7: Animate the Background (Optional)**

**Ask Copilot:**

> “Add an animated rainbow gradient background to the body using Tailwind CSS or custom CSS.”

**Example with CSS:**

```css
/* style.css */
body {
  animation: gradientBG 10s ease infinite;
  background-size: 400% 400%;
}

@keyframes gradientBG {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}
```

**Add the link in your HTML:**

```html
<link rel="stylesheet" href="style.css">
```

---

### **Step 8: Debug and Enhance with Copilot Agent**

**Use prompts like:**

* “Fix error in app.js line 25”
* “Avoid repeating the same quote consecutively”
* “Use a quote API with fallback URLs”
* “Make the design responsive on mobile”

---

### **Step 9: Test Your App**

1. Open `index.html` in your browser.
2. Click **"Get Inspired"** and see the quote change.
3. Use Developer Tools (F12) to inspect and debug if needed.
   
![alt text](../images/img79.png)

---

### **Step 10: Optional Enhancements**

* Integrate a real quote API like [ZenQuotes](https://zenquotes.io/) or [TheySaidSo](https://theysaidso.com/)
* Add a "Share on Twitter" button
* Use localStorage to store favorite quotes

---

# **Build an Angular Market Share Dashboard with Self-Healing AI**

## **Introduction**

GitHub Copilot Agent Mode is an advanced feature of GitHub Copilot available in VS Code Insiders, providing a more interactive and autonomous coding experience. It not only generates code based on your requests but also iterates on the code, detects errors, and automatically fixes them. This guide will walk you through how to use Agent Mode in GitHub Copilot to build a complete Angular Market Share Dashboard.

---

## **Prerequisites**

* A GitHub account with Copilot enabled.
* VS Code Insiders installed on your system.
* Node.js and npm installed.
* Basic knowledge of Angular.

---

## **Step-by-Step Guide: Using GitHub Copilot Agent Mode**

### **Step 1: Enabling GitHub Copilot Agent Mode**

1. **Open VS Code Insiders** and ensure GitHub Copilot is enabled.
2. Go to **Settings (Ctrl + ,)**.
3. Search for **"Copilot Agent"**.
4. Toggle the option to **"Enable Agent Mode"**.

---

### **Step 2: Starting a New Angular Project**

1. **Open a Blank Folder** in VS Code Insiders.
2. Open the **Copilot Chat** by clicking the Copilot icon.
3. In the chat, ask:

   ```plaintext
   How do I create a new Angular application named "Market Dashboard"?
   ```
   ![alt text](../images/img108.png)

4. Copilot will guide you through creating a new Angular project:
   * It will ask for CSS or styling preferences.
   * It will handle the Angular project scaffolding.
     ![alt text](../images/img109.png)
     
   **Choose Stylesheet Format:**

   * Use the **arrow keys** on your keyboard to navigate the options.
   * Select **Sass (SCSS)** by pressing **Enter**.
     ![alt text](../images/img110.png)

   **Angular Project Creation:**

   * The Angular CLI will now generate the project files and install necessary packages.
   * You will see a series of installation messages, including:

     ```plaintext
     ✔ Packages installed successfully.
     ✔ Angular project "Market Dashboard" created successfully.
     ```  
     
     ![alt text](../images/img111.png)

5. Once the project is created:
     ![alt text](../images/img112.png)
   * Close the current folder.
   * Open the newly created Angular project folder.

---

### **Step 3: Installing Dependencies**

1. Open a **new terminal** in VS Code Insiders.
2. Run the following command to install Angular dependencies:

   ```bash
   npm install
   ```
   
3. Once installation is complete, start the Angular application:

   ```bash
   ng start
   ```
   ![alt text](../images/img113.png)

   ![alt text](../images/img114.png)

4. The application should open on `http://localhost:4200` with the default Angular welcome screen.

   ![alt text](../images/img115.png)
---

### **Step 4: Enabling GitHub Copilot Agent Mode for Development**

1. Ensure **Agent Mode** is enabled (refer to Step 1).
2. Go to the **Copilot Edits** section.
3. Switch to **"Agent Mode"**.
4. Ensure the **Cloud AI Model** is selected.
   ![alt text](../images/img116.png)

---

### **Step 5: Creating the Market Dashboard**

1. In Copilot Chat, provide the following prompt:

   ```plaintext
   Modify this Angular application to include a Market Dashboard that displays market share details using a pie chart and bar chart.
   ```
2. GitHub Copilot Agent will:
 
   * Install any necessary dependencies (e.g., `chart.js`).
   * Generate a `market-share.json` file with sample market data.
   * Create an Angular Service (`MarketDataService`) to fetch data from this file.
   * Create a `MarketDashboard` component to display the data.
   * Set up routing and add a navigation link to the dashboard.
     ![alt text](../images/img117.png)

---

## **Step 6: Reviewing the Generated Code**
   
![alt text](../images/img118.png)

1. Check the `market-share.json` file in the `src/assets` folder.
2. Review the `MarketDataService` service:

   * Located in the `src/app/services` folder.
   * Contains methods to fetch market data.
3. Review the `MarketDashboard` component:

   * Located in the `src/app/components` folder.
   * Includes logic for rendering the pie chart and bar chart.
4. Ensure the routing has been set up in `app-routing.module.ts` for the dashboard.
   
---

### **Step 7: Testing the Dashboard**

1. Restart the Angular server:

   ```bash
   ng serve --open
   ```
2. Navigate to the Market Dashboard using the link in the navigation bar.
3. Verify that:
   
     ![alt text](../images/img119.png)
   * The pie chart and bar chart display the market share data.
   * The charts are interactive and properly styled.

---

### **Step 8: Iterating and Fixing Errors with Agent Mode**

1. If any errors are detected (either in the code or the console), GitHub Copilot Agent will:

   * Automatically identify the error.
   * Suggest a fix or directly apply the fix.
2. You can continue to refine the dashboard by asking Copilot:

   ```plaintext
   Make the pie chart display percentage values for each segment.
   ```
3. Copilot will update the chart logic accordingly.
   ![alt text](../images/img120.png)

---

### **Step 9: Enhancing the Dashboard (Optional)**

* Add more data fields to `market-share.json`.
* Use Agent Mode to create an additional line chart for historical market trends.
* Customize the dashboard's layout and style using CSS.

---

## **Conclusion**

You have successfully used GitHub Copilot's Agent Mode to create and refine an Angular Market Share Dashboard. This powerful feature allows you to iterate, detect, and fix errors automatically, streamlining the development process.

---

# **Crafting a Webpage with HTML & CSS**

## **Introduction**

GitHub Copilot is an AI-powered tool that can help you quickly generate code. In this guide, we will explore how to use GitHub Copilot to create a professional webpage using HTML and CSS in Visual Studio Code (VS Code).

---

## **Prerequisites**

* Install Visual Studio Code (VS Code).
* Install GitHub Copilot extension in VS Code.
* Ensure you have a GitHub account linked to Copilot.

---

## **Understanding the Workflow**

* We will create a simple landing page using HTML and CSS.
* We will use GitHub Copilot to generate the HTML structure and CSS styling.
* We will refine the page layout using Copilot suggestions.

---

## **Step 1: Setting Up Your Project**

1. Open VS Code.
2. Create a new folder for your project.
3. Inside this folder, create two files:

   * `index.html` (for HTML structure)
   * `styles.css` (for CSS styling)

---

## **Step 2: Generating HTML Structure**

1. Open `index.html`.
2. Type the following prompt for Copilot:
   **Prompt:** "Create a simple landing page with a navbar, a hero section with text on the left and an image on the right."
   ![alt text](../images/img181.png)

3. Review the generated HTML and make adjustments as needed.

### **Example HTML Code:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Landing Page</title>
    <style>
        body {
            margin: 0;
            font-family: Arial, sans-serif;
            background: #f9f9f9;
        }
        .navbar {
            background: #222;
            color: #fff;
            padding: 1rem 2rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .navbar .logo {
            font-weight: bold;
            font-size: 1.5rem;
        }
        .navbar nav a {
            color: #fff;
            text-decoration: none;
            margin-left: 1.5rem;
            font-size: 1rem;
        }
        .hero {
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 4rem 2rem;
            background: #fff;
        }
        .hero-content {
            flex: 1;
            max-width: 500px;
        }
        .hero-content h1 {
            font-size: 2.5rem;
            margin-bottom: 1rem;
        }
        .hero-content p {
            font-size: 1.2rem;
            margin-bottom: 2rem;
            color: #555;
        }
        .hero-content button {
            padding: 0.75rem 2rem;
            font-size: 1rem;
            background: #0078d4;
            color: #fff;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        .hero-image {
            flex: 1;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        .hero-image img {
            max-width: 100%;
            height: auto;
            border-radius: 12px;
            box-shadow: 0 4px 24px rgba(0,0,0,0.08);
        }
        @media (max-width: 900px) {
            .hero {
                flex-direction: column;
                text-align: center;
            }
            .hero-image {
                margin-top: 2rem;
            }
        }
    </style>
</head>
<body>
    <div class="navbar">
        <div class="logo">MyBrand</div>
        <nav>
            <a href="#">Home</a>
            <a href="#">Features</a>
            <a href="#">Contact</a>
        </nav>
    </div>
    <section class="hero">
        <div class="hero-content">
            <h1>Welcome to My Landing Page</h1>
            <p>Discover our amazing product that helps you achieve more with less effort. Start your journey with us today!</p>
            <button>Get Started</button>
        </div>
        <div class="hero-image">
            <img src="https://images.unsplash.com/photo-1506744038136-46273834b3fb?auto=format&fit=crop&w=600&q=80" alt="Hero Image">
        </div>
    </section>
</body>
</html>
```

---

## **Step 3: Generating CSS Styles**

1. Open `styles.css`.
2. Use the following prompt:
   **Prompt:** "Style the landing page with a dark purple and black theme. Align text on the left and image on the right."
   ![alt text](../images/img182.png)

3. Review the CSS generated by Copilot.


### **Example CSS Code:**

```css
body {
    font-family: Arial, sans-serif;
    background-color: #1a1a2e; /* deep dark purple */
    color: #fff;
    margin: 0;
    padding: 0;
}

nav ul {
    list-style-type: none;
    display: flex;
    justify-content: flex-start;
    gap: 30px;
    padding: 18px 40px;
    background-color: #0f0f3d; /* darker purple/black */
    margin: 0;
}

nav ul li a {
    color: #fff;
    text-decoration: none;
    font-weight: 600;
    font-size: 1.1rem;
    transition: color 0.2s;
}

nav ul li a:hover {
    color: #a259ff; /* accent purple */
}

.hero {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 60px 40px;
    background: linear-gradient(90deg, #1a1a2e 60%, #23234d 100%);
    min-height: 70vh;
}

.hero .text {
    max-width: 48%;
    text-align: left;
}

.hero .text h1 {
    font-size: 2.8rem;
    margin-bottom: 18px;
    color: #a259ff;
}

.hero .text p {
    font-size: 1.25rem;
    margin-bottom: 28px;
    color: #e0e0e0;
}

.hero .text button {
    background: #a259ff;
    color: #fff;
    border: none;
    padding: 14px 36px;
    border-radius: 6px;
    font-size: 1.1rem;
    font-weight: bold;
    cursor: pointer;
    transition: background 0.2s;
}

.hero .text button:hover {
    background: #6c2bd7;
}

.hero .image {
    flex: 1;
    display: flex;
    justify-content: flex-end;
    align-items: center;
}

.hero .image img {
    max-width: 400px;
    width: 100%;
    border-radius: 16px;
    box-shadow: 0 8px 32px rgba(30, 20, 60, 0.4);
}

@media (max-width: 900px) {
    .hero {
        flex-direction: column;
        padding: 40px 10px;
        text-align: center;
    }
    .hero .text, .hero .image {
        max-width: 100%;
    }
    .hero .image {
        justify-content: center;
        margin-top: 30px;
    }
    .hero .image img {
        max-width: 90vw;
    }
}
```

---

## **Step 4: Testing and Refining the Page**

1. Open the `index.html` file in your browser.
   ![alt text](../images/img183.png)

2. Make sure the layout looks as expected.
3. If needed, use Copilot to further refine the layout and design.

--- 

## **Conclusion**

GitHub Copilot can significantly speed up the process of building web pages. With the right prompts, you can create, style, and enhance your webpage efficiently. Use it as a coding assistant to build better designs.

---

# **Building a Sales Dashboard using GitHub Copilot in VS Code**

## **Introduction**

In this guide, we will explore how to leverage GitHub Copilot in VS Code to quickly build a sales dashboard using Python and Matplotlib. You will learn how to efficiently generate code suggestions with Copilot, fix any issues, and iteratively build a complete dashboard.

---

## **Prerequisites**

* Visual Studio Code (VS Code) installed.
* GitHub Copilot extension set up and enabled in VS Code.
* Basic knowledge of Python and data visualization using Matplotlib.
* A CSV file named `data.csv` containing sales data with columns: `Product`, `Sales`, `Region`, `Sales_Rep`, `Customer_Type`, and `Date`.

---

## **Step 1: Project Setup**

1. Create a new folder for your project and open it in VS Code.
2. Save your CSV file (`data.csv`) inside this folder.
3. Create a new Python file named `dashboard.py`.

---

## **Step 2: Reading the Sales Data**

1. Start by importing the necessary libraries:

   ```python
   import pandas as pd
   import matplotlib.pyplot as plt
   ```

2. Use GitHub Copilot to write a function to read the CSV data:

   * **Prompt:** "Write a function to read a CSV file named 'data.csv' and display the first 5 rows."

3. Copilot will suggest the following code:

   ![alt text](../images/img213.png)

4. Save and run the code using the terminal:

   ```bash
   python dashboard.py
   ```
   ![alt text](../images/img214.png)

---

## **Step 3: Generating the First Chart (Bar Chart)**

1. Prompt Copilot:

   * **Prompt:** "Write a function to create a bar chart showing total sales by product."

2. Copilot may generate the following code:

   ```python
   def plot_sales_by_product(data):
       product_sales = data.groupby('Product')['Sales'].sum()
       product_sales.plot(kind='bar')
       plt.title('Total Sales by Product')
       plt.show()

   plot_sales_by_product(data)
   ```
   ![alt text](../images/img215.png)

3. Adjust the column name if it doesn't match your CSV data.

---

## **Step 4: Adding a Pie Chart (Sales by Region)**

1. Use Copilot again:

   * **Prompt:** "Add a function to create a pie chart showing sales distribution by region."

2. It will suggest:

   ```python
   def plot_sales_by_region(data):
       region_sales = data.groupby('Region')['Sales'].sum()
       region_sales.plot(kind='pie', autopct='%1.1f%%')
       plt.title('Sales Distribution by Region')
       plt.show()

   plot_sales_by_region(data)
   ```

3. Test and ensure it displays correctly.

---

## **Step 5: Creating a Dashboard Layout**

1. Combine the charts in a single dashboard.

   * **Prompt:** "Create a function to display the bar chart and pie chart side by side in one figure."

2. Copilot should suggest a layout using `plt.subplot()`:

   ```python
   def create_dashboard(data):
       plt.figure(figsize=(12, 6))

       plt.subplot(1, 2, 1)
       product_sales = data.groupby('Product')['Sales'].sum()
       product_sales.plot(kind='bar')
       plt.title('Total Sales by Product')

       plt.subplot(1, 2, 2)
       region_sales = data.groupby('Region')['Sales'].sum()
       region_sales.plot(kind='pie', autopct='%1.1f%%')
       plt.title('Sales Distribution by Region')

       plt.tight_layout()
       plt.show()

   create_dashboard(data)
   ```
   ![alt text](../images/img216.png)

---

## **Step 6: Adding More Charts**

1. Add a horizontal bar chart for sales by representative.
2. Add a dual-axis chart for new and returning customers.
3. Add a line chart for daily sales trends.

---

## **Step 7: Finalizing the Dashboard**

1. Adjust the layout to display all charts in a neat grid.
2. Test the code and fix any column name issues suggested by Copilot.
   
---

## **Conclusion**

In this guide, we learned how to quickly build a sales dashboard using GitHub Copilot in VS Code, leveraging its code generation capabilities to create, fix, and organize multiple charts.

---

# **GitHub Copilot and Generative AI**

## **Introduction**

GitHub Copilot and other generative AI tools are transforming how developers write, debug, and optimize code. This guide provides a detailed, step-by-step walkthrough of how these tools enhance the developer workflow, boost productivity, and foster innovation.

---

## **1. Understanding GitHub Copilot and Generative AI**

* **What is GitHub Copilot?**

  * A code completion tool powered by OpenAI’s Codex model.
  * Assists developers in writing code by providing real-time code suggestions.

* **What is Generative AI?**

  * A type of AI that can generate text, code, images, and more.
  * Uses large-scale neural networks to understand and generate complex content.

---

## **2. Setting Up GitHub Copilot**

* **Prerequisites:**

  * A GitHub account (Free or Pro).
  * A compatible IDE (VS Code, JetBrains, GitHub Codespaces).

* **Step-by-Step Setup:**

  1. Go to the GitHub Copilot page on GitHub.
  2. Enable GitHub Copilot for your account.
  3. Install the GitHub Copilot extension in your IDE.
  4. Log in with your GitHub credentials.

---

## **3. How GitHub Copilot Works**

* **Contextual Code Suggestions:**

  * Copilot analyzes your code context and provides inline suggestions.
  * Supports multiple languages (Python, JavaScript, TypeScript, Java, etc.).

* **Multi-Line Completions:**

  * Offers complete function implementations based on the code comments or function signatures.

* **Comment-Based Commands:**

  * Developers can use comments to describe the desired functionality, and Copilot will generate the code.

---

## **4. Practical Use Cases of GitHub Copilot**

* **1. Code Completion:**

  * Automatically completes lines of code.

* **2. Code Refactoring:**

  * Recommends optimized versions of existing code.

* **3. Code Documentation:**

  * Generates docstrings for functions and classes.

* **4. Error Fixing:**

  * Identifies and suggests fixes for code errors.

---

## **5. Advanced Usage: Customizing Copilot**

* **Personalized Suggestions:**

  * GitHub Copilot can learn from your code style and provide more personalized suggestions.

* **Configuring Copilot Settings:**

  * Adjust Copilot’s behavior in the IDE settings (enable/disable, change suggestion frequency).

---

## **6. Exploring Other Generative AI Tools**

* **GitHub Codespaces with Copilot:**

  * A cloud-based IDE with built-in Copilot support.

* **Copilot Chat (Preview Feature):**

  * A conversational AI assistant integrated into the IDE for code explanations and debugging.

---

## **7. The Future of Developer Ecosystem with Generative AI**

* Enhanced developer productivity.
* Automated code generation and optimization.
* Natural language code interaction (Copilot Chat).

---

## **Conclusion**

GitHub Copilot and generative AI are revolutionizing the developer ecosystem by enhancing code generation, improving efficiency, and enabling faster problem-solving. This guide provides a foundational understanding for developers to leverage these tools effectively.

---

# **Journey with AI Companion**

## **Introduction**

GitHub Copilot is an AI-powered code completion tool developed by GitHub in collaboration with OpenAI. It helps developers write code faster by providing code suggestions directly in your code editor. This guide will walk you through everything you need to know to effectively use GitHub Copilot.

---

## **Prerequisites**

* A GitHub account
* An active GitHub Copilot subscription
* Visual Studio Code (VS Code) installed (or any supported IDE)

---

## **Step 1: Setting Up GitHub Copilot**

1. **Sign in to GitHub**

   * Go to [GitHub.com](https://github.com) and log in.

2. **Activate GitHub Copilot Subscription**

   * Go to your account settings.
   * Under "Billing and plans," ensure that you have an active Copilot subscription.

3. **Install GitHub Copilot Extension in VS Code**

   * Open VS Code.
   * Go to the Extensions tab (Ctrl+Shift+X).
   * Search for "GitHub Copilot" and click "Install."

4. **Sign in to GitHub from VS Code**

   * Click on the GitHub Copilot icon in the lower right.
   * Sign in using your GitHub account.

---

## **Step 2: Basic Usage of GitHub Copilot**

### **Generating Code Suggestions**

* Start typing any code or function name, and Copilot will automatically provide suggestions.
* Use `Tab` to accept the suggestion.

### **Example:**

* Type `def calculate_area` and GitHub Copilot will automatically suggest the full function code:

```python
# Start typing
def calculate_area(radius):
```

**Copilot Suggestion:**

![alt text](../images/img217.png)

---

## **Step 3: Using Prompts Effectively**

### **Writing Natural Language Prompts**

* GitHub Copilot can understand natural language comments.
* Example:

```python
# Write a function to calculate factorial of a number
```

**Copilot Suggestion:**

![alt text](../images/img218.png)

```python
def factorial(n):
    if n == 0 or n == 1:
        return 1
    else:
        return n * factorial(n - 1)
```

### **Advanced Prompting**

* Use more descriptive prompts for complex tasks.
* Example:

```python
# Write a Python function that connects to a MongoDB database and retrieves all documents from a collection
```

**Copilot Suggestion:**

![alt text](../images/img219.png)
```python
from pymongo import MongoClient

def fetch_all_documents():
    client = MongoClient("mongodb://localhost:27017")
    db = client["mydatabase"]
    collection = db["mycollection"]
    return list(collection.find())
```

---

## **Step 4: Leveraging Copilot in Different Programming Languages**

* GitHub Copilot is not limited to Python. You can use it with multiple languages like JavaScript, Java, C++, and more.

### **JavaScript Example:**

```javascript
// Create a function to check if a number is even
```

**Copilot Suggestion:**

```javascript
function isEven(num) {
    return num % 2 === 0;
}
```

---

## **Step 5: Debugging with Copilot**

* If a code snippet is not working, use natural language to guide Copilot.
* Example:

```python
# Fix the following function to handle negative inputs
```

**Copilot will suggest an improved version of the function**
![alt text](../images/img220.png)

---

## **Step 6: Copilot Configuration and Settings**

* Go to VS Code settings (File > Preferences > Settings).
* Search for "GitHub Copilot."
* You can configure options like:

  * Enabling or disabling Copilot.
  * Setting Copilot to automatically show suggestions or wait for manual triggers.

---

## **Step 7: Best Practices for Using GitHub Copilot**

* Use clear and descriptive comments for better suggestions.
* Regularly review Copilot’s code to ensure accuracy.
* Use Copilot’s inline suggestions for speed, but always verify the logic.

---

## **Conclusion**

GitHub Copilot can significantly enhance your coding speed and efficiency. By using the right prompts and understanding how Copilot works, you can leverage its full potential.

---

# **GitHub Copilot Coding Agent**

This guide will help you **get started** with the GitHub Copilot Coding Agent, showing you how to assign tasks, review PRs, and iterate with Copilot as your AI teammate.

---

## **What is Copilot Coding Agent?**

The **Copilot Coding Agent** is an AI-powered teammate that:
- Takes issues or tasks from your repo.
- Implements changes (code, docs, tests).
- Raises Pull Requests (PRs).
- Responds to PR review comments and feedback.
- Revises code as requested.

It works from **natural language instructions** and is available for **Copilot Enterprise** and **Copilot Pro+** users.

---

## **Prerequisites**

- **GitHub Copilot Enterprise** or **Copilot Pro+** subscription.
- Repository hosted on **GitHub.com**.
- Permissions to create issues and review PRs.
- (Recommended) Branch protection rules enabled.

---

## **How It Works**

1. **Create an Issue**: Describe your task in plain English.
2. **Assign to Copilot Agent**: Assign the issue to the Copilot Agent bot.
3. **Copilot Plans & Implements**: Copilot analyzes, plans, and makes changes.
4. **PR Creation**: Copilot opens a draft PR with the changes.
5. **Review & Feedback**: You review, comment, or request changes.
6. **Copilot Revises**: Copilot updates the PR as per your feedback.
7. **Merge**: You merge the PR when satisfied.

---

## **Step-by-Step Example Workflow**

### **1. Create an Issue**

**Prompt Example:**
```
Title: Update README with Contributor and License sections

Body:
Please update the README file to include:
- A "Contributors" section listing all contributors.
- A "License" section with MIT License details.
```

### **2. Assign the Issue to Copilot Agent**

- Assign the issue to `@github-copilot-agent` (or the Copilot bot for your org).

### **3. Copilot Plans & Implements**

**What Copilot Does:**
- Reads your repo and current README.
- Checks for existing LICENSE or CONTRIBUTORS files.
- Plans the steps (e.g., add sections, avoid duplication).
- Updates the README as requested.

**Copilot Suggestion Example:**
> "I will add a Contributors section and a License section to the README. If a LICENSE file exists, I will link to it."

### **4. Copilot Opens a Draft PR**

- PR is created in **draft mode** (prevents triggering workflows).
- PR links to the original issue and includes a summary of changes.

**PR Example:**
```
Files changed:
- README.md: Added Contributors and License sections.
```

### **5. Review & Request Changes**

- Review the PR.
- If you want changes, comment on the PR.

**Prompt Example:**
```
Please move the License section into a separate LICENSE file and link to it from the README.
```

### **6. Copilot Revises**

- Copilot reads your feedback.
- Creates a LICENSE file with MIT License.
- Updates README to link to LICENSE file.
- Updates the PR with new commits.

**Copilot Suggestion Example:**
> "Created LICENSE file with MIT License. Updated README to link to LICENSE."

### **7. Merge the PR**

- Once satisfied, approve and merge the PR.

---

## **Usage Cost**

- **GitHub Actions minutes**: Used for automation.
- **Premium requests**: Counted against your Copilot quota (from June 4, 2025).

---

## **Risks & Mitigations**

| Risk                    | Mitigation                                   |
|-------------------------|----------------------------------------------|
| Unwanted code push      | Use branch protection rules                  |
| Code exposure           | Restrict agent’s internet access via firewall|
| Prompt injection        | GitHub sanitizes user input                  |

---

## **Limitations**

- Works only in the repository where the issue exists.
- Only one PR at a time per repo.
- Cannot work on existing PRs (must start from issues).
- Branches from `main` only.
- No commit signing.
- No support for self-hosted runners.
- Content exclusion not honored (agent sees all files).

---

## **Best Practices**

- **Write clear, actionable issues.**
- **Review all PRs** from Copilot before merging.
- **Use branch protection** to control merges.
- **Label issues** for better organization.
- **Monitor usage costs** if running many tasks.

---

## **Summary**

The **GitHub Copilot Coding Agent** automates routine coding tasks, creates PRs, and responds to feedback—acting as an AI teammate. Use it to streamline development, but always review its output for quality and security.

---

# **Kubernetes Automation with Generative AI using GitHub Copilot**

## **Introduction**
This guide provides a detailed step-by-step approach to automating Kubernetes tasks using GitHub Copilot, leveraging its generative AI capabilities. You will learn how to effectively use Copilot for CLI commands, Kubernetes cluster setup, configuration management, CI/CD, and more.

---

## **Prerequisites**
* Basic knowledge of Kubernetes and DevOps tools.
* Installed GitHub Copilot in VS Code (Visual Studio Code).
* Access to a Kubernetes cluster (local or cloud-based).
* GitHub CLI installed and authenticated (`gh` command available).

---

## **Understanding Kubernetes Automation**

### **What is Kubernetes and Why Automate It?**
Kubernetes is an open-source platform for automating the deployment, scaling, and management of containerized applications. Automation in Kubernetes helps reduce manual effort, minimize errors, and ensure consistent and reliable deployments.


### **Understanding GitHub Copilot and Its Role in Automation**
GitHub Copilot is an AI-powered code completion tool that assists with code generation, command-line tasks, and automation scripts. When integrated with Kubernetes, it can generate YAML configurations, Helm charts, and Kubernetes commands, reducing manual scripting time.


### **How GitHub Copilot Works with CLI and Code Editors**
* In CLI: Copilot can be used to generate commands, scripts, and configuration files directly from the terminal.
* In Code Editors: Copilot provides code completions, suggestions, and automated configurations.
  

### **Why Use GitHub Copilot for Kubernetes Management?**
* Automate repetitive tasks.
* Generate complex Kubernetes configurations instantly.
* Minimize human errors in scripting.
* Enhance productivity with smart AI suggestions.


### **Best Practices for Effective Prompting with GitHub Copilot**
* Use clear and precise prompts for accurate suggestions.
* Experiment with variations in prompts for better results.
* Always review and test the generated commands.

---

## **1. Setting Up GitHub Copilot**
1. Open VS Code.
2. Install the GitHub Copilot extension from the Extensions Marketplace.
3. Sign in with your GitHub account.
4. Enable Copilot for VS Code and CLI.

### **Example Prompt:**

* **Prompt:** "How do I enable GitHub Copilot in CLI?"
  
* **Copilot Suggestion:** "Use the command: `gh extension install github/copilot-cli`"
  ![alt text](../images/img242.png)

---

## **2. Using GitHub Copilot in CLI (Command Line Interface)**
GitHub Copilot can generate and assist with Kubernetes commands directly from the terminal.

### **Example: Generating a Kubernetes Deployment Command**
1. Run the command:

   ```bash
   gh copilot ask "Generate a Kubernetes deployment command for a Node.js application."
   ```
2. Copilot Suggestion:

   ![alt text](../images/img243.png)

### **Advanced Usage - Revising Commands**
* **Prompt:** "Revise the command to include resource limits."
* **Copilot Suggestion:**
  ![alt text](../images/img244.png)

---

## **3. Automating Kubernetes Cluster Setup**
* Use Copilot to generate cluster setup scripts, manage nodes, and set up networking.

### **Example: Cluster Setup with Minikube**
* **Prompt:** "Generate a script to set up a Minikube cluster with 3 nodes."
  
* **Copilot Suggestion:**
   ![alt text](../images/img245.png)

---

## **4. Configuration Management with Helm and Customization**
* Use Helm for Kubernetes configuration management and Copilot for Helm chart automation.

### **Example: Creating a Helm Chart**
* **Prompt:** "Generate a Helm chart for a MySQL database."

* **opilot Suggestion:**
   ![alt text](../images/img246.png)

### **Customizing Helm Values**
* **Prompt:** "Customize Helm values for persistent storage."
* **Copilot Suggestion:**
  ![alt text](../images/img247.png)

---

## **5. CI/CD Automation using Jenkins and GitHub Actions**
* Automate Kubernetes deployments with Jenkins and GitHub Actions.

### **Example: Jenkins Configuration**
* **Prompt:** "Generate a Jenkinsfile for Kubernetes deployment."
* **Copilot Suggestion:**

   ```groovy
   pipeline {
     agent any
     stages {
       stage('Deploy') {
         steps {
           sh 'kubectl apply -f deployment.yaml'
         }
       }
     }
   }
   ```

---

### **GitHub Actions Workflow**
* **Prompt:** "Generate a GitHub Actions workflow for Kubernetes deployment."
* **Copilot Suggestion:**

  ```yaml
  name: Deploy to Kubernetes

  on: [push]

  jobs:
    deploy:
      runs-on: ubuntu-latest
      steps:
        - name: Set up Kubernetes
          run: kubectl apply -f deployment.yaml
  ```

---

## **6. Advanced Tips**

### **Using Workspace Context**
* Prompt: "Generate Helm charts for all microservices in my project."
* Copilot can read your project structure and suggest Helm charts for each service.


### **Multi-Language Support**
* **Prompt:** "Generate a Kubernetes deployment in my local language (e.g., Malayalam)."
* **Copilot** can provide commands and explanations in your preferred language.


### **Best Practices**
* Always review and test commands suggested by Copilot.
* Use precise prompts for accurate suggestions.
* Regularly update GitHub Copilot for the latest features.

---

## **Conclusion**
GitHub Copilot is a powerful tool for automating Kubernetes tasks. With its AI-driven suggestions, you can accelerate your workflow, minimize errors, and focus on higher-level tasks. Experiment with different prompts to unlock its full potential.

---

# **GitHub Copilot Billing 6 Nov 2023**

Here's a **step-by-step detailed guide with tables** summarizing the **GitHub Copilot billing model** as of **November 6, 2023**, covering both **Individual** and **Business (Enterprise)** tiers, proration, seat management, and Azure billing integration.

---

## **GitHub Copilot SKUs (Licensing Options)**

| SKU                    | Target Audience     | Monthly Cost    | Trial               | Notes                                                       |
| ---------------------- | ------------------- | --------------- | ------------------- | ----------------------------------------------------------- |
| **Copilot Individual** | Individual users    | \$10/month      | ✅ 30-day free trial | License is tied to the GitHub handle                        |
| **Copilot Business**   | Organizations/Teams | \$19/user/month | ❌                   | Centralized management, policy control, enterprise features |

---

## **Copilot Individual Billing**

### **Features:**

* \$10/month per user
* 30-day free trial
* Can enable/disable usage of your code for model training
* License is bound to the **GitHub account**
* Simple billing through **github.com billing settings**

---

## **Copilot for Business – Key Features**

| Feature                   | Description                                                      |
| ------------------------- | ---------------------------------------------------------------- |
| Centralized Management    | Admins can manage licenses, billing, and settings from one place |
| Policy Settings           | Control access, public code blocking, team enablement, etc.      |
| License Flexibility       | Add/remove users monthly                                         |
| Usage Reporting           | See who uses Copilot and how often                               |
| Azure Billing Integration | Option to link GitHub Copilot billing to Azure subscription      |

---

## **Copilot Business Billing Example**

![alt text](../images/img286.png)


| Scenario                           | Action Date | Billing Impact                                         |
| ---------------------------------- | ----------- | ------------------------------------------------------ |
| User added mid-month (e.g. 27th)   | Nov 27      | Charged **prorated** from 27th to end of billing cycle |
| Billing cycle date                 | e.g. 15th   | Recurring monthly billing on 15th                      |
| User removed mid-month (e.g. 30th) | Nov 30      | Still billed **full month** until next cycle (Dec 15)  |

---

## **Proration Explained**

| Date Action           | Date of Billing Cycle Start | Billed Period   | Billed Amount                     |
| --------------------- | --------------------------- | --------------- | --------------------------------- |
| Add user on Nov 27    | Nov 15                      | Nov 27 – Dec 15 | Prorated amount (approx. 19 days) |
| Remove user on Nov 30 | Nov 15                      | Nov 15 – Dec 15 | Full month billed (no proration)  |

> 📝 **Note:** Always remove users before the billing date to avoid unnecessary charges.

---

## **Seat Management Best Practices**

| Strategy                               | Why It's Useful                                                    |
| -------------------------------------- | ------------------------------------------------------------------ |
| Monitor usage monthly                  | Optimize license usage                                             |
| Remove unused users before billing day | Avoid full-month charge on removals                                |
| Reassign seats strategically           | Save cost during non-coding project phases (e.g., design, release) |

---

## **Azure Subscription Integration**

![alt text](../images/img287.png)

### **Use Case:**

* Manage GitHub Copilot billing via **Azure Billing Portal**
* Treat Copilot as another Azure resource

### **Billing Cycle Change:**

![alt text](../images/img288.png)

| Action                              | Old Billing Cycle | New Billing Cycle                       |
| ----------------------------------- | ----------------- | --------------------------------------- |
| Link Copilot to Azure on 2nd        | 15th–15th monthly | **1st–1st monthly** (Azure default)     |
| Billing shifts from GitHub to Azure | After linking     | Reflected as a resource in Azure portal |

---

## **Summary Table: GitHub Copilot Billing**

| Feature               | Individual      | Business              |
| --------------------- | --------------- | --------------------- |
| Monthly Cost          | \$10            | \$19 per user         |
| Trial                 | 30 days         | ❌                     |
| License Scope         | Per GitHub user | Per organization      |
| Billing Method        | GitHub billing  | GitHub or Azure       |
| Billing Model         | Fixed           | Usage-based, prorated |
| Central Management    | ❌               | ✅                     |
| Policy Controls       | Limited         | Full admin control    |
| Code Training Opt-Out | ✅               | Admin controlled      |

---

## **Recommendations for Businesses**

1. **Monitor Usage:** Use centralized reports to track actual usage.
2. **Plan Removals:** Remove users just before the billing date to avoid paying full month.
3. **Azure Billing:** Use Azure for unified cost tracking if you already use Microsoft services.
4. **License Right-Sizing:** Adjust license counts monthly based on project needs.

