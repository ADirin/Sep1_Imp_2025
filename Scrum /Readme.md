## Software Development Methodologies Flowchart Overview

This flowchart visualizes the main processes and cycles of three software development methodologies: **SDLC**, **Agile**, and **DevOps**.  
All three methodologies start from a common **Start** node and end at their respective process completions.

### Node Groups and Explanations

| No. | Node Group | Key Explanation |
|----:|------------|-----------------|
| 1 | **SDLC** | Traditional linear process: moves from requirements to maintenance with clear, sequential phases. |
| 2 | **Agile** | Iterative sprints involving planning, development, testing, and review, repeated until completion. |
| 3 | **DevOps** | Continuous integration, delivery, testing, and monitoring with a strong emphasis on automation and feedback loops. |

```` mermaid
flowchart TD
    %% DevOps vs Agile vs SDLC Process Comparison
    A(["Start"]) --> B1("SDLC: Requirement Analysis")
    B1 --> C1("SDLC: System Design")
    C1 --> D1("SDLC: Implementation")
    D1 --> E1("SDLC: Testing")
    E1 --> F1("SDLC: Deployment")
    F1 --> G1("SDLC: Maintenance")
    G1 --> H1(["End SDLC"])

    A --> B2("Agile: Plan Sprint")
    B2 --> C2("Agile: Develop Features")
    C2 --> D2("Agile: Test Features")
    D2 --> E2("Agile: Review & Retrospective")
    E2 --> F2{"More Sprints?"}
    F2 -->|Yes| B2
    F2 -->|No| G2(["End Agile"])

    A --> B3("DevOps: Continuous Integration")
    B3 --> C3("DevOps: Continuous Testing")
    C3 --> D3("DevOps: Continuous Delivery")
    D3 --> E3("DevOps: Continuous Monitoring & Feedback")
    E3 --> F3(["End DevOps"])

    classDef startend fill:#f96,stroke:#333,stroke-width:2px;
    class A,H1,G2,F3 startend
````
