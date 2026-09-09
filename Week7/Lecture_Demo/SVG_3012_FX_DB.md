# Travel Time Calculator — Application Guide

A JavaFX desktop application that calculates travel time from speed and distance, persists results to a MariaDB database, and displays saved records in a table. This guide covers the application architecture, how to run it, and how the Docker and Jenkins setup works.

---

## 1. What the Application Does

The user enters a **speed** and a **distance**, selects a **travel type** (e.g. Car, Bicycle, Walking, Train) from a dropdown, and clicks **Calculate & Save**. The app:

1. Validates the inputs (no negative speed/distance).
2. Calculates `time = distance / speed`.
3. Saves the result to a MariaDB database.
4. Refreshes a table showing all previously saved records.

---

## 2. Application Architecture

The project uses a flat `app` package (no subpackages) for simplicity. Each class has a single responsibility:

```
src/main/java/app/
├── Main.java              — JavaFX UI and application entry point
├── DatabaseConnection.java — JDBC connection factory
├── TravelType.java         — Model: a travel type (Car, Bicycle, etc.)
├── TravelRecord.java       — Model: one saved speed/distance/time entry
├── TravelCalculator.java   — Core calculation + input validation logic
├── TravelTypeDAO.java      — Reads travel types from the database
└── TravelRecordDAO.java    — Reads/writes travel records to the database

src/test/java/app/
├── TravelTypeTest.java
├── TravelRecordTest.java
└── TravelCalculatorTest.java
```

### Class Responsibilities

| Class | Responsibility |
|---|---|
| **`Main`** | Builds the JavaFX UI (form, table), wires button actions, and calls the DAOs/calculator. This is the only class that touches JavaFX components. |
| **`DatabaseConnection`** | Centralizes the JDBC URL, username, and password, and opens a `Connection`. Every DAO calls this rather than hardcoding connection details itself. |
| **`TravelType`** | Simple model representing a row from the `travel_type` table. Its `toString()` is what the `ComboBox` displays. |
| **`TravelRecord`** | Simple model representing a row from the `travel_record` table. |
| **`TravelCalculator`** | Pure logic, no I/O: `validateInputs()` rejects negative values, `timeCal()` computes `distance / speed`. This is the class the unit tests target directly, since it has no JavaFX or database dependency. |
| **`TravelTypeDAO`** | Fetches the list of travel types for the dropdown. |
| **`TravelRecordDAO`** | Inserts a new record and fetches all saved records for the table. |

### Why `Main` and the DAOs aren't unit tested directly
- `Main` extends `javafx.application.Application` and manipulates live UI components — testing it meaningfully requires a UI-testing framework (e.g. TestFX), not plain JUnit.
- The DAOs open a real JDBC connection to MariaDB — testing them as pure "units" would require mocking the database (e.g. Mockito + an in-memory H2 database), which is a separate, optional step from core logic testing.
- `TravelCalculator` has none of these dependencies, which is exactly why it's the one covered by JUnit tests.

---

## 3. Database Schema (MariaDB, via HeidiSQL)

Two related tables: a lookup table and a records table with a foreign key.

```sql
CREATE DATABASE IF NOT EXISTS svg_travel_db
    CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

USE svg_travel_db;

CREATE TABLE travel_type (
    id          INT AUTO_INCREMENT PRIMARY KEY,
    type_name   VARCHAR(50) NOT NULL UNIQUE
);

INSERT INTO travel_type (type_name) VALUES
    ('Car'), ('Bicycle'), ('Walking'), ('Train');

CREATE TABLE travel_record (
    id              INT AUTO_INCREMENT PRIMARY KEY,
    speed           DOUBLE NOT NULL,
    distance        DOUBLE NOT NULL,
    time_taken      DOUBLE NOT NULL,
    travel_type_id  INT NOT NULL,
    created_at      DATETIME DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT fk_travel_type
        FOREIGN KEY (travel_type_id) REFERENCES travel_type(id)
        ON DELETE RESTRICT
);
```

Run this once in HeidiSQL's Query tab (F9 to execute) before running the app for the first time.

### Allowing remote/container connections
By default MariaDB's `root` user is often restricted to `localhost` only, which blocks connections from Docker. If connecting from a container, run:

