# 📝Unit Test in JAVA (JUNIT)
A unit test is an automated piece of code that invokes the method being tested and then checks some assumptions about the logical behavior of that method.
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

2. Writing Unit Tests
Once planning is complete, the next step is to write the unit tests. This includes:

3. Running Unit Tests
After writing the tests, they need to be executed. This step involves:

4. Debugging and Fixing Issues
If any tests fail, debugging is necessary. This includes:

5. Refactoring Code
Once the tests pass, consider refactoring the code. This step involves:

6. Continuous Integration
Integrating unit tests into a continuous integration (CI) pipeline is essential for maintaining code quality. This includes:

7. Maintenance of Unit Tests
Unit tests require ongoing maintenance to remain effective. This involves:

# Example Application: Calculator 


Let's consider a simple example application: a calculator that performs basic arithmetic operations (addition, subtraction, multiplication, division).

### Demonstration: Unit Testing in IntelliJ IDEA

### Step 1: Create a New Maven Java Project

1. Open IntelliJ IDEA and select "Create New Project" from the welcome screen.

2. Choose "Java" from the left-hand
  - Select the descriptive name for your project
  - Select the proper location based on your preferences
  - in **Build System** select *--Maven--*
  - JDK "No need to change if already selected"
  - And then click *--Create--* 
  

### Step 2: Write the Calculator Class

Create a new Java class named `Calculator` with methods for addition, subtraction, multiplication, and division.

```java
public class Calculator {
    public int addMe(int a, int b) {
        return a + b;
    }

    public int subMe(int a, int b) {
        return a - b;
    }

    public int mulMe(int a, int b) {
        return a * b;
    }

    public int divMe(int a, int b) {
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
- A drop list appears select th **--Goto--**
- then select **--Test--**
- Select **Create Test** and then select the functions you want to Test, for example in the following I have selected all the funtions...

````
import org.junit.Before;
import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

class CalculatorTest {
    Calculator cal = new Calculator();
    
    @Test
    void addMe() {
        assertEquals(4, cal.addMe(2,2));

    }

    @Test
    void subMe() {
        assertEquals(0, cal.subMe(2,2));
    }

    @Test
    void mulMe() {
        assertEquals(6, cal.mulMe(2,3));
    }

    @Test
    void divMe() {
        assertEquals(2, cal.divMe(4,2));
    }
}
````
### Step 4: Run Unit Tests
Right-click on the CalculatorTest class and select "Run 'CalculatorTest'".

IntelliJ IDEA will execute the unit tests and display the results in the "Run" tool window.

Green checkmarks indicate successful tests, while red crosses indicate failures.

-----------------

#  JUnit 4 vs JUnit 5 Annotations  

| Feature                    | JUnit 4                   | JUnit 5                 |
|-----------------------------|-------------------------------|--------------------------|
|1. **Mark a test method**        | **`@Test`**               | **`@Test`**              |
|2. **Run before each test**      | **`@Before`**             | **`@BeforeEach`**        |
|3. **Run after each test**       | **`@After`**              | **`@AfterEach`**         |
|4. **Run once before all tests** | **`@BeforeClass` (static)** | **`@BeforeAll` (static)** |
|   Run once after all tests      | `@AfterClass` (static)    | `@AfterAll` (static)     |
|   Disable/ignore test           | `@Ignore`                 | `@Disabled`              |
|   Custom display name           | *(Not available)*         | `@DisplayName`           |
|   Repeat test multiple times    | *(Not available)*         | `@RepeatedTest`          |
|   Parameterized tests           | `@RunWith(Parameterized.class)` | `@ParameterizedTest` |
|   Tagging/filtering tests       | *(Not available)*         | `@Tag`                   |
|   Nested test classes           | *(Not available)*         | `@Nested`                |
|   Custom runner                 | `@RunWith`                | *(Extension model instead)* |
|   Add custom test behavior      | `@Rule`                   | *(Replaced by extensions* |



---------------------------------------------------------

### 📌Lecture Demo (Cal/ Degree Duration)
- [Source Code](https://github.com/ADirin/Week3_lectureDemo_Cal_Degree.git)

