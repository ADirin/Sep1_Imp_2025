# Sprint 2 Tasks (2 Weeks)

**AD/ F2026**

## Important Notes for Students

- In this course, the primary focus is on the process of product development rather than the final product itself. Students are not expected to build a complete system in a single sprint.
- The course emphasizes Agile and Scrum practices, as well as tools such as CI/CD, Docker, and Kubernetes, during project implementation.
- The lecturer will act as the Product Owner. At the end of each sprint, teams will participate in a dedicated Sprint Review session via Zoom. Attendance is mandatory, and each team member must clearly present their individual contribution.
- **Attendance at the Sprint Review is mandatory for all team members, and each member must clearly present their individual contribution during the sprint.**
- This course is delivered through face-to-face lectures. No additional support will be provided for students who choose not to participate in the scheduled sessions.
- **Important:** In-class assignments are directly related to the project and sprint implementation. Those who fail to submit them **will receive zero** points for project implementation.

## Objective

This document outlines the tasks for Sprint 2. The primary objective of this sprint is to lay the foundation for the application by implementing the database, initiating the user interface development, and integrating essential development tools such as **unit testing**, **Maven dependencies**, and **code coverage reporting**. The Scrum Master will oversee team participation, AI application use, backlog updates, and sprint 1 review preparation and sprint 2 planning submission.

## Sprint 2 Scrum Master Tasks

1. **Ensure Team Participation** — Encourage active involvement of all team members.
2. **Track AI Application** — Monitor instances where AI is used in implementation.
3. **Update Backlogs** — Keep Jira backlogs current with status and changes.
4. **Prepare Presentation** — Create a presentation detailing time spent by each member.
5. **Present Backlog Status** — Show the status of sprint backlog items.
6. **Compile Report** — Create a comprehensive report on Sprint 2 implementation.

## Sprint 2 Implementation Tasks

*While teams are free to determine their own approach for Sprint 2, the following examples are provided for guidance. The prioritization and sequencing of tasks will be established by the Scrum Master in collaboration with the development team.*

### Implementing the Database (2 points)

- **Design Database Schema:** Based on the product requirements, design and create the database schema. This involves identifying entities, attributes, and relationships.
- **Database Technology:** Utilize the selected database technology, e.g., MariaDB, for implementation.
  - **Note:** For our database localization efforts in OTP2, please ensure you are using a relational database.
- **Create Tables/Collections:** Define tables (or collections, depending on the database type) with appropriate fields and relationships to accurately represent the data model.
- **Seed with Sample Data (Optional):** Populate the database with sample data to facilitate testing and development.
- **Test the CRUD operations** work properly in your database.

### Start Developing the User Interface (2 points)

- **Frontend Development:** Begin developing the frontend of the application, referencing the Figma design for visual guidance.
- **Create Initial Views/Screens:** Implement initial views or screens that align with the product vision and user stories.
- **Focus on Layout and Interactivity:** Prioritize the layout and basic interactive elements of the user interface.

### Integrate Unit Testing (3 points)

- Write unit tests for key functions (both backend and frontend if applicable).
- Follow the best practices for naming, structure, and test case clarity.
- Tests must be included in the project repository.

### Use Maven for Build Management

- Set up a Maven project structure (if using Java-based stack).
- Ensure all dependencies and plugins are correctly managed via `pom.xml`.

### Configure Code Coverage Testing (3 points)

- **Use JaCoCo:** Integrate JaCoCo to generate code coverage reports.
- **Run Tests Regularly:** Execute tests regularly to track code coverage and identify areas that require additional testing.
- **Aim for Meaningful Coverage:** Strive for meaningful code coverage, focusing on critical functionalities and complex logic.
- **Export and Publish Report:** Export the JaCoCo HTML report and publish it to the team's public HTML folder (e.g., university web server or a similar accessible location).

### Prepare for Sprint Review

- **Demonstration Readiness:** Be prepared to demonstrate the following:
  - The working database, showcasing its structure and data.
  - Progress on the user interface, highlighting implemented features and interactivity.
  - Unit tests in action, demonstrating their functionality and coverage.
  - Maven setup, illustrating dependency management and build process.
  - The publicly available JaCoCo report showcasing code coverage metrics.
- **Individual Contributions:** Each team member should be aware of and able to articulate their individual contributions to the sprint.
- **Update GitHub:** Ensure that all code changes and project updates are committed to the GitHub repository.
- **Update Trello/Jira:** Update Trello or Jira (or both, depending on the team's workflow) to reflect the status of tasks and progress made during the sprint.

## Note: Sample Project Folder Structure (Maven + JaCoCo)

```
my-project/
├── pom.xml
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── example/
│   │   │           └── app/
│   │   │               └── Main.java
│   │   └── resources/
│   └── test/
│       ├── java/
│       │   └── com/
│       │       └── example/
│       │           └── app/
│       │               └── MainTest.java
│       └── resources/
├── target/
│   └── site/
│       └── jacoco/
│           └── index.html   # Publish this to your public HTML folder
└── public_html/   (optional clone of target/site/jacoco for publishing if needed)
```

> **Note:** Remember to include the table below in the Sprint-review report.

| Team Member Name | Assigned Tasks | Time Spent (hrs) | In-class tasks |
|---|---|---|---|
| [Name] | [Task] | [Time] | *Submitted / Not Submitted* |
| [Name] | [Task] | [Time] | *Submitted / Not Submitted* |
| [Name] | [Task] | [Time] | *Submitted / Not Submitted* |

## Submission Summary

1. Submit a Sprint Report (*In GitHub*) detailing activities and outcomes for each of the following:
2. Individual (commit)
3. Product/Sprint Backlog Update (Trello / Jira)
4. GitHub Update