```sql
CREATE USER 'root'@'%' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON svg_travel_db.* TO 'root'@'%';
FLUSH PRIVILEGES;
```

Also check `my.ini` for `bind-address = 127.0.0.1` — if present, either remove it or set it to `0.0.0.0`, then restart the MariaDB service.

---

## 4. Running Locally (no Docker)

```
mvn clean install       # compile + run tests
mvn javafx:run          # launch the JavaFX app
```

Make sure `DatabaseConnection.java` points at `localhost` and the DB is reachable, since this runs directly on your machine.

---

## 5. Running in Docker

### Why the Dockerfile needs extra setup
JavaFX isn't bundled with the standard JDK, and rendering a GUI from inside a Linux container onto your Windows desktop requires an X11 server and specific system libraries.

```dockerfile
FROM eclipse-temurin:21-jdk

WORKDIR /app

# GUI libraries required for JavaFX to render via X11
RUN apt-get update && apt-get install -y \
    libx11-6 libxext6 libxrender1 libxtst6 libxi6 libgtk-3-0 mesa-utils wget unzip \
    && rm -rf /var/lib/apt/lists/*

# JavaFX SDK (not bundled with the JDK)
RUN mkdir -p /javafx-sdk \
    && wget -O javafx.zip https://download2.gluonhq.com/openjfx/21/openjfx-21_linux-x64_bin-sdk.zip \
    && unzip javafx.zip -d /javafx-sdk \
    && mv /javafx-sdk/javafx-sdk-21/lib /javafx-sdk/lib \
    && rm -rf /javafx-sdk/javafx-sdk-21 javafx.zip

COPY target/svg_3012_db.jar app.jar

ENV DISPLAY=host.docker.internal:0.0

CMD ["java", \
     "--module-path", "/javafx-sdk/lib", \
     "--add-modules", "javafx.controls,javafx.fxml", \
     "-Dprism.order=sw", \
     "-jar", "app.jar"]
```

Key details:
- **`-Dprism.order=sw`** forces JavaFX to use software rendering instead of OpenGL/GLX, since X servers on Windows (VcXsrv/Xming) typically don't support the GLX version JavaFX wants by default.
- **`DISPLAY=host.docker.internal:0.0`** tells the container where to send its GUI output — back to an X server running on your Windows host.

### Prerequisites on Windows before running
1. Install and run an X server — **VcXsrv** is recommended. Launch via XLaunch: Multiple windows → Display number `0` → Start no client → **check "Disable access control"**.
2. Allow it through Windows Firewall for Private networks when prompted.
3. If the app can't reach MariaDB, make sure `DatabaseConnection`'s URL uses `host.docker.internal` instead of `localhost` (see Section 3), since `localhost` inside a container refers to the container itself, not your Windows host.

### Build and run

```
mvn clean package
docker build -t amirdirin/svg_3012_db .
docker run --rm -e DISPLAY=host.docker.internal:0.0 amirdirin/svg_3012_db
```

### Quick diagnostic if the window never appears
Test raw X11 connectivity, independent of JavaFX:
```
docker run --rm -e DISPLAY=host.docker.internal:0.0 eclipse-temurin:21-jdk bash -c "apt-get update && apt-get install -y x11-apps && xclock"
```
If `xclock` doesn't appear either, the issue is your X server/firewall setup, not the JavaFX app.

---

## 6. Jenkins Pipeline

The `Jenkinsfile` automates: checkout → build → test → coverage → publish results → Docker build → Docker push.

