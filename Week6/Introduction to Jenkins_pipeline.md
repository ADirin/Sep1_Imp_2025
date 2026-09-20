### [Lecture demo] https://github.com/ADirin/Week6_3011_demo.git

#  Using pipeline in Jenkins:

Jenkins Pipeline is a powerful tool for managing complex build and deployment workflows, making it an essential skill for students interested in DevOps and continuous integration/continuous delivery (CI/CD).

## Jenkins Pipeline
**Jenkins Pipeline** is a suite of plugins in Jenkins, an open-source automation server, that supports implementing and integrating continuous delivery pipelines. It allows you to define your entire build process, from code commit to deployment, as code. This is done using a domain-specific language (DSL) called Groovy.

### Key Features:
- **Declarative and Scripted Pipelines**: Two types of syntax to define your pipeline, making it flexible and easy to use.
- **Stages and Steps**: Break down your pipeline into stages (e.g., Build, Test, Deploy) and steps (individual tasks within stages).
- **Version Control Integration**: Easily integrate with Git, SVN, and other version control systems.
- **Extensibility**: Add plugins to extend Jenkins' functionality, such as integrating with various tools and platforms.

### Benefits:
- **Automation**: Automate repetitive tasks, reducing manual errors.
- **Consistency**: Ensure consistent build and deployment processes.
- **Visibility**: Gain insights into your pipeline's status and performance through visualizations and logs.


# Modify Pipeline to Use Declarative Syntax Correctly

Make sure your pipeline stages are clearly defined and contain actual steps. In your current script, you only have one stage, which might be too minimal to display a clear visual pipeline. You can add multiple stages and steps to see the pipeline stages more clearly.

## Options for Applying Pipelines in Jenkins

There are several ways to apply pipelines in Jenkins, each with its own advantages. Here are some common options:

### 1. Jenkinsfile in GitHub
- **Description**: Store your pipeline definition in a `Jenkinsfile` within your project's repository on GitHub.
- **Advantages**:
  - **Version Control**: Track changes to your pipeline alongside your code.
  - **Collaboration**: Easily share and review pipeline code with team members.
  - **Consistency**: Ensure the same pipeline is used across different Jenkins instances.

### 2. Pipeline Script Directly in Jenkins
- **Description**: Write and manage your pipeline script directly within the Jenkins user interface.
- **Advantages**:
  - **Quick Setup**: Easily create and modify pipelines without needing to push changes to a repository.
  - **Immediate Feedback**: Test and debug pipeline scripts directly in Jenkins.

### 3. Shared Libraries
- **Description**: Use shared libraries to define reusable pipeline code that can be used across multiple projects.
- **Advantages**:
  - **Reusability**: Share common pipeline code across different projects.
  - **Maintainability**: Centralize updates and maintenance of pipeline code.

### 4. Declarative vs. Scripted Pipelines
- **Declarative Pipelines**:
  - **Description**: Use a simplified, structured syntax to define your pipeline.
  - **Advantages**: Easier to read and write, with built-in error handling and validation.
- **Scripted Pipelines**:
  - **Description**: Use Groovy scripting for more complex and flexible pipeline definitions.
  - **Advantages**: Greater control and flexibility for advanced use cases.

### 5. Pipeline as Code with Multibranch Pipelines
- **Description**: Automatically create pipelines for each branch in your repository.
- **Advantages**:
  - **Branch-Specific Pipelines**: Customize pipelines for different branches.
  - **Automation**: Automatically detect and create pipelines for new branches.

Each method has its own use cases and benefits, so you can choose the one that best fits your project's needs.


## Example Jenkins pipeline:

```groovy
pipeline {
    agent any
    stages {
        stage('Compile') {
            steps {
                echo 'Compile stage completed'
            }
        }
        stage('Test') {
            steps {
                echo 'Test stage completed'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploy stage completed'
            }
        }
    }
}
```



## 2. Using a Pipeline with a Jenkinsfile in GitHub

### Steps to Set Up a Jenkins Pipeline with a Jenkinsfile

