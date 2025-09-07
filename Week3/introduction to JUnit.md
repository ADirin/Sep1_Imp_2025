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




## Example Application: Calculator (Lecture Demo)

Let's consider a simple example application: a calculator that performs basic arithmetic operations (addition, subtraction, multiplication, division).

### Demonstration: Unit Testing in IntelliJ IDEA

### Step 1: Create a New Maven Java Project

1. Open IntelliJ IDEA and select "Create New Project" from the welcome screen.

2. Choose "Java" from the left-hand
  - Select the descriptive name for your project
  - Select the proper location based on your preferences
  - in Build System select --Maven--
  - JDK "No need to change if already selected"
  - And then click --Create-- 
  

### Step 2: Write the Calculator Class

Create a new Java class named `Calculator` with methods for addition, subtraction, multiplication, and division.

```java
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }

    public int subtract(int a, int b) {
        return a - b;
    }

    public int multiply(int a, int b) {
        return a * b;
    }

    public int divide(int a, int b) {
        if (b == 0) {
            throw new IllegalArgumentException("Cannot divide by zero");
        }
        return a / b;
    }
}
````
### Step 3: Write Unit Tests
Create a new Java class named CalculatorTest for unit tests.
- In your code editor (java code where you wrote the functions)
- Right click to the public class...
- A drop list appears select th --Goto--
- then select --Test--
- Select Create Test and then select the functions you want to Test, for example in the following I have selected all the funtions...

````
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*;

public class CalculatorTest {

    private final Calculator calculator = new Calculator();

    @Test
    public void testAddition() {
        assertEquals(5, calculator.add(2, 3));
    }

    @Test
    public void testSubtraction() {
        assertEquals(1, calculator.subtract(3, 2));
    }

    @Test
    public void testMultiplication() {
        assertEquals(6, calculator.multiply(2, 3));
    }

    @Test
    public void testDivision() {
        assertEquals(2, calculator.divide(6, 3));
    }

    @Test
    public void testDivisionByZero() {
        assertThrows(IllegalArgumentException.class, () -> calculator.divide(6, 0));
    }
}
````
### Step 4: Run Unit Tests
Right-click on the CalculatorTest class and select "Run 'CalculatorTest'".

IntelliJ IDEA will execute the unit tests and display the results in the "Run" tool window.

Green checkmarks indicate successful tests, while red crosses indicate failures.

-----------------

# 📌 JUnit 4 vs JUnit 5 Annotations  

| Purpose                     | JUnit 4 Annotation         | JUnit 5 Annotation       |
|-----------------------------|----------------------------|--------------------------|
| Mark a test method          | `@Test`                   | `@Test`                 |
| Run before each test        | `@Before`                 | `@BeforeEach`           |
| Run after each test         | `@After`                  | `@AfterEach`            |
| Run once before all tests   | `@BeforeClass` (static)   | `@BeforeAll` (static)   |
| Run once after all tests    | `@AfterClass` (static)    | `@AfterAll` (static)    |
| Disable/ignore test         | `@Ignore`                 | `@Disabled`             |
| Custom display name         | *(Not available)*         | `@DisplayName`          |
| Repeat test multiple times  | *(Not available)*         | `@RepeatedTest`         |
| Parameterized tests         | `@RunWith(Parameterized.class)` | `@ParameterizedTest` |
| Tagging/filtering tests     | *(Not available)*         | `@Tag`                  |
| Nested test classes         | *(Not available)*         | `@Nested`               |
| Custom runner               | `@RunWith`                | *(Extension model instead)* |
| Add custom test behavior    | `@Rule`                   | *(Replaced by extensions)* |




