# In-class Assignment 4 (Individual): Introduction to Jenkins Pipeline
**AD / SPRINT 3 / 2 — Fall 2026**

> **Note:** The in-class assignments are compulsory for completing both the sprint implementation and the overall project. Failure to complete the in-class assignments will result in losing the implementation part of your sprint tasks and, consequently, your overall course grade. This will cause you to fail the course if your total points do not reach the required threshold of 2 points.
>
> This assignment requires that you participate in the lecture and do the class exercise at the same time as the teacher.

## Task Objective

Based on your Week 5 in-class assignment (Temperature Converter), implement the following steps and submit the results in Oma:

1. Create a Jenkinsfile (pipeline) to generate a JaCoCo report.
2. Build and run the local Docker image of your application and verify that everything is working correctly.
3. Extend the pipeline to deploy the image to `hub.docker.com`.

## Instructions

### Step 1. Add the Jenkinsfile into your Maven project

- Right-click in the project (IntelliJ) → New → File
- Name: `Jenkinsfile`
- Write the stages in the pipeline in your Jenkinsfile.

> **Note:** You may copy and paste the following code, but make sure you fix it before committing.

```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-username/your-repo.git'
            }
        }
        stage('Build') {
            steps {
                bat 'mvn clean install' // sh for linux and ios
            }
        }
        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }
        stage('Code Coverage') {
            steps {
                bat 'mvn jacoco:report'
            }
        }
        stage('Publish Test Results') {
            steps {
                junit '**/target/surefire-reports/*.xml'
            }
        }
        stage('Publish Coverage Report') {
            steps {
                jacoco()
            }
        }
        // follow the lecture demo for hub.docker.com deployment stages
    }
}
```

### Step 2. Commit the final version of your source code after modifying the pipeline

### Step 3. Create a local Jenkins project

- Select **New Item**.
- Select **Pipeline** project and give it a name (e.g., `Amir_Tempreture_V1_pipeline`), then click OK.
- Select **Pipeline**; in the Definition dropdown, select **Pipeline script from SCM**.
  - Select **Git** → Add the GitHub repository URL.
  - Under **Script Path**, add: `Jenkinsfile`
  - Save.
- Run the project via **Build Now**.

## Submission Instructions

1. GitHub repository link
2. Screenshot of a successfully executed pipeline (e.g., Ocean Blue view or stage view). See the sample output from the class demo (`SVG_301x.java`).
3. Submit the screenshots of your hub.docker.com
4. Screenshots showing the successful execution of the image.