1. **Create a Jenkinsfile in GitHub**
   - Add a `Jenkinsfile` to your GitHub repository with the following content:
   - **NOTE**: if you want to use the following pipeline in windows environment make sure
     1. replace sh to **bat** throughout the following script, for Linux or Mac OS the sh works fine
     2. You have to add the git-repo link in the git tag

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
        }
    }
    ```
# Steps to Ensure You See the Stages:

## 1. Ensure You're Using a  Pipeline Job

## 2. Make Sure You Have Jenkins Blue Ocean Installed (Optional)
**Blue Ocean** is a plugin that provides a better UI for Jenkins pipelines, making it easier to visualize stages.
If you don't have it installed:
1. Go to **Manage Jenkins > Manage Plugins**.
2. Search for **Blue Ocean** under the "Available" tab and install it.
3. Once installed, you can view your pipeline visually by clicking on the **Blue Ocean** option in the Jenkins dashboard.

## 3. Run the Pipeline Job
1. Click on **Build Now** for your pipeline job.
2. Go to **Build History > Console Output** to view the output.
3. If **Blue Ocean** is installed, you can also view the graphical representation of the pipeline by clicking on the **Blue Ocean** link.

## 4. Check the Stages Tab
Even without **Blue Ocean**, the regular Jenkins UI should show a **Stages** tab or **Pipeline Steps** in the pipeline build details, where you can see each of your pipeline stages and their status (e.g., Passed, Failed, etc.).
----------------------------------------------------------------------------------------------


### Sequence Diagram
![Sequence Diagram](/Images/Seq.png)

# IMPORTANT setup
## Jenkins configuration to recognize the docker (follow the step beloow)

1.  In jenkins go to manage jenkins--> select plugins in avaliable plugins select the docker, docker API plugin
2.  In jenkins go to tools and find the docker (usuall at the end of the page), give the docker desktop path.
    -  You can find the path from the edit system enviroment--> environment variable ---> path and copy the path for example 'C:\Program Files\Docker\Docker\resources\bin'
  
  
![Docker path](/Images/dockerSetUP.jpg)
   
4. In jenkins go to creadention --> select ayatem--> select gloabale credention ---> add credentials

![Docker path](/Images/dock2.jpg)

    - give your docker **destop user name**
    - go to hub.docker.com
    - go to account setting

![Docker path](/Images/hub1.jpg)

  
    - select personal access tokens
        - generate token
        - experation: none
        - read /write permission
        - copy the generate tokens
![Docker path](/Images/act2.jpg)

  6.  In Jenkins for password paste the token you have created
  7.  Add a name for the Docker credentilas : Docker-Hub

## 3. Creating a Jenkins Pipeline in GitHUb

### Steps to Create a Declarative Pipeline

1. **Create a New Pipeline Job in Jenkins**
   - Go to Jenkins dashboard.
   - Click on "New Item".
   - Enter a job name.
   - Select "Pipeline" and click "OK".

2. **Configure the Pipeline**
   - Under the "Pipeline" section, choose "Pipeline script from SCM" and add the github repository URL (public)
   - Write the following script:

  ```groovy
 
  pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    environment {
        PATH = "C:\\Program Files\\Docker\\Docker\\resources\\bin;${env.PATH}"
        DOCKERHUB_CREDENTIALS_ID = 'Docker_Hub'
        DOCKERHUB_REPO = 'amirdirin/demo1_2026'
        DOCKER_IMAGE_TAG = 'latest'
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/ADirin/lectDemo_1_f2026.git'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'mvn clean test'
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

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}")
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS_ID) {
                        docker.image("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}").push()
                    }
                }
            }
        }

    }
 }

```

3. **Save and Run**
   - Save the job and click "Build Now" to run the pipeline.


## 4. The Image generation process

![Deployment Process](/Images/dep.png)




It starts with a developer, sitting at their desk, finishing a fix.

They type `git push`, and their code leaves their machine. It doesn't go directly anywhere important yet — it's just handed off, waiting for something to notice it.

**That "something" is Jenkins.**

Jenkins is drawn as a big container in the diagram because it's not one single action — it's a whole workspace where several things happen one after another. The moment it senses the new code has arrived, it opens the door and lets the code in. Inside, the first thing it does is hand the code to its **pipeline stages** — this is the disciplined part of the story: checkout the code, compile it, run the tests. Nothing glamorous happens here, just careful checking. If this part goes badly, the story would end here (that's the branch we drew in the activity diagram) — but let's say everything passes.

**Now the code moves next door, inside the same building, to the Docker engine.**

This is a key detail the diagram is trying to make obvious: Docker isn't some separate company or service Jenkins has to call long-distance — it's a tool sitting right there in the same room. The pipeline stages hand off a clean, tested build to the Docker engine, and the Docker engine does something transformative: it doesn't just save the code, it *packages* it — wraps it up with everything it needs (the right Java version, libraries, configuration) into a self-contained image, like sealing a finished product in a shipping crate.

**With the crate sealed, it's time to leave the building.**

The Docker engine carries that image out of Jenkins entirely and hands it off to **Docker Hub** — and this is the first time in the story something happens *outside* Jenkins's walls. Docker Hub is a public warehouse: it doesn't build anything, doesn't test anything, it just stores images and makes them available to whoever needs to pull one down later.

**Finally, somewhere else entirely, a target server gets the call.**

This server isn't part of Jenkins, isn't part of the developer's laptop — it's wherever the application actually needs to run in the real world. It doesn't wait for Jenkins to deliver anything directly. Instead, it reaches out on its own timeline and says, in effect, "give me the latest image" — a `docker pull` — takes delivery of that same sealed crate, and starts running it.


### Sequence Diagram

![Sequence Diagram](/Images/Seq2.png)







