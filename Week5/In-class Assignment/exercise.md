# Temperature Converter Project Extension
📋 Instructions

1. Extend the Project with Kelvin-to-Celsius Functionality
## Use your existing tempConverter project from the previous in-class assignment.

Implementation Details:

- *Function to add:* kelvinToCelsius

- *Formula:* °C = K - 273.15

- *Explanation:* Subtract 273.15 from the Kelvin temperature

Example:
````java
// Input: 300 K
// Output: 26.85°C
// Calculation: 300 - 273.15 = 26.85

````
2. Extend the JUnit Tests
Create new JUnit test cases to verify the correctness of your newly added Kelvin-to-Celsius conversion function.

3. Generate a Code Coverage Report
Create a JaCoCo code coverage report for the entire project to ensure your tests cover all the code.

4. Set Up Jenkins and a JaCoCo Report (Optional)

- In Jenkins, create a new Freestyle project item.

- Under Source Code Management, add your GitHub repository link.

- Configure the build to run your tests (e.g., using an mvn test or mvn clean verify command).

- After a successful test run, configure the post-build action to publish the JaCoCo coverage report.

## Submission
1. Screenshot: Take a screenshot of the successful test report in Jenkins. Ensure your name is visible in the screenshot (e.g., in the Jenkins job name, browser tab, or via a watermark).

2. Upload: Submit the following two items to the designated Moodle folder:

- The screenshot of the Jenkins test report.

- The link to your GitHub repository.
