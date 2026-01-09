# Summary of the course

This course introduces students to foundational software engineering principles within a DevOps-oriented development workflow. Throughout the course, students apply Scrum practices, working in teams with defined roles and iterative development cycles. Each sprint includes sprint planning, in-class development assignments, and sprint review activities, enabling students to translate theory into practice.

The course covers dependency management, version control, automated testing, continuous integration, containerization, and secure DevOps practices. Learning is reinforced through hands-on in-class assignments and a team-based project, culminating in a final project presentation and evaluation. This structure ensures continuous feedback, iterative improvement, and alignment between technical skills, agile processes, and collaborative software development.
```mermaid
graph TD
    A[Week 1: Software Engineering Foundations] --> B[Week 2: DevOps Fundamentals]
    B --> C[Week 3: Dependency Management]
    C --> D[Week 4: Unit Testing]
    D --> E[Week 5: Continuous Integration with Jenkins]
    E --> F[Week 6: Containerization with Docker]
    F --> G[Week 7: SecDevOps]
    G --> H[Week 8: Project Presentation and Examination]

    A1[Course Overview and Software Development Fundamentals] --> A
    B1[Continuous Integration and Delivery Practices] --> B
    C1[Dependency Management and Version Control] --> C
    D1[Unit Testing Principles and Practices] --> D
    E1[Build and Test Automation] --> E
    F1[Containerization Concepts and Usage] --> F
    G1[Security Integration in DevOps] --> G
    H1[Final Project Presentation and Learning Consolidation] --> H

    click A1 href "https://github.com/ADirin/Sep1_Imp_2025/tree/main/Week1"
    click B1 href "https://github.com/ADirin/Sep1_Imp_2025/tree/main/Week2%20"
    click C1 href "https://github.com/ADirin/Sep1_Imp_2025/tree/main/Week4"
    click D1 href "https://github.com/ADirin/Sep1_Imp_2025/tree/main/Week3"
    click E1 href "https://github.com/ADirin/Sep1_Imp_2025/tree/main/Week5"
    click F1 href "https://github.com/ADirin/Sep1_Imp_2025/tree/main/Week6"
    click G1 href "https://github.com/ADirin/Sep1_Imp_2025/tree/main/Week7"
    click H1 href "https://github.com/ADirin/Sep1_Imp_2025/tree/main/Week8"


```

## The Sprints delivery
```mermaid
%% Academic Jira-style Course Board (Updated Sprint Review Weeks)
flowchart LR
    %% Columns
    subgraph Backlog["Backlog"]
        B1[Week 1: Software Engineering Basics]
        B2[Week 2: DevOps Fundamentals]
        B3[Week 3: Dependency Management & Unit Testing]
        B4[Week 4: Coverage Code]
        B5[Week 5: CI with Jenkins]
        B6[Week 6: Docker & Containerization]
        B7[Week 7: SecDevOps]
        B8[Week 8: Final Project Presentation]
    end

    subgraph ToDo["To Do"]
        T1[Prepare GitHub repository & Trello board]
        T2[Plan Sprint 1 tasks]
        T3[Plan Sprint 2 tasks]
        T4[Plan Sprint 3 tasks]
        T5[Plan Sprint 4 tasks]
    end

    subgraph InProgress["In Progress"]
        IP1[Complete Week 1 assignment]
        IP2[Complete Week 2 assignment]
        IP3[Unit Testing and Coverage Code Implementation]
        IP4[CI Pipeline Setup]
        IP5[Docker Project Setup]
        IP6[Docker image & GitHub & Presentation]
    end

    subgraph Done["Done"]
        D1[Sprint 1 Review submitted Week 3]
        D2[Sprint 2 Review submitted Week 5]
        D3[Sprint 3 Review submitted Week 7]
        D4[Sprint 4 Final Project submitted]
    end

    %% Flow between columns
    B1 --> T1
    B1 --> T2
 
    B3 --> T3
    B4 --> T3
    B5 --> T4
    B6 --> T4
    B7 --> T5
    B8 --> T5

    T1 --> IP1
    T2 --> IP1
    T2 --> IP2
    T3 --> IP3
    T3 --> IP4
    T4 --> IP5
    IP6 --> D4
    T5 --> IP6


    IP1 --> D1
  
    IP3 --> D2
  
    IP5 --> D3
    

```
