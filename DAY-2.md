# Github copilot agenda - Day 2

## Test-Driven Development (TDD)

* [Python Unit Testing with Pytest](#python-unit-testing-with-pytest)
* [Master Test-Driven Development with GitHub Copilot](#master-test-driven-development-with-github-copilot)
* [GitHub Copilot Chat for Unit Test Generation](#github-copilot-chat-for-unit-test-generation)
* [Unit Test Generation using GitHub Copilot - JUnit, Mockito & Java](#unit-test-generation-using-github-copilot---junit-mockito--java)
* [Unit Test Generation using GitHub Copilot - XUnit & .NET Core](#unit-test-generation-using-github-copilot---xunit--net-core)


## Competitive Tools

* [Kubernetes Automation with Generative AI using GitHub Copilot](#kubernetes-automation-with-generative-ai-using-github-copilot)
* [GitHub Copilot and Generative AI](#github-copilot-and-generative-ai)


## Prompt-Based Code Generation

* [GitHub Copilot Can Now Read URLs! Paste Links & Get Smarter Code](#github-copilot-can-now-read-urls-paste-links--get-smarter-code)
* [Setting Up GitHub Copilot with Custom Coding Instructions](#setting-up-github-copilot-with-custom-coding-instructions)
* [Crafting a Webpage with HTML & CSS](#crafting-a-webpage-with-html--css)
* [GitHub Copilot Chat for New Projects](#github-copilot-chat-for-new-projects)

---

# **Python Unit Testing with Pytest**

## **Step 1: Initial Setup**

1. **Create Project Structure:**
   ```powershell
   mkdir python_unit_testing_project
   cd python_unit_testing_project
   ```

2. **Create Virtual Environment:**
   ```powershell
   python -m venv venv
   .\venv\Scripts\activate  # Windows
   ```

3. **Install Required Packages:**
   ```powershell
   pip install pytest
   ```

4. **Create Project Files and Directories:**
   ```
   python_unit_testing_project/
   ├── src/
   │   ├── __init__.py     # Empty file
   │   └── calculator.py
   ├── tests/
   │   ├── __init__.py     # Empty file
   │   ├── conftest.py     # For test configuration
   │   └── test_calculator.py
   └── setup.py            # For package installation
   ```

## **Step 2: Configure Python Package**

1. **Create `setup.py`:**
   ```python
   # setup.py
   from setuptools import setup, find_packages

   setup(
       name="calculator",
       packages=find_packages(),
       version="0.1",
       install_requires=[],
   )
   ```

2. **Create `conftest.py`:**
   ```python
   # tests/conftest.py
   import os
   import sys
   sys.path.insert(0, os.path.abspath(os.path.join(os.path.dirname(__file__), '..')))
   ```

3. **Install Package in Development Mode:**
   ```powershell
   pip install -e .
   ```

## **Step 3: Create Calculator Class**

```python
# src/calculator.py
class Calculator:
    def add(self, a: float, b: float) -> float:
        return a + b

    def subtract(self, a: float, b: float) -> float:
        return a - b

    def multiply(self, a: float, b: float) -> float:
        return a * b

    def divide(self, a: float, b: float) -> float:
        if b == 0:
            raise ValueError("Cannot divide by zero.")
        return a / b
```

## **Step 4: Write Test Cases**

```python
# tests/test_calculator.py
import pytest
from src.calculator import Calculator

calc = Calculator()

def test_add():
    assert calc.add(2, 3) == 5
    assert calc.add(-1, 1) == 0
    assert calc.add(0, 0) == 0

def test_subtract():
    assert calc.subtract(5, 3) == 2
    assert calc.subtract(0, 1) == -1

def test_multiply():
    assert calc.multiply(2, 3) == 6
    assert calc.multiply(5, 0) == 0

def test_divide():
    assert calc.divide(6, 2) == 3
    with pytest.raises(ValueError):
        calc.divide(5, 0)
```

---

## **Step 5: Run Tests**

1. **Set Up Test Framework with Copilot:**
   - Open Copilot Chat in your workspace
   - Type the following command:
     ```
     @workspace/setupTests
     ```
   - Select **pytest** from the available testing frameworks
   ![alt text](/images/img135.png)

   - Follow the configuration wizard to set up the test framework:
   ![alt text](/images/img136.png)

2. **Run from Project Root:**
   ```powershell
   cd python_unit_testing_project
   pytest
   ```

3. **Expected Output:**
   ![alt text](/images/img137.png)

---

## **Step 6: Additional Testing Features**

1. **Run with Verbose Output:**
   ```powershell
   pytest -v
   ```

2. **Run Specific Test:**
   ```powershell
   pytest tests/test_calculator.py::test_add
   ```

3. **Show Print Statements:**
   ```powershell
   pytest -s
   ```

---

### **Troubleshooting**

If you encounter import errors:
1. Verify all `__init__.py` files exist
2. Ensure you're running pytest from project root
3. Confirm package is installed with `pip list`
4. Check virtual environment is activated


> **Note:** Always run tests from the project root directory to avoid import issues.


---

## **Step 7: Advanced Unit Testing with Data-Driven Tests**

1. **Refactor the Test File to Use Data-Driven Testing:**

   ```python
   # tests/test_calculator.py
   import pytest
   from src.calculator import Calculator

   calc = Calculator()

   @pytest.mark.parametrize("a, b, expected", [
       (2, 3, 5),
       (-1, 1, 0),
       (0, 0, 0),
   ])
   def test_add(a, b, expected):
       assert calc.add(a, b) == expected

   @pytest.mark.parametrize("a, b, expected", [
       (5, 3, 2),
       (0, 1, -1),
   ])
   def test_subtract(a, b, expected):
       assert calc.subtract(a, b) == expected
   ```

2. **Run Tests Again:**

   ```bash
   pytest tests/
   ```

   ![alt text](/images/img138.png)

---

## **Step 6: Handling Exception Scenarios**

1. **Add Tests for Exception Handling (Division by Zero):**

   ```python
   # tests/test_calculator.py

   def test_divide_exceptions():
       with pytest.raises(ValueError, match="Cannot divide by zero."):
           calc.divide(10, 0)
   ```

---

## **Step 7: Mocking External Dependencies (Advanced)**

1. **Create an External API Client in `src/weather.py`:**

   ```python
   # src/weather.py
   import requests

   class Weather:
       def get_weather(self, city: str) -> dict:
           response = requests.get(f"https://api.weatherapi.com/v1/current.json?q={city}")
           return response.json()
   ```

2. **Test the Weather API with Mocking:**

   ```python
   # tests/test_weather.py
   import pytest
   from src.weather import Weather
   from unittest.mock import patch

   @patch("src.weather.requests.get")
   def test_get_weather(mock_get):
       mock_response = {
           "location": {"name": "London"},
           "current": {"temp_c": 20}
       }
       mock_get.return_value.json.return_value = mock_response

       weather = Weather()
       response = weather.get_weather("London")
       assert response["location"]["name"] == "London"
       assert response["current"]["temp_c"] == 20
   ```

3. **Run Tests to Verify Mocking Works:**

   ```bash
   pytest 
   ```

   ![alt text](/images/img139.png)

---

## **Step 8: Automating with GitHub Copilot**

* Use GitHub Copilot to generate additional test cases:

  * Type `# test` at the top of any function.
  * Use prompt commands like `Generate unit test cases for this function.`
  * Customize the test cases as needed.

---

## **Step 9: Managing Test Coverage**

1. **Install Coverage Tool:**

   ```bash
   pip install coverage
   ```

2. **Run Coverage Analysis:**

   ```bash
   coverage run -m pytest tests/
   coverage report -m
   ```
   ![alt text](/images/img140.png)

---

### **Best Practices for Effective Unit Testing**

* **Write Small, Isolated Tests:** Each test should only focus on one functionality.
* **Use Data-Driven Tests:** For functions with multiple scenarios.
* **Test for Exceptions:** Ensure edge cases are handled.
* **Mock External APIs:** To avoid network calls during testing.
* **Automate Test Runs:** Set up GitHub Actions for Continuous Integration (CI).

---

# **Master Test-Driven Development with GitHub Copilot**

## **Introduction**
Test-Driven Development (TDD) is a software development approach where test cases are written before the actual code. GitHub Copilot can significantly enhance this process by assisting in generating test cases and writing the corresponding code efficiently. In this guide, we will explore how to use GitHub Copilot to practice TDD using a Password Validator example.


## **What is Test-Driven Development (TDD)?**
TDD is a software development methodology that follows three main steps:

1. **Write Tests First:** Create unit tests that define the expected behavior of the code.
2. **Write Code:** Develop the code that meets the requirements of the tests.
3. **Refactor:** Optimize the code while ensuring that all tests still pass.

---

## **Setting Up the Project**

1. Create a new .NET project:

   ```bash
   dotnet new console -n PasswordValidator
   cd PasswordValidator
   dotnet new xunit -n PasswordValidator.Tests
   ```
2. Add a new class file `PasswordValidator.cs` in the main project directory.
3. Add a blank `PasswordValidator` class:

   ```csharp
   public class PasswordValidator
   {
   }
   ```

---

## **Step 1: Writing Unit Test Cases First**

* Open `PasswordValidator.Tests/UnitTest1.cs`.
* Start by providing a prompt to GitHub Copilot:

  ```plaintext
  Generate unit test cases for a PasswordValidator class that checks the following:
  - Password should not be null or empty.
  - Password must be at least 8 characters.
  - It should contain at least one uppercase letter, one lowercase letter, one digit, and one special character (_).
  ```

### **Copilot Suggestion**

Copilot may generate a data-driven test method like this:

![alt text](/images/img232.png)

```csharp
using Xunit;

public class PasswordValidatorTests
{
    [Theory]
    [InlineData("Password1_", true)]
    [InlineData("short1_", false)]
    [InlineData("nocapitals1_", false)]
    [InlineData("NOLOWERCASE1_", false)]
    [InlineData("NoDigit_", false)]
    public void ValidatePassword_ShouldValidatePasswordCorrectly(string password, bool expected)
    {
        var result = PasswordValidator.Validate(password);
        Assert.Equal(expected, result);
    }
}
```

---

## **Step 2: Generating the Password Validator Code**

* Now, provide a prompt for Copilot to generate the Password Validator code:

  ```plaintext
  Generate a PasswordValidator class that meets the unit test requirements above.
  ```

### **Copilot Suggestion**

![alt text](/images/img233.png)

```csharp
public static class PasswordValidator
{
    public static bool Validate(string password)
    {
        if (string.IsNullOrEmpty(password)) return false;
        if (password.Length < 8) return false;
        if (!password.Any(char.IsUpper)) return false;
        if (!password.Any(char.IsLower)) return false;
        if (!password.Any(char.IsDigit)) return false;
        if (!password.Contains("_")) return false;
        return true;
    }
}
```

---

## **Step 3: Running the Tests**

* Use the following command to run the test cases:

  ```bash
  dotnet test
  ```
* The tests should pass if the code is correctly written.
  ![alt text](/images/img234.png)
---

## **Step 4: Adding New Requirements and Refactoring**

* Now, add a new requirement: "Password must start with a letter."
* Provide a prompt to Copilot for the new test case:

  ```plaintext
  Generate a unit test case for PasswordValidator to ensure the password starts with a letter.
  ```

### **Copilot Suggestion**

```csharp
[InlineData("1Password_", false)]
[InlineData("_Password1", false)]
[InlineData("Password1_", true)]
```

* Update the `Validate` method in the PasswordValidator class accordingly:

```csharp
if (!char.IsLetter(password[0])) return false;
```

---

## **Conclusion**
Test-Driven Development (TDD) using GitHub Copilot can significantly speed up development by automating the generation of test cases and the corresponding code. Following this guide, you have learned how to:

* Set up a TDD project.
* Use GitHub Copilot to write test cases.
* Generate code that meets the test requirements.
* Enhance and refactor code as new requirements are added.

---
> **Reference video:** https://www.youtube.com/watch?v=dr7vB5gBLj0&list=PLIPPtc5KlYDPDBmDjBCDJt-5Zcdq3tG86&index=69


---

# **GitHub Copilot Chat for Unit Test Generation**

GitHub Copilot Chat helps you:

* Generate **unit test scripts** interactively.
* Support various **testing frameworks** like NUnit, XUnit, JUnit, etc.
* Handle **exceptions**, **edge cases**, **mocking**, and **data-driven tests**.
* Integrate with **VS Code** or temporarily use it for IntelliJ projects.

---

## **Basic Unit Test Generation**

### **Step 1: Select the Method**

Select the method you want to test in your code editor.

**is_palindrome.py**

```python
using System;
public static class PalindromeChecker
{
    public static bool IsPalindrome(string input)
    {
        if (input == null) return false;

        int left = 0;
        int right = input.Length - 1;

        while (left < right)
        {
            if (input[left] != input[right])
                return false;
            left++;
            right--;
        }
        return true;
    }
}
```


### **Step 2: Use Copilot Chat**

**Type:**

```
/tests
```

**Copilot Suggestion**

![alt text](/images/img294.png)
Copilot will auto-generate test cases for the method. For a `isPalindrome(string)` method, Copilot Chat generates:

* A test for a valid palindrome.
* A test for a non-palindrome.
* A test for an empty string.
* A test for a single character.
* A test for case sensitivity.

---

## **Customize for Framework**

### **Default is NUnit or similar.**

If you're using a different framework (e.g., XUnit), prompt with:

```
/test generate using xunit
```

**Copilot Suggestion**
This ensures compatibility with your test environment.

![alt text](/images/img295.png)


---

## **Prompt-Based Refinement**

You can ask for test cases for **specific edge cases**:

```
Write a unit test for non-prime input using xunit
```

**Use prompt-based refinement for:**

![alt text](/images/img296.png)
* Covering missing conditions.
* Adding more edge cases.
* Specifying mocking or exception handling.

---

## **Exception Handling**

**Given a method like `divide(int a, int b)`, simply type:**

```
/tests
```

**Copilot suggestion:**

* Test for valid division.
* Test for division by zero with proper exception assertion.

---

## **Data-Driven Tests**

**Ask:**

```
/tests generate data-driven test using theory
```

**Copilot suggestion:**

* Use `[Theory]` and `[InlineData]` for XUnit.
* Allow CSV-driven or inline datasets.
* Handle exception scenarios in test data too.

---

## **Java Support via IntelliJ**

**IntelliJ doesn’t yet support Copilot Chat natively.**

**Workaround:**

* Open your Java project in **VS Code**.
* Use Copilot Chat to generate JUnit tests.
* Copy generated tests back to IntelliJ.

### **Java Prompt Example:**

```
Add unit tests for divide() using JUnit and Mockito
```

---

## **Mocking & External Services**

If testing a service like `TodoService` that depends on an interface:

### **Prompt:**

```
/tests using mockito for TodoService
```

**Copilot Suggestion:**

* Generate mocks using `@Mock`, `@InjectMocks`.
* Use `Mockito.when(...).thenReturn(...)`.
* Ensure external dependencies are stubbed.

---

## **Validating Test Cases**

After generating:

1. Save the test file.
2. Run tests in your IDE.
3. Fix failed tests by aligning **expected vs actual** behavior.
4. Use `/fix` or ask Copilot Chat:

   ```
   Why is this test failing?
   ```

---

## **Coverage Check**

After tests run:
![alt text](/images/img297.png)
* Run with **coverage** to ensure all branches/conditions are tested.
* Green lines in IntelliJ or VS Code indicate full coverage.

---

## **Tips**

| Tip                      | Description                                                                 |
| ------------------------ | --------------------------------------------------------------------------- |
| 🧠 Be Specific           | Mention the framework, boundary case, or data-driven setup.                 |
| 🔁 Iterate               | Prompt again to add more edge case coverage.                                |
| ⚙️ Environment Match     | Ensure the test framework matches your dev environment (NUnit/XUnit/JUnit). |
| 🚫 Avoid False Positives | Verify that expected outputs are realistic.                                 |

---

## **Prompt Cheat Sheet**

| Task                     | Prompt                                         |
| ------------------------ | ---------------------------------------------- |
| Basic unit test          | `/test`                                        |
| Use a specific framework | `/test using xunit`                            |
| Generate for edge case   | `Write unit test for non-prime using xunit`    |
| Data-driven test         | `/test generate data-driven test using theory` |
| Java + Mockito           | `Add unit test using Mockito for method XYZ`   |
| Fix failing test         | `Why is this test failing?`                    |

---
> **Reference video:** https://www.youtube.com/watch?v=i-hqpu7EOp8&list=PLIPPtc5KlYDPDBmDjBCDJt-5Zcdq3tG86&index=95

---

# **Unit Test Generation using GitHub Copilot - JUnit, Mockito & Java**

### **1. Basic Unit Test Generation**

* A **simple `Calculator` class** with methods like `add`, `subtract`, and `multiply` is used.
* GitHub Copilot is prompted to generate a **unit test for the `add()` method**.
* The test is correctly suggested using **JUnit**.

---

### **2. Parameterized Tests (Inline)**

* Use of **`@ParameterizedTest`** and **`@CsvSource`** for testing multiple data sets.
* **Example:**
  ```java
  @Paramete*rizedTest
  @CsvSource({
    "2, 3, 5",
    "10, 5, 15",
    "0, 0, 0"
  })
  void testAdd(int a, int b, int expected) {
      assertEquals(expected, calculator.add(a, b));
  }
  ```

* **Copilot sometimes fails** to infer `@ParameterizedTest` unless guided by initial import/context.

---

### **3. Parameterized Tests (CSV File Source)**

* Usage of **`@CsvFileSource`** for reading test data from external `.csv` files.
  
* **Syntax Example:**

  ```java
  @ParameterizedTest
  @CsvFileSource(resources = "/data.csv", numLinesToSkip = 1)
  void testSubtract(int a, int b, int expected) {
      assertEquals(expected, calculator.subtract(a, b));
  }
  ```

* Supports skipping header lines via `numLinesToSkip`.

---

### **4. Testing Exception Handling**

* A `divide(int a, int b)` method is added with exception handling (e.g., divide-by-zero).
* Copilot is prompted to write tests for **exception scenarios**, using:

  ```java
  @Test
  void testDivideByZeroThrowsException() {
      assertThrows(ArithmeticException.class, () -> calculator.divide(10, 0));
  }
  ```
* **Code coverage** checked to verify that the exception branch is executed.

---

### **5. Mockito for Mocking**

* A **`ToDoService` interface** is introduced, not fully implemented.
* **Mockito** is used to **mock service dependencies**.

#### **Setup Steps:**

1. Annotate test class with:

   ```java
   @RunWith(MockitoJUnitRunner.class)
   ```
2. Mock and inject:

   ```java
   @Mock
   ToDoService toDoService;

   @InjectMocks
   ToDoBusinessImpl toDoBusiness;
   ```
3. Define mock behavior using `when(...).thenReturn(...)`.

#### **Example Test:**

```java
@Test
public void testRetrieveTodosRelatedToJava() {
    List<String> todos = Arrays.asList("Learn Java", "Learn Spring", "Buy Groceries");
    when(toDoService.retrieveTodos("User")).thenReturn(todos);

    List<String> filtered = toDoBusiness.retrieveTodosRelatedToJava("User");

    assertEquals(2, filtered.size());
}
```

---

### **GitHub Copilot Tips Observed**

* Initially needs help with imports and annotations.
* Once context is built (imports, annotations present), Copilot is **faster and more accurate**.
* Effective for:

  * Simple unit tests
  * Parameterized tests
  * CSV data-driven tests
  * Mock-based test generation
  * Exception handling

---

### **Coverage & Verification**

* Used **code coverage tools** to ensure:

  * All paths (including exceptions) are tested.
  * Mocked dependencies simulate expected behavior accurately.

---

### **Key Takeaways**

* Copilot enhances productivity, but:

  * Developers must **guide it** with initial setup.
  * Proper **JUnit & Mockito knowledge** is essential.
* Effective testing includes:

  * Positive & negative test cases
  * Data-driven tests
  * Exception coverage
  * Dependency mocking

---

# **Unit Test Generation using GitHub Copilot - XUnit & .NET Core**

Here’s a **step-by-step guide** to using **GitHub Copilot** for **unit test generation** in a **.NET Core application using xUnit**. This guide includes necessary **examples**, **Copilot prompts**, and **explanations** to help you effectively generate and validate tests with Copilot.

---

## **Prerequisites**

1. **.NET SDK** installed ([Download .NET](https://dotnet.microsoft.com/download))
2. **Visual Studio Code (VS Code)** with the following extensions:

   * C# by Microsoft
   * GitHub Copilot
3. Basic understanding of:

   * Writing methods in C#
   * Writing unit tests with xUnit

---

## **Step 1: Setup the Project**

### **Create the Project**

```bash
dotnet new sln -n CopilotTestingDemo
dotnet new classlib -n DemoLibrary
dotnet new xunit -n DemoLibrary.Tests
dotnet sln add DemoLibrary/DemoLibrary.csproj
dotnet sln add DemoLibrary.Tests/DemoLibrary.Tests.csproj
dotnet add DemoLibrary.Tests/DemoLibrary.Tests.csproj reference DemoLibrary/DemoLibrary.csproj
```

---

## **Step 2: Create Business Logic Methods**

### **In `DemoLibrary/Validator.cs`:**

```csharp
namespace DemoLibrary;

public class Validator
{
    public bool IsPalindrome(string input)
    {
        if (string.IsNullOrEmpty(input)) return false;
        var reversed = new string(input.Reverse().ToArray());
        return string.Equals(input, reversed, StringComparison.OrdinalIgnoreCase);
    }

    public int IsPrime(int number)
    {
        if (number <= 1) return 0;
        for (int i = 2; i <= Math.Sqrt(number); i++)
        {
            if (number % i == 0)
                return 0;
        }
        return 1;
    }
}
```

---

## **Step 3: Prompt GitHub Copilot to Generate Tests**

Open `DemoLibrary.Tests/ValidatorTests.cs` and start typing test method stubs. Let Copilot autocomplete them.

### **Example Prompt:**

```csharp
// Unit tests for IsPalindrome method
```

### **Copilot Suggestion:**

```csharp
using Xunit;
using DemoLibrary;

namespace DemoLibrary.Tests;

public class ValidatorTests
{
    [Theory]
    [InlineData("racecar", true)]
    [InlineData("hello", false)]
    [InlineData("madam", true)]
    [InlineData("", false)]
    [InlineData("a", true)]
    public void IsPalindrome_TestCases(string input, bool expected)
    {
        var validator = new Validator();

        var result = validator.IsPalindrome(input);

        Assert.Equal(expected, result);
    }
}
```

> ✅ Accept the suggestion or press `Tab`.

---

## **Step 4: Add More Prompts for Other Methods**

### **Example Prompt:**

```csharp
// Add unit test for IsPrime method using InlineData
```

### **Copilot Suggestion:**

```csharp
[Theory]
[InlineData(2, 1)]
[InlineData(4, 0)]
[InlineData(11, 1)]
[InlineData(1, 0)]
public void IsPrime_TestCases(int number, int expected)
{
    var validator = new Validator();

    var result = validator.IsPrime(number);

    Assert.Equal(expected, result);
}
```

---

## **Step 5: Use Data-Driven Tests (CSV File)**

### **Create a CSV: `DemoLibrary.Tests/palindromes.csv`**

```csv
racecar
hello
Malayalam
```

### **Prompt Copilot:**

```csharp
// Method to read test data from palindromes.csv
```

### **Copilot Suggestion:**

```csharp
public static IEnumerable<object[]> GetPalindromeTestData()
{
    var lines = File.ReadAllLines("palindromes.csv");
    foreach (var line in lines)
    {
        yield return new object[] { line };
    }
}
```

### **Use `MemberData` with `[Theory]`**

```csharp
[Theory]
[MemberData(nameof(GetPalindromeTestData))]
public void IsPalindrome_CsvDataDriven(string input)
{
    var validator = new Validator();
    var result = validator.IsPalindrome(input);
    Assert.True(result); // all are palindromes
}
```

---

## **Step 6: Data-Driven Tests with Expected Output**

### **CSV: `primes.csv`**

```csv
2,1
4,0
5,1
9,0
```

### **Prompt Copilot:**

```csharp
// Method to read data with expected output from primes.csv
```

### **Copilot Suggestion:**

```csharp
public static IEnumerable<object[]> GetPrimeTestData()
{
    var lines = File.ReadAllLines("primes.csv");
    foreach (var line in lines)
    {
        var parts = line.Split(',');
        yield return new object[] { int.Parse(parts[0]), int.Parse(parts[1]) };
    }
}
```

### **Test Method Using `[MemberData]`**

```csharp
[Theory]
[MemberData(nameof(GetPrimeTestData))]
public void IsPrime_CsvDataDriven(int input, int expected)
{
    var validator = new Validator();
    var result = validator.IsPrime(input);
    Assert.Equal(expected, result);
}
```

---

## **Step 7: Run All Tests**

**Run the following in terminal:**

```bash
dotnet test
```

**Sample Output:**

```text
Build started, please wait...
Build completed.

Test run for /Users/gowthami/Documents/PalindromeTests/bin/Debug/net7.0/PalindromeTests.dll (.NETCoreApp,Version=v7.0)
Microsoft (R) Test Execution Command Line Tool Version 17.4.0
Copyright (c) Microsoft Corporation.  All rights reserved.

Starting test execution, please wait...
A total of 1 test files matched the specified pattern.
Passed!  - Failed:     0, Passed:    10, Skipped:     0, Total:    10, Duration: 200 ms
```

* **Passed:** All 10 test cases passed successfully.
* **Failed:** No test case failed.
* **Skipped:** No test case was skipped.
* **Duration:** All tests ran within \~200 ms.

---

## **Works Beyond .NET**

Copilot also supports **Spring Boot**, **Node.js**, **Python**, and others for test generation. Just follow similar patterns using the relevant framework (like JUnit for Java, PyTest for Python, etc.).

---

## **Summary**

| Feature        | Copilot Prompt                          | Result                       |
| -------------- | --------------------------------------- | ---------------------------- |
| Method Test    | `// Add unit test for IsPalindrome`     | Suggests test method         |
| Data-driven    | `// Read from CSV file`                 | Generates file reading logic |
| Negative Tests | `// Add non-palindrome test case`       | Suggests negative case       |
| Class-level    | `// Add unit tests for Validator class` | All test methods generated   |

---
> **Reference video:** https://www.youtube.com/watch?v=7C266My-w3Q&list=PLIPPtc5KlYDPDBmDjBCDJt-5Zcdq3tG86&index=101


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
  ![alt text](/images/img242.png)

---

## **2. Using GitHub Copilot in CLI (Command Line Interface)**
GitHub Copilot can generate and assist with Kubernetes commands directly from the terminal.

### **Example: Generating a Kubernetes Deployment Command**
1. Run the command:

   ```bash
   gh copilot ask "Generate a Kubernetes deployment command for a Node.js application."
   ```
2. Copilot Suggestion:

   ![alt text](/images/img243.png)

### **Advanced Usage - Revising Commands**
* **Prompt:** "Revise the command to include resource limits."
* **Copilot Suggestion:**
  ![alt text](/images/img244.png)

---

## **3. Automating Kubernetes Cluster Setup**
* Use Copilot to generate cluster setup scripts, manage nodes, and set up networking.

### **Example: Cluster Setup with Minikube**
* **Prompt:** "Generate a script to set up a Minikube cluster with 3 nodes."
  
* **Copilot Suggestion:**
   ![alt text](/images/img245.png)

---

## **4. Configuration Management with Helm and Customization**
* Use Helm for Kubernetes configuration management and Copilot for Helm chart automation.

### **Example: Creating a Helm Chart**
* **Prompt:** "Generate a Helm chart for a MySQL database."

* **opilot Suggestion:**
   ![alt text](/images/img246.png)

### **Customizing Helm Values**
* **Prompt:** "Customize Helm values for persistent storage."
* **Copilot Suggestion:**
  ![alt text](/images/img247.png)

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
> **Reference video:** https://www.youtube.com/watch?v=4kB3Sg-Gkwc&list=PLIPPtc5KlYDPDBmDjBCDJt-5Zcdq3tG86&index=71

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

# **GitHub Copilot Can Now Read URLs! Paste Links & Get Smarter Code**

## **What’s New?**

GitHub Copilot Chat can now **understand context directly from GitHub and external URLs** — such as issues, pull requests, discussions, articles, documentation, etc.

---

## **Step-by-Step Guide to Using URL Context in GitHub Copilot Chat**

### **Step 1: Start with a GitHub Issue or File**

**1. Open a GitHub repository** with issues, discussions, or files.

![alt text](/images/img29.png)

**2. Copy the URL** of:

* An issue (e.g., bug report, feature request)
* A file you want to edit (e.g., `README.md`)
* A pull request or discussion

*Here I have opened the issue.*

![alt text](/images/img28.png)

---

### **Step 2: Paste the URL into Copilot Chat**

* Go to **Copilot Chat** (on GitHub.com).
  ![alt text](/images/img30.png)

* Paste the copied **URL**.

* Ask Copilot something like:

  ![alt text](/images/img31.png)

  ```
  Explain what this issue is about.  
  ```

* **Copilot will read the content** of the link and summarize it.

  ![alt text](/images/img32.png)

---

### **Step 3: Enhance the Issue or Discussion**

* You can refine or enhance the issue by asking:

  ```
  How can I enhance the issue details?
  ```

* **Copilot Output:**
  ![alt text](/images/img33.png)

---

### **Step 4: Use a File Link to Implement a Solution**

**1. Copy the URL of the target file (e.g., `README.md`).**

**2. Ask Copilot:**

   ![alt text](/images/img34.png)

**3. Copilot will use:**

   * The issue link (for context)
   * The file link (as a target)
   * And generate the implementation
   * It has implemented the license and contact details

   ![alt text](/images/img36.png)

---

### **Step 5: Combine External Resources**

You can now include **external documentation or articles**:

**1. Copy the URL of an article or style guide***
  ![alt text](/images/img37.png)

**2. Ask Copilot:**

  ```
  Use this article to implement a footer with Angular Material.
  ```

**3. Copilot will read the article and extract relevant patterns**
  ![alt text](/images/img38.png)

---

### **Step 6: Use in IntelliJ**

* Open **IntelliJ** with Copilot Chat enabled.

* Use the same workflow:

  1. Paste GitHub issue/file URL
  2. Paste article or doc link
  3. Open Copilot chat and ask Copilot chat to generate code
  4. Refine or implement based on suggestions
     ![alt text](/images/img39.png)

* **Copilot Output:**
  ![alt text](/images/img40.png)

---
> **Reference video:** https://www.youtube.com/watch?v=QRZfR0TSmQ8&list=PLIPPtc5KlYDPDBmDjBCDJt-5Zcdq3tG86&index=15

---

# **Setting Up GitHub Copilot with Custom Coding Instructions**

## **Prerequisites**
- Visual Studio Code installed
- GitHub account with Copilot subscription
- Git installed on your system

## **Step 1: Initial Setup in VS Code**

1. **Install GitHub Copilot Extension:**
   * Open VS Code
   * Press `Ctrl + Shift + X` to open Extensions
   * Search for "GitHub Copilot"
   * Click "Install"
   * Wait for installation to complete
   * Reload VS Code when prompted

2. **Configure Copilot Settings:**
   * Press `Ctrl + ,` to open Settings
   * Type "GitHub Copilot" in the search bar
   * Navigate to "GitHub Copilot Chat > Preview Features"
   * Check the box for "GitHub Copilot Chat Code Instructions"
     ![alt text](/images/img143.png)

   * Restart VS Code to apply changes

## **Step 2: Repository Setup**

![alt text](/images/img144.png)

1. **Create Project Directory:**
   ```powershell
   New-Item -ItemType Directory -Path "my-copilot-project"
   Set-Location my-copilot-project
   code .
   ```

2. **Initialize Git Repository:**
   ```powershell
   git init
   git branch -M main
   ```

3. **Create GitHub Directory Structure:**
   ```powershell
   New-Item -ItemType Directory -Path ".github"
   ```
   

## **Step 3: Creating Instruction Files**

![alt text](/images/img145.png)

1. **Create Base Instructions File:**
   ```powershell
   New-Item -Path ".github/copilot_instructions.md"
   ```

2. **Add Basic Configuration:**
   ```markdown
   # GitHub Copilot Instructions

   ## Global Code Style
   - Use meaningful variable and function names
   - Include proper documentation for all public APIs
   - Follow language-specific best practices
   
   ## Documentation Requirements
   - Add header comments to all files
   - Include usage examples for public functions
   - Document all parameters and return values

   ## Security Guidelines
   - Validate all user inputs
   - Use secure coding practices
   - Handle errors appropriately
   ```

## **Step 4: Language-Specific Instructions**

1. **Python Instructions:**
   ```markdown
   ## Python Guidelines
   
   ### Naming Conventions
   - Classes: PascalCase (Example: `UserAccount`)
   - Functions: snake_case (Example: `calculate_total`)
   - Variables: snake_case (Example: `user_input`)
   - Constants: UPPER_CASE (Example: `MAX_ATTEMPTS`)
   
   ### Documentation
   ```python
   def function_name(param1: type, param2: type) -> return_type:
       """
       Brief description of function.
       
       Args:
           param1 (type): Description of param1
           param2 (type): Description of param2
           
       Returns:
           return_type: Description of return value
           
       Raises:
           ExceptionType: When and why this exception occurs
       """
   ```

2. **JavaScript/TypeScript Instructions:**
   ```markdown
   ## JavaScript/TypeScript Guidelines
   
   ### Naming Conventions
   - Classes: PascalCase (Example: `UserService`)
   - Functions: camelCase (Example: `getUserData`)
   - Variables: camelCase (Example: `userData`)
   - Constants: UPPER_CASE (Example: `API_KEY`)
   
   ### Documentation
   ```typescript
   /**
    * Brief description of function
    * @param {parameterType} paramName - Parameter description
    * @returns {returnType} Description of return value
    * @throws {ErrorType} Description of when errors are thrown
    */
   ```

## **Step 5: Testing Your Instructions**

1. **Create Test Files:**
   * Create a new Python file: `test.py`
   * Create a new JavaScript file: `test.js`

2. **Test Python Implementation:**
   ```python
   # "Create a function to calculate the area of a circle"
   ```
   ![alt text](/images/img146.png)
   

3. **Verify Generated Code:**
   ![alt text](/images/img147.png)

   * Check if docstrings are present
   * Verify naming conventions
   * Ensure error handling is included

## **Step 6: Version Control**

1. **Commit Your Changes:**
   ```powershell
   git add .github/copilot_instructions.md
   git commit -m "Add Copilot instructions and guidelines"
   ```

2. **Link to GitHub:**
   ```powershell
   git remote add origin <your-repository-url>
   git push -u origin main
   ```

## **Step 7: Maintenance and Updates**

1. **Regular Review Process:**
   * Review instructions monthly
   * Update based on team feedback
   * Keep track of changes in a changelog

2. **Team Collaboration:**
   * Share instructions with team members
   * Collect feedback and suggestions
   * Update guidelines based on project needs

## **Step 8: Troubleshooting Guide**

**Common Issues and Solutions:**
* Instructions not applying:
  - Verify file location (.github/copilot_instructions.md)
  - Check file permissions
  - Restart VS Code

* Inconsistent behavior:
  - Clear Copilot cache
  - Update VS Code and Copilot extension
  - Re-authenticate GitHub account

---

> **Reference video:** https://www.youtube.com/watch?v=J8yiJizRFyw&list=PLIPPtc5KlYDPDBmDjBCDJt-5Zcdq3tG86&index=36


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
   ![alt text](/images/img181.png)

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
   ![alt text](/images/img182.png)

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
   ![alt text](/images/img183.png)

2. Make sure the layout looks as expected.
3. If needed, use Copilot to further refine the layout and design.

--- 

## **Conclusion**

GitHub Copilot can significantly speed up the process of building web pages. With the right prompts, you can create, style, and enhance your webpage efficiently. Use it as a coding assistant to build better designs.

---

> **Reference video:** https://www.youtube.com/watch?v=4TH8IdVe1UA&list=PLIPPtc5KlYDPDBmDjBCDJt-5Zcdq3tG86&index=52 

---

# **GitHub Copilot Chat for New Projects**

## **Part 1: Create a Machine Learning Project (Sentiment Analysis) using GitHub Copilot Chat**

**Goal**

Create a Jupyter notebook to perform sentiment analysis on 7 hard-coded statements and visualize the sentiment scores.

---

### **Step-by-Step**

#### **Step 1: Open Visual Studio Code with Copilot Chat**

Make sure you have:

* GitHub Copilot Chat extension installed
* VS Code updated
* Jupyter extension installed

---

#### **Step 2: Prompt Copilot Chat to Create a Sentiment Analysis Notebook**

**Prompt to Copilot Chat:**

> "Create a Jupyter notebook for sentiment analysis of 7 hardcoded statements. Calculate sentiment scores and plot them as a bar chart."

**Copilot Suggestion:**

Copilot will respond with:
![alt text](/images/img299.png)

* Notebook code including:

  * Library imports
  * List of 7 statements
  * Sentiment analysis using `TextBlob` or `VADER`
  * Score calculation
  * Bar chart plot


---

#### **Step 3: Verify the Notebook**

**Example Snippet:**

**Import libraries**
```python
# Step 1: Import libraries
import pandas as pd
import matplotlib.pyplot as plt
from textblob import TextBlob
```


**Create data**
```python
# Define 7 hardcoded statements
statements = [
    "I love sunny days and blue skies.",
    "This is the worst movie I have ever seen.",
    "The food was okay, nothing special.",
    "I'm so excited about the upcoming concert!",
    "I feel sad when it rains all day.",
    "The customer service was excellent and very helpful.",
    "I'm not sure how I feel about this new policy."
]

# Create a DataFrame
df = pd.DataFrame({'Statement': statements})
df
```

**Ouput**
![alt text](/images/img302.png)



**Calculate sentiment polarity for each statement**
```python
df['Polarity'] = df['Statement'].apply(lambda x: TextBlob(x).sentiment.polarity)
df
```
**Ouput**
![alt text](/images/img301.png)



```python
# Plot sentiment scores as a bar chart
plt.figure(figsize=(10, 6))
bars = plt.bar(range(len(df)), df['Polarity'], color='skyblue')
plt.xticks(range(len(df)), [f"Statement {i+1}" for i in range(len(df))], rotation=45)
plt.xlabel('Statements')
plt.ylabel('Sentiment Polarity')
plt.title('Sentiment Analysis of 7 Hardcoded Statements')
plt.ylim(-1, 1)

# Annotate bars with actual statements
for bar, statement in zip(bars, df['Statement']):
    plt.text(bar.get_x() + bar.get_width()/2, bar.get_height(), statement, 
             ha='center', va='bottom', fontsize=8, rotation=90, wrap=True)

plt.tight_layout()
plt.show()
```


**output**
![alt text](/images/img300.png)

---

#### **Step 4: Customize with a Line Chart**

**Prompt to Copilot Chat:**

> "Replace the bar chart with a line chart for sentiment scores of each statement."

**Copilot Suggestion:**

```python
# Plot sentiment scores as a line chart
plt.figure(figsize=(10, 6))
plt.plot(range(len(df)), df['Polarity'], marker='o', color='skyblue', linewidth=2)
plt.xticks(range(len(df)), [f"Statement {i+1}" for i in range(len(df))], rotation=45)
plt.xlabel('Statements')
plt.ylabel('Sentiment Polarity')
plt.title('Sentiment Analysis of 7 Hardcoded Statements')
plt.ylim(-1, 1)

# Annotate points with actual statements
for i, (x, y, statement) in enumerate(zip(range(len(df)), df['Polarity'], df['Statement'])):
    plt.text(x, y, statement, ha='center', va='bottom', fontsize=8, rotation=90, wrap=True)

plt.tight_layout()
plt.show()
```

**Output**
![alt text](/images/img303.png)

---

## **Part 2: Build a Java Employee Management Application using GitHub Copilot Chat**

**Goal**

Develop a complete Java-based Employee Management System with:

* Add employee
* Validate email and phone
* Calculate payroll
* Send notifications
* Exception handling and logging

---

### **Step-by-Step**

#### **Step 1: Prompt Copilot to Create Project Structure**

**Prompt:**

> "Create a Java application for employee management. Include methods to add employee, validate email and phone, calculate payroll, and send notifications."

**Copilot Suggestion:**

* Java class named `EmployeeManagement`
* Methods for all requested features
* Standard folder structure (Maven style):

  ```
  src/
    main/
      java/com/example/
    test/
  ```

---

#### **Step 2: Request Project Workspace**

**Prompt:**

> "Create a workspace for this Java application with appropriate file structure and logging support using Log4j."

**Copilot Suggestion:**

* Creates file layout:

  ```
  - src/main/java/com/example/EmployeeManagement.java
  - src/test/java/com/example/EmployeeManagementTest.java
  - resources/log4j2.xml
  ```

---

#### **Step 3: Review the Main Java Class**

**Example Snippet:**

```java
public class EmployeeManagement {

    public void addEmployee(String name, String email, String phone) {
        if (!isValidEmail(email) || !isValidPhone(phone)) {
            System.out.println("Invalid email or phone");
            return;
        }
        // logic to add employee
    }

    public boolean isValidEmail(String email) {
        return email.matches("^[\\w-.]+@([\\w-]+\\.)+[\\w-]{2,4}$");
    }

    public boolean isValidPhone(String phone) {
        return phone.matches("\\d{10}");
    }

    public double calculatePayroll(double hours, double rate) {
        return hours * rate;
    }

    public void sendNotification(String message) {
        System.out.println("Sending notification: " + message);
    }
}
```

---

#### **Step 4: Add Exception Handling**

**Prompt:**

> "Add exception handling to all methods in EmployeeManagement.java."

**Copilot Suggestion:**
Adds `try-catch` blocks:

```java
try {
    // logic
} catch (Exception e) {
    e.printStackTrace();
}
```

---

#### **Step 5: Add Logging Support**

**Prompt:**

> "Add try-catch blocks that log exceptions using Log4j."

**Copilot Suggestion:**

```java
private static final Logger logger = LogManager.getLogger(EmployeeManagement.class);

public void addEmployee(String name, String email, String phone) {
    try {
        if (!isValidEmail(email)) throw new IllegalArgumentException("Invalid Email");
        // logic
    } catch (Exception e) {
        logger.error("Error adding employee", e);
    }
}
```

---

#### **Step 6: Add Unit Tests**

**Prompt:**

> "Generate JUnit test cases for each method in EmployeeManagement class."

**Copilot Suggestion:**
Creates `EmployeeManagementTest.java` with tests like:

```java
@Test
public void testIsValidEmail() {
    EmployeeManagement em = new EmployeeManagement();
    assertTrue(em.isValidEmail("test@example.com"));
}
```

---

## **Conclusion**

GitHub Copilot Chat can:

* **Bootstrap new projects** in multiple languages
* **Understand and implement features** based on plain English instructions
* **Quickly iterate** on changes like exception handling, logging, testing
* **Empower non-experts** to create functional ML or Java apps easily

---

> **Reference video:** https://www.youtube.com/watch?v=c8sU7FwI7r4&list=PLIPPtc5KlYDPDBmDjBCDJt-5Zcdq3tG86&index=99

---
