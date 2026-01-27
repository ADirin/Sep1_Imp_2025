# Assignment: JUnit Testing in IntelliJ

## Objective
Learn how to create and run unit tests using JUnit in IntelliJ IDEA by implementing and testing a **TemperatureConverter** class.

---

## Instructions
### Create a Maven project in Intellij.  
Open IntelliJ IDEA and select "Create New Project" from the welcome screen.
  - Select the descriptive name for your project, for example 'OTP1_inclass_assignment'
  - Select the proper location based on your preferences
  - In *Build System* select --**Maven**--
  - JDK "No need to change if already selected"
  - And then click *Create* 

### Step 1: Create the `TemperatureConverter` Class
1. **Create a new Java class** in your IntelliJ project and name it `TemperatureConverter`.
2. **Implement the following methods** in your `TemperatureConverter` class:
   - **`fahrenheitToCelsius`**:
     - This method should convert a temperature from Fahrenheit to Celsius using the formula:
       \[
       Celsius = (Fahrenheit - 32) * 5 \ 9
       \]
     - The method should accept a `double` parameter (temperature in Fahrenheit) and return a `double` value (temperature in Celsius).

   - **`celsiusToFahrenheit`**:
     - This method should convert a temperature from Celsius to Fahrenheit using the formula:
       \[
       Fahrenheit = (Celsius * 9 / 5 + 32 ]
     - The method should accept a `double` parameter (temperature in Celsius) and return a `double` value (temperature in Fahrenheit).

   - **`isExtremeTemperature`**:
     - This method should check if the given Celsius temperature is considered "extreme."
     - A temperature is considered extreme if it is:
       - Below **-40°C**, or
       - Above **50°C**.
     - The method should accept a `double` parameter (temperature in Celsius) and return a `boolean` value (`true` for extreme temperatures and `false` otherwise).

---

### Step 2: Create the Test Class
1. **Generate a test class** for `TemperatureConverter`:
   - Right-click on the `TemperatureConverter` class file.
   - Select `Go to > Test` or `Generate > Test`.
   - Choose to create a new test class and name it `TemperatureConverterTest`.
   - Select `JUnit5` as the testing framework.

---

### Step 3: Write Unit Tests
1. **Initialize the Test Class**:
   - Set up a new instance of `TemperatureConverter` in the test class.

2. **Write Test Methods**:
   - Write test methods to validate the following functionalities:
     - **`fahrenheitToCelsius`**: Ensure the conversion from Fahrenheit to Celsius is accurate for various inputs.
     - **`celsiusToFahrenheit`**: Verify the conversion from Celsius to Fahrenheit for multiple test cases.
     - **`isExtremeTemperature`**: Check if the method correctly identifies extreme temperatures.

   *You are required to implement the test methods independently.*

---

### Step 4: Run the Tests
1. **Run the test class**:
   - Right-click on the `TemperatureConverterTest` class and select `Run 'TemperatureConverterTest'`.
   - Observe the results in the Run window.

---

## Submission Instructions
1. Take a screenshot of the output showing the results of your tests.
3. Submit the screenshot to the designated folder in Oma as per classroom instructions.
4. Attach also the github repo of the solution

---

## Additional Notes
- Ensure that all your test cases pass successfully.

