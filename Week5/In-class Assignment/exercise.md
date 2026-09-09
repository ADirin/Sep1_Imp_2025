# In-class Assignment 3 (Individual): Introduction to Jenkins
**AD / SPRINT 3 — Fall 2026**

> **Note:** The in-class assignments are compulsory for completing both the sprint implementation and the overall project. Failure to complete the in-class assignments will result in losing the implementation part of your sprint tasks and, consequently, your overall course grade. This will cause you to fail the course if your total points do not reach the required threshold of 2 points.

## Instructions

1. **Continue the in-class assignment 2 and extend the project with Kelvin-to-Celsius Functionality**

   Use your existing `tempConverter` project from the previous in-class assignment.

   **Implementation Details:**
   - Function to add: `kelvinToCelsius`
   - Formula: `°C = K - 273.15`
   - Explanation: Subtract 273.15 from the Kelvin temperature

   **Example:**
   ```java
   // Input: 300 K
   // Output: 26.85°C
   // Calculation: 300 - 273.15 = 26.85
   ```

2. **Extend the JUnit Tests**

   Create new JUnit test cases to verify the correctness of your newly added Kelvin-to-Celsius conversion function.

3. **Generate a Code Coverage Report**

   Create a JaCoCo code coverage report for the entire project to ensure your tests cover all the code.

4. **Commit the final version of the code to the GitHub Repository**

5. **Set Up Jenkins and a JaCoCo Report**
   - In Jenkins, create a new Freestyle project item.
   - Select Freestyle project and give it a name (e.g., `Amir_Tempreture_V1`).
   - Under Source Code Management, add your GitHub repository link.
     - Add the GitHub repository URL.
   - Configure the Build Step.
     - Select Invoke top-level Maven targets.
     - Run your tests (e.g., using an `mvn test` or `mvn clean verify` command).
   - After a successful test run, configure the Post-Build Action to publish the JaCoCo coverage report.
     - Select Publish coverage report.

## Submission Instructions

1. **Screenshot:** Take a screenshot of the successful test report in Jenkins. Ensure your name is visible in the screenshot (e.g., in the Jenkins job name, browser tab, or via a watermark).
2. Submit the screenshot to the Oma folder in the respective folder.
3. Submit your GitHub repository link in Oma.
