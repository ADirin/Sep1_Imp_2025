# In-class Assignment 1 (Individual): JUnit Testing in IntelliJ

**AD / SPRINT 2**

> **Note:** The in-class assignments are compulsory for completing both the sprint implementation and the overall project. Failure to complete the in-class assignments will result in losing the implementation part of your sprint tasks and, consequently, your overall course grade. This will cause you to fail the course if your total points do not reach the required threshold of 2 points.

## Task Objective

Learn how to create and run unit tests using JUnit in IntelliJ IDEA by implementing and testing a `TemperatureConverter` class.

## Instructions

### Create a Maven Project in IntelliJ (or any other IDE you use)

1. Open IntelliJ IDEA and select **Create New Project** from the welcome screen.
2. Enter a descriptive name for your project, for example: `OTP1_inclass1_assignment_YourName`
3. Choose the location where you want to save the project.
4. In the **Build System** section, select **Maven**.
5. **JDK:** No need to change if already selected.
6. Click **Create**.

### Step 1: Create the TemperatureConverter Class

- Create a new Java class named `TemperatureConverter`.
- Implement `fahrenheitToCelsius(double fahrenheit)`: returns `(fahrenheit - 32) * 5 / 9`
- Implement `celsiusToFahrenheit(double celsius)`: returns `(celsius * 9 / 5) + 32`
- Implement `isExtremeTemperature(double celsius)`: returns `true` if the temperature is below -40°C or above 50°C.

### Step 2: Create the Test Class

- Right-click `TemperatureConverter` → **Go To** → **Test** → **Create New Test**.
- Name: `TemperatureConverterTest`
- Framework: **JUnit5**

### Step 3: Write Unit Tests

- Initialize an instance of `TemperatureConverter`.
- Test `fahrenheitToCelsius` with multiple inputs.
- Test `celsiusToFahrenheit` with multiple inputs.
- Test `isExtremeTemperature` with edge cases.

### Step 4: Run the Tests

- Right-click `TemperatureConverterTest` → **Run**.

## Submission Instructions

1. Take a screenshot of your test results.
2. Submit the screenshot to the Oma folder.
3. Attach a link to your GitHub repository.
