# Sprint 3: CI/CD Integration, Feature Extension, and Basic Docker Image

**Date:** AD/2025

## Objective
This document outlines the objectives and tasks completed during Sprint 3, focusing on extending the functional prototype, integrating Jenkins for CI/CD, enhancing automated testing with code coverage, and preparing for a functional review. The primary goal was to solidify the backend logic, add critical features, and establish a robust CI/CD pipeline to ensure continuous integration and delivery. We start to create a local docker image for our project.

**Sprint 3 Scrum Master tasks:**

---

## Tasks

### 1. Extend Functional Prototype (3 points)
During this sprint, we concentrated on implementing the remaining and more complex features outlined in our original product backlog. This involved:

- **Feature Implementation:** We successfully implemented the following key features:
  - User authentication and authorization module.
  - Data validation and sanitization to prevent security vulnerabilities.
  - Enhanced search functionality with filtering and sorting options.
  - Reporting and analytics dashboard for administrators.

- **Core Functionality Validation:** We rigorously tested all core functionalities to ensure they are operational and meet the specified requirements. This included:
  - End-to-end testing of user workflows.
  - Performance testing to identify and address bottlenecks.
  - Security testing to identify and mitigate potential vulnerabilities.

- **Bug Fixing and Optimization:** We addressed logic bugs identified during testing and optimized backend performance to improve responsiveness and scalability. This involved:
  - Profiling the application to identify performance bottlenecks.
  - Optimizing database queries and data structures.
  - Implementing caching mechanisms to reduce database load.

### 2. Integrate Jenkins for CI/CD (5 Points)
We established a CI/CD pipeline using Jenkins to automate the build, test, and deployment processes. The following steps were taken:

- **Pipeline Configuration:** We configured a Jenkins pipeline to automatically build and test the project upon code committed to the main branch. The pipeline includes the following stages:
  - **Code Checkout:** The pipeline retrieves the latest code from the Git repository.
  - **Build:** The pipeline compiles the code using Maven.
  - **Unit Tests:** The pipeline executes the unit tests using JUnit.
  - **Code Coverage:** The pipeline generates code coverage reports using JaCoCo.

- **Notifications:** Configured email notifications to alert the team to build success or failure (optional).

### 3. Automated Unit & Coverage Testing
We expanded our unit test suite to cover new and existing features, ensuring comprehensive test coverage.

- **Test Suite Extension:** We wrote unit tests for the newly implemented features, focusing on edge cases and boundary conditions.
- **JaCoCo Integration:** We continued using JaCoCo for code coverage analysis, ensuring that the Jenkins pipeline generates and publishes the JaCoCo HTML report.
- **Report Publication:** The Jenkins pipeline automatically publishes the JaCoCo HTML report to a designated location, making it accessible to the team. (Optional)

### 4. Functional Review Readiness
We ensured that the application is ready for a functional review, with all main features implemented and testable.

- **End-to-End Functionality:** The application functions end-to-end, allowing users to complete all core workflows.
- **Feature Completeness:** All main features are implemented and can be demonstrated.
- **Backend and Frontend Integration:** The backend and frontend are fully integrated and functional.
- **Documentation:** Updated documentation to reflect the new features and functionalities.

### 5. Develop and test the Docker image in the local machine (2 points)
- Create and test the initial docker image of the project using Docker desktop

### 6. Prepare for Sprint Review
We prepared for the sprint review by gathering evidence of completed tasks and preparing a demonstration of the working prototype.

- **Demonstration Preparation:** We prepared a demonstration of the fully working prototype, highlighting the extended features and the Jenkins CI/CD pipeline.
- **Jenkins CI/CD Demonstration:** We will demonstrate the Jenkins CI/CD pipeline in action, showcasing the build, test, and report generation processes.
- **Code Coverage Report Presentation:** We will present the updated code coverage report, highlighting the areas of the codebase that are well-tested and areas that require further testing.
- **Demonstrate that the docker image is working locally**
- **Team Contribution Evidence:** We gathered evidence of team contributions and individual tasks completed, including commit logs, task assignments, and meeting notes.
- **GitHub Updates:** Updated the project repository with the latest code, documentation, and configuration files.
- **Trello/Jira Updates:** Updated Trello/Jira boards to reflect the progress made during the sprint, including task completion status and remaining tasks.

---

**Summary:** This sprint has significantly advanced the project by establishing a robust CI/CD pipeline, extending the functional prototype with critical features, and enhancing automated testing with code coverage. We are now well-positioned to focus on polishing the UI and finalizing functionality extensions in Sprint 4.
