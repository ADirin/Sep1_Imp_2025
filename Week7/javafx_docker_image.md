# JavaFX Docker Image with Xming (Windows)

- JavaFX apps need a graphical display to show windows and UI. Docker containers (especially Linux-based ones) don’t have a display of their own.
- so, we tell the container to send the GUI to your Windows desktop using:
- that is why we need to install **VcXsrv** or [**Xming**] (https://sourceforge.net/projects/xming/) on Windows to run a JavaFX app in Docker
- These tools receive the GUI output from the container and show it on your screen.


## The process is:

         1. Build a fat JAR with Maven (using maven-shade-plugin).

         2. Create a Docker image with OpenJDK 17.

         3. Install GUI/X11 libraries inside the container.

         4. Download and extract the JavaFX SDK.

         5. Copy the application JAR into the container.

         6. Run the JavaFX app with --module-path pointing to the JavaFX SDK.

         7. Forward the display to Windows using Xming.

---
## Short instruction for **MAC users**

macOS: Install & Configure **XQuartz**
Install:
```
brew install --cask xquartz
````
Start:
````
open -a XQuartz
````
Enable network access:
````
Open XQuartz Preferences (Cmd + ,) → Security → Check Allow connections from network clients
````
Restart:
````
open -a XQuartz
````

## Configure X11 on the System
- Set DISPLAY Variable

macOS:
````
export DISPLAY=:0
````

Allow X11 Connections
````
xhost +localhost
````
## Run the JavaFX Application in Docker
````
docker run -e DISPLAY=host.docker.internal:0 \
           -e DB_HOST=host.docker.internal \
           -p 3000:3000 \
           -v /tmp/.X11-unix:/tmp/.X11-unix \
           flash_card
````
## Why We Need DB_HOST and Port 3000

### DB_HOST
- The DB_HOST environment variable specifies the database server's address. Inside Docker, localhost refers to the container, not the host. Setting DB_HOST=host.docker.internal ensures the JavaFX app connects to the MariaDB database on the host machine.


### Port 3000
- Port 3000 is used for communication with external web services (e.g., OAuth providers).
Mapping with -p 3000:3000 ensures authentication redirects work properly.


----------------------------------------------------------------------------------------------

## 1. Example JAVAFX Project Structure

Your project should look like this:

````
javafx-celsius-converter/
├─ src/main/java/com/example/Main.java
├─ pom.xml
└─ target/
└─ Jenkinsfiles
└─ Dockfile
````

- `Main.java` contains your JavaFX application.
- `pom.xml` contains Maven configuration to build a **fat JAR** with JavaFX dependencies.

---

## 2. Maven Configuration (pom.xml)

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                             http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>javafx-celsius-converter</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <java.version>17</java.version>
        <javafx.version>21</javafx.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.openjfx</groupId>
            <artifactId>javafx-controls</artifactId>
            <version>${javafx.version}</version>
        </dependency>
        <dependency>
            <groupId>org.openjfx</groupId>
            <artifactId>javafx-fxml</artifactId>
            <version>${javafx.version}</version>
        </dependency>
    </dependencies>

    <build>
        <finalName>tempconv</finalName>
        <plugins>
            <!-- Compiler plugin -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.10.1</version>
                <configuration>
                    <source>${java.version}</source>
                    <target>${java.version}</target>
                </configuration>
            </plugin>

            <!-- Shade plugin to create a fat JAR -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>3.4.1</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <transformers>
                                <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                    <mainClass>com.example.Main</mainClass>
                                </transformer>
                            </transformers>
                        </configuration>
                    </execution>
                </executions>
            </plugin>

            <!-- JavaFX Maven plugin (optional for local dev) -->
            <plugin>
                <groupId>org.openjfx</groupId>
                <artifactId>javafx-maven-plugin</artifactId>
                <version>0.0.8</version>
                <configuration>
                    <mainClass>com.example.Main</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
````

# 3. JavaFX Main Class (Main.java)

````
package com.example;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class Main extends Application {
    @Override
    public void start(Stage stage) {
        Label label = new Label("Enter Celsius:");
        TextField celsiusInput = new TextField();
        Button convertButton = new Button("Convert");
        Label resultLabel = new Label();

        convertButton.setOnAction(e -> {
            try {
                double celsius = Double.parseDouble(celsiusInput.getText());
                double fahrenheit = (celsius * 9 / 5) + 32;
                resultLabel.setText("Fahrenheit: " + fahrenheit);
            } catch (NumberFormatException ex) {
                resultLabel.setText("Invalid input!");
            }
        });

        VBox root = new VBox(10, label, celsiusInput, convertButton, resultLabel);
        Scene scene = new Scene(root, 300, 200);
        stage.setTitle("Celsius to Fahrenheit");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
````
# FROM openjdk:17-slim

````
WORKDIR /app

# Install GUI libraries
RUN apt-get update && apt-get install -y \
    libx11-6 libxext6 libxrender1 libxtst6 libxi6 libgtk-3-0 mesa-utils wget unzip \
    && rm -rf /var/lib/apt/lists/*

# Download and unzip JavaFX Linux SDK
RUN mkdir -p /javafx-sdk \
    && wget -O javafx.zip https://download2.gluonhq.com/openjfx/21.0.2/openjfx-21.0.2_linux-x64_bin-sdk.zip \
    && unzip javafx.zip -d /javafx-sdk \
    && mv /javafx-sdk/javafx-sdk-21.0.2/lib /javafx-sdk/lib \
    && rm -rf /javafx-sdk/javafx-sdk-21.0.2 javafx.zip

# Copy your fat JAR
COPY target/tempconv.jar app.jar

# Set X11 display (Windows host with Xming/X11)
ENV DISPLAY=host.docker.internal:0.0

# Run JavaFX app
CMD ["java", "--module-path", "/javafx-sdk/lib", "--add-modules", "javafx.controls,javafx.fxml", "-jar", "app.jar"]

````
# 5. Build and Run

1. Build fat JAR:
````
mvn clean package
````

This will generate target/tempconv.jar.

2. Build Docker image:

````
docker build -t fx-tempconv .
````

3. Run Xming

  - Make sure Xming is running on Windows.

  - Allow connections from localhost / 127.0.0.1.

## 4. Run Docker container:
````
docker run --rm -e DISPLAY=host.docker.internal:0.0 fx-tempconv
````

You should see your JavaFX window displayed via Xming.

5. Notes

- If the application cannot find javafx.controls, ensure the --module-path points correctly to /javafx-sdk/lib.

- For a simpler approach, you can package JavaFX inside the fat JAR and skip module-path configuration in Docker.

- Make sure firewall/Xming allows connections from Docker.
--------------------------

6.  Sample Jenkins file to create an image remotely:

```groovey
pipeline {
    agent any
    tools{
        maven 'Maven3'
        
    }

    environment {
        PATH = "C:\\Program Files\\Docker\\Docker\\resources\\bin;${env.PATH}"
        DOCKERHUB_CREDENTIALS_ID = 'Docker_Hub'
        DOCKER_IMAGE = 'amirdirin/travelcalculator'
        DOCKER_TAG = 'latest'
    }

    stages {
        stage('Setup Maven') {
            steps {
                script {
                    def mvnHome = tool name: 'Maven3', type: 'maven'
                    env.PATH = "${mvnHome}/bin:${env.PATH}"
                }
            }
        }

        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/ADirin/TimeCal.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn clean package -DskipTests'
                    } else {
                        bat 'mvn clean package -DskipTests'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn test'
                    } else {
                        bat 'mvn test'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    if (isUnix()) {
                        sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                    } else {
                        bat "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                    }
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', env.DOCKERHUB_CREDENTIALS_ID) {
                        docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push()
                    }
                }
            }
        }
    }

    post {
        always {
            junit '**/target/surefire-reports/*.xml'
            jacoco execPattern: '**/target/jacoco.exec'
        }
    }
}


```
## Q&A and Important Note
Make sure the following steps are taken prior to the execution of the file
1.  Ensure JavaFX SDK is downloaded and extracted for example  `C:\javafx-sdk-17.0.16\`
2.  Add JavaFX SDK to Project Libraries
         - Go to File > Project Structure > Libraries
         - `C:\javafx-sdk-17.0.16\lib`
         - Select all .jar files inside the lib folder
         - Click OK and apply changes
You should have a folder
3. Edit Run Configuration
         - Go to Run > Edit Configurations
         - Select your main class (TravelTimeCalculator)
         - In the VM options field, add:
         `--module-path "C:\javafx-sdk-17.0.16\lib" --add-modules javafx.controls,javafx.fxml

4.  The code that is shared in this tutorial (githun site) are sample you have to modify based on your own project, for exmple the Jenkins script (Jenkinsfile) or dockerfile.
   
[Run-Edit Config](/Images/Edit_cong.jpg)