```groovy
pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    environment {
        PATH = "C:\\Program Files\\Docker\\Docker\\resources\\bin;${env.PATH}"
        DOCKERHUB_CREDENTIALS_ID = 'Docker_Hub'
        DOCKERHUB_REPO = 'amirdirin/svg_3012_2026'
        DOCKER_IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/ADirin/svg_3012_pipeline_docker.git'
            }
        }
        stage('Build') {
            steps {
                bat 'mvn clean install'
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

### Requirements for each stage to succeed

| Stage | Requirement |
|---|---|
| **Checkout** | The GitHub repo must contain the full project, including `pom.xml`, `Dockerfile`, and `Jenkinsfile` at the root (or correct relative paths). |
| **Build / Test** | `pom.xml` must be valid — a missing or misspelled `<properties>` entry (e.g. an undefined `${junit.version}`) will fail project loading before any stage runs. |
| **Code Coverage** | The `jacoco-maven-plugin` **must be declared under `<build><plugins>`** in `pom.xml` (not just referenced as a Maven goal) — otherwise `mvn jacoco:report` fails with "No plugin found for prefix 'jacoco'". |
| **Publish Test Results** | Requires `mvn test` to have actually run and produced `target/surefire-reports/*.xml`. |
| **Publish Coverage Report** | Requires the JaCoCo `prepare-agent` execution to have run during `test`, and `report` to have generated `target/site/jacoco/`. |
| **Build Docker Image** | Requires a file literally named `Dockerfile` (capital D, no extension) present in the workspace root — a missing or misnamed file causes "failed to read dockerfile: open Dockerfile: no such file or directory". |
| **Push Docker Image** | Requires Docker Hub credentials configured in Jenkins under the ID referenced by `DOCKERHUB_CREDENTIALS_ID` (`Docker_Hub` here), with push access to `DOCKERHUB_REPO`. |

### Required `pom.xml` plugin for JaCoCo

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.openjfx</groupId>
            <artifactId>javafx-maven-plugin</artifactId>
            <version>0.0.8</version>
            <configuration>
                <mainClass>app.Main</mainClass>
            </configuration>
        </plugin>

        <plugin>
            <groupId>org.jacoco</groupId>
            <artifactId>jacoco-maven-plugin</artifactId>
            <version>0.8.12</version>
            <executions>
                <execution>
                    <id>prepare-agent</id>
                    <goals><goal>prepare-agent</goal></goals>
                </execution>
                <execution>
                    <id>report</id>
                    <phase>test</phase>
                    <goals><goal>report</goal></goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

### Setting up the Jenkins job (one-time, local Jenkins)

1. **New Item** → select **Pipeline** → give it a name (e.g. `svg_3012_pipeline_docker`) → OK.
2. Under **Pipeline**, set Definition to **Pipeline script from SCM**.
3. Select **Git**, add the GitHub repository URL.
4. Under **Script Path**, enter `Jenkinsfile`.
5. Save, then click **Build Now**.

---

## 7. Common Errors and Fixes (Quick Reference)

| Symptom | Cause | Fix |
|---|---|---|
| `'dependencies.dependency.version' ... must be a valid version but is '${junit.version}'` | Property referenced but never defined in `<properties>` | Add `<junit.version>5.10.2</junit.version>` to `<properties>`, or hardcode the version directly |
| `No plugin found for prefix 'java'` | Ran `mvn java` or similar — not a real Maven goal | Use `mvn javafx:run` to launch the app, or `mvn exec:java -Dexec.mainClass="app.Main"` with the exec plugin configured |
| `No plugin found for prefix 'jacoco'` | JaCoCo not declared under `<build><plugins>` | Add the `jacoco-maven-plugin` block shown above |
| `failed to read dockerfile: open Dockerfile: no such file or directory` | `Dockerfile` missing, misnamed, or not committed to the repo | Confirm `git ls-files \| grep -i dockerfile` shows it; check exact casing and no extension |
| `GLX version 1.2 ... 1.3 or higher is required` | JavaFX trying to use hardware OpenGL rendering via a limited X server | Add `-Dprism.order=sw` to force software rendering |
| App builds/runs but window never appears (`QuantumRenderer: shutdown`) | X11 connectivity issue, or an unhandled exception during `start()` (often a DB connection failure) | Test with `xclock` to isolate X11 vs. app issues; wrap `launch(args)` in try/catch to surface hidden exceptions |
| App can't connect to MariaDB from inside a container | `localhost` inside a container refers to the container itself, not the host | Use `host.docker.internal` in the JDBC URL (host MariaDB) or the DB container's service name (containerized MariaDB) |
| `Access denied for user 'root'@...` | MariaDB user not permitted to connect from non-localhost | `CREATE USER 'root'@'%' ...` and `GRANT ALL PRIVILEGES ...` in HeidiSQL, then `FLUSH PRIVILEGES` |
