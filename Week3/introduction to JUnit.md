# Unit Test in JAVA (JUNIT)
 - A unit test is an automated piece of code that invokes the method being tested and then checks some assumptions about the logical behavior of that method.
## Why Unit Test
- Ensure that your code works
- At the function and method level
- It is a low-level regression test suite
- Give a chance to improve  the design by breaking everything
- It shows the project’s progress
- It forces you to plan before you code
- It reduces the cost of bogs fix

## Best Practices for writing unit test
- Make sure your tests are one thing and one thing only
- Each unit test should be independent of the other
- Keep consistent convention
- Readability is important for tests
- Name your unit test clearly and consistently
- Separate your concerns. Extract layers to improve the design
- Avoid unnecessary preconditions
- Check code coverage during testing

## Workflow of Unit Testing

1. Planning the Unit Tests
Before writing any tests, it is essential to plan what needs to be tested. This involves:
- Identifying the components or functions that require testing.
- Determining the expected behavior of these components.
- Establishing testing criteria and success metrics.
2. Writing Unit Tests
Once planning is complete, the next step is to write the unit tests. This includes:
- Selecting a testing framework (e.g., JUnit for Java, NUnit for .NET, or pytest for Python).
- Writing test cases that cover various scenarios, including edge cases and error conditions.
- Ensuring that each test case is independent and can run in isolation.
3. Running Unit Tests
After writing the tests, they need to be executed. This step involves:
- Running the test suite using the chosen testing framework.
- Observing the results to determine which tests pass and which fail.
- Analyzing any failures to understand the underlying issues.
4. Debugging and Fixing Issues
If any tests fail, debugging is necessary. This includes:
- Investigating the code to identify the root cause of the failure.
- Making the necessary code changes to fix the identified issues.
- Rerunning the tests to ensure that the fixes have resolved the failures.
5. Refactoring Code
Once the tests pass, consider refactoring the code. This step involves:
- Improving the code structure without changing its external behavior.
- Ensuring that existing unit tests still pass after refactoring.
- Adding new tests if new functionality or changes were introduced.
6. Continuous Integration
Integrating unit tests into a continuous integration (CI) pipeline is essential for maintaining code quality. This includes:
- Automating the execution of unit tests whenever new code is committed.
- Ensuring that the build process fails if any unit tests do not pass.
- Regularly reviewing test coverage and adding tests as needed.
7. Maintenance of Unit Tests
Unit tests require ongoing maintenance to remain effective. This involves:
- Updating tests when the codebase changes or when new features are added.
- Removing obsolete tests that no longer apply.
- Periodically reviewing and refactoring test code for clarity and effici



# JUnit Setup in Advance in IntelliJ  

---

## 🔹 Install JUnit Plugin
1. Open **IntelliJ IDEA**.  
2. Navigate to:  
   - **Windows/Linux**: `File -> Settings`  
   - **macOS**: `IntelliJ IDEA -> Preferences`  
3. In the left sidebar, select **Plugins**.  
4. In the **Marketplace** tab, search for **JUnit**.  
5. Install the **JUnit plugin** (if not already installed).  

---

## 🔹 Configure JUnit Library
1. Open the **Project Structure** dialog:  
   - Right-click your project in the **Project Explorer**  
   - Select **Open Module Settings**  
2. In the **Project Settings** section, click **Libraries**.  
3. If JUnit is not listed:  
   - Click the **+** button → Select **From Maven...**  
   - Search for **`junit:junit`** in the search bar.  
   - Choose the desired version, e.g.:  
     - `junit:junit:4.12`  
     - `org.junit.jupiter:junit-jupiter-api:5.8.0`  
   - Click **OK** to add the library.  

---

## 🔹 Configure JUnit for Tests
1. In the **Project Structure** dialog, go to **Modules** (left sidebar).  
2. Open the **Dependencies** tab.  
3. Verify that the **JUnit library** is listed in the **Module SDK** section.  
