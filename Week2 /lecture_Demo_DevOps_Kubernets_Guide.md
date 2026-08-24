# Building, Testing, and Deploying a Java Console Calculator: A Complete DevOps Walkthrough

**A student guide covering: console app design, unit testing, Maven, Jenkins CI/CD, code coverage, Docker, Docker Hub, running containers in a browser sandbox, and Kubernetes deployment with minikube.**

[lecture demo_2026] https://github.com/ADirin/calculator_2026f.git
<img width="568" height="53" alt="image" src="https://github.com/user-attachments/assets/67d0b032-03f7-43aa-8887-d51f25972acb" />


---

## Table of Contents

1. [Overview and Learning Goals](#1-overview-and-learning-goals)
2. [Part 1: Design the Console Calculator](#2-part-1-design-the-console-calculator)
3. [Part 2: Generate Unit Tests](#3-part-2-generate-unit-tests)
4. [Part 3: Configure Maven (pom.xml)](#4-part-3-configure-maven-pomxml)
5. [Part 4: Continuous Integration with Jenkins](#5-part-4-continuous-integration-with-jenkins)
6. [Part 5: Code Coverage Report in Jenkins](#6-part-5-code-coverage-report-in-jenkins)
7. [Part 6: Build a Docker Image and Push to Docker Hub](#7-part-6-build-a-docker-image-and-push-to-docker-hub)
8. [Part 7: Run the Console App in a Browser Sandbox (iximiuz Labs)](#8-part-7-run-the-console-app-in-a-browser-sandbox-iximiuz-labs)
9. [Part 8: From Console App to Web App](#9-part-8-from-console-app-to-web-app)
10. [Part 9: Deploy to Kubernetes with minikube](#10-part-9-deploy-to-kubernetes-with-minikube)
11. [Part 10: Run the App on localhost (No Kubernetes)](#11-part-10-run-the-app-on-localhost-no-kubernetes)
12. [Troubleshooting Cheat Sheet](#12-troubleshooting-cheat-sheet)
13. [Minikube cheatsheet](#13-minikube-comments)

---

## 1. Overview and Learning Goals

By the end of this guide you will have:

- Written a simple Java console calculator with reusable, testable methods.
- Written JUnit 5 unit tests for it.
- Configured Maven dependencies and plugins to build, test, and package the project.
- Written a `Jenkinsfile` that builds, tests, and reports code coverage automatically.
- Built a Docker image and published it to Docker Hub.
- Run a container in a free online Docker playground, no local Docker install needed.
- Converted the console app into a small web server so it can be opened in a browser.
- Deployed that web server to a local Kubernetes cluster using minikube, with a `Deployment` and a `Service`.
- Run the same app directly on your machine (`localhost`) without any containers at all.

**Prerequisites:** JDK 21, Maven, Git, Docker Desktop, a free Docker Hub account, and (for the Kubernetes part) `kubectl` and `minikube` installed.

---

## 2. Part 1: Design the Console Calculator

Keep your math logic separate from your input/output logic. This single decision is what makes the class easy to unit test *and* easy to reuse later (e.g., wrapping it in a web server) without rewriting anything.

**`src/main/java/Calculator.java`** — pure logic, no `Scanner`, no `System.out`:

```java
public class Calculator {

    public static double sumMe(double a, double b) {
        return a + b;
    }

    public static double subtractMe(double a, double b) {
        return a - b;
    }

    public static double multiply(double a, double b) {
        return a * b;
    }

    public static double divide(double a, double b) {
        if (b == 0) {
            return 0; // Design choice: avoid throwing/Infinity on divide-by-zero
        }
        return a / b;
    }
}
```

**`src/main/java/CalculatorConsole.java`** — the console entry point that talks to the user:

```java
import java.util.Scanner;

public class CalculatorConsole {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.println("Enter the first number:");
        double a = sc.nextDouble();

        System.out.println("Enter the second number:");
        double b = sc.nextDouble();

        System.out.println("Add: " + Calculator.sumMe(a, b));
        System.out.println("Subtract: " + Calculator.subtractMe(a, b));
        System.out.println("Multiply: " + Calculator.multiply(a, b));
        System.out.println("Divide: " + Calculator.divide(a, b));

        sc.close();
    }
}
```

> **Why split these into two files?** `Calculator` has no side effects, so it's trivial to unit test. `CalculatorConsole` handles I/O and is not something you'd normally unit test directly — you'd test it (if at all) with integration or end-to-end tests. Keeping them separate also means that later, when you want a **web version**, you write a *third* class (`CalculatorServer`, see Part 8) that reuses `Calculator` without touching either existing file.

---

## 3. Part 2: Generate Unit Tests

Create **`src/test/java/CalculatorTest.java`** using JUnit 5:

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;

public class CalculatorTest {

    private static final double DELTA = 1e-9;

    @Test
    public void testSumMe_positiveNumbers() {
        assertEquals(8.0, Calculator.sumMe(5, 3), DELTA);
    }

    @Test
    public void testSumMe_mixedSigns() {
        assertEquals(2.0, Calculator.sumMe(5, -3), DELTA);
    }

    @Test
    public void testSubtractMe_positiveNumbers() {
        assertEquals(2.0, Calculator.subtractMe(5, 3), DELTA);
    }

    @Test
    public void testSubtractMe_negativeResult() {
        assertEquals(-2.0, Calculator.subtractMe(3, 5), DELTA);
    }

    @Test
    public void testMultiply_positiveNumbers() {
        assertEquals(15.0, Calculator.multiply(5, 3), DELTA);
    }

    @Test
    public void testMultiply_byZero() {
        assertEquals(0.0, Calculator.multiply(5, 0), DELTA);
    }

    @Test
    public void testDivide_positiveNumbers() {
        assertEquals(2.5, Calculator.divide(5, 2), DELTA);
    }

    @Test
    public void testDivide_byZero_returnsZero() {
        // Matches the class's own design choice: divide-by-zero returns 0
        assertEquals(0.0, Calculator.divide(5, 0), DELTA);
    }
}
```

**Rule of thumb for students:** for every method, write at least one "normal" case, one edge case (zero, negative numbers), and — if the method has a branch (like `divide`'s `if (b == 0)`) — a test for *each* branch. That is what code coverage tools (Part 6) will measure.

Run the tests locally:

```bash
mvn test
```

---

## 4. Part 3: Configure Maven (pom.xml)

Your `pom.xml` needs three things: JUnit 5 dependencies, a Surefire configuration to run them, and a JaCoCo plugin to measure coverage.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>calculator</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>21</maven.compiler.source>
        <maven.compiler.target>21</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <!-- JUnit 5 (Jupiter) API for writing tests -->
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-api</artifactId>
            <version>5.10.2</version>
            <scope>test</scope>
        </dependency>

        <!-- JUnit 5 engine to actually run the tests -->
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-engine</artifactId>
            <version>5.10.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <finalName>cal</finalName>
        <plugins>

            <!-- Compiles source using the Java version set above -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.13.0</version>
                <configuration>
                    <release>21</release>
                </configuration>
            </plugin>

            <!-- Runs the tests; auto-detects JUnit Platform for Jupiter tests -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.5.1</version>
                <configuration>
                    <includes>
                        <include>**/*Test.java</include>
                    </includes>
                </configuration>
            </plugin>

            <!-- JaCoCo: instruments the code and produces a coverage report -->
            <plugin>
                <groupId>org.jacoco</groupId>
                <artifactId>jacoco-maven-plugin</artifactId>
                <version>0.8.12</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>prepare-agent</goal>
                        </goals>
                    </execution>
                    <execution>
                        <id>report</id>
                        <phase>prepare-package</phase>
                        <goals>
                            <goal>report</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

            <!-- Packages the jar with a runnable Main-Class -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <version>3.4.2</version>
                <configuration>
                    <archive>
                        <manifest>
                            <mainClass>CalculatorConsole</mainClass>
                        </manifest>
                    </archive>
                </configuration>
            </plugin>

        </plugins>
    </build>

</project>
```

> **Important for students:** the `<mainClass>` tag decides which class's `main()` method runs when you do `java -jar cal.jar`. If you later add a web server class (Part 8) and forget to update this tag, your jar will silently keep running the *old* class — this is one of the most common mistakes when converting a console app into a service.

Build and test:

```bash
mvn clean package
```

This produces `target/cal.jar` and, because of the JaCoCo plugin, a coverage report at `target/site/jacoco/index.html`.

---

## 5. Part 4: Continuous Integration with Jenkins

### 5.1 Install and start Jenkins

Easiest path for students: run Jenkins itself in Docker.

```bash
docker run -d --name jenkins \
  -p 8080:8080 -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts
```

Open `http://localhost:8080`, unlock Jenkins using the initial admin password (`docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword`), and install the suggested plugins. Additionally install: **Maven Integration**, **JaCoCo**, and **Docker Pipeline** plugins from *Manage Jenkins → Plugins*.

### 5.2 Create a Pipeline job

In Jenkins: **New Item → Pipeline → name it `calculator-pipeline`**. Under *Pipeline*, choose **Pipeline script from SCM**, point it at your Git repository, and set the script path to `Jenkinsfile`.

### 5.3 Write the Jenkinsfile

Place this at the root of your repository:

```groovy
pipeline {
    agent any

    tools {
        maven 'Maven3'   // Must match a Maven installation name configured in Jenkins
        jdk 'JDK21'       // Must match a JDK installation name configured in Jenkins
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Code Coverage') {
            steps {
                sh 'mvn jacoco:report'
            }
            post {
                always {
                    // Requires the JaCoCo Jenkins plugin
                    jacoco execPattern: '**/target/jacoco.exec',
                           classPattern: '**/target/classes',
                           sourcePattern: '**/src/main/java'
                }
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
                archiveArtifacts artifacts: 'target/cal.jar', fingerprint: true
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        def app = docker.build("yourdockerhubusername/calculator:${env.BUILD_NUMBER}")
                        app.push()
                        app.push('latest')
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed — check the Test or Code Coverage stage first.'
        }
    }
}
```

**Before running this pipeline**, students must:

1. Configure a Maven installation named `Maven3` and a JDK named `JDK21` under *Manage Jenkins → Tools*.
2. Add Docker Hub credentials in Jenkins: *Manage Jenkins → Credentials → Add Credentials*, type "Username with password," ID = `dockerhub-credentials`.
3. Replace `yourdockerhubusername/calculator` with your actual Docker Hub repository name.

Click **Build Now** and watch the stages run in order.

---

## 6. Part 5: Code Coverage Report in Jenkins

The `Code Coverage` stage above already generates and publishes the report, but here's what's happening and how to read it:

1. **`prepare-agent`** (from `pom.xml`) attaches a Java agent during `mvn test` that records which lines execute.
2. **`mvn jacoco:report`** turns that raw execution data (`target/jacoco.exec`) into a human-readable HTML report at `target/site/jacoco/index.html`.
3. The **JaCoCo Jenkins plugin** (installed in 5.1) reads that same data and renders a trend graph and coverage percentage directly on the Jenkins job's page — look for a "Coverage Report" link on the left sidebar of the build.

**To view it locally without Jenkins**, just open the HTML file after running `mvn clean package`:

```bash
mvn clean package
# then open target/site/jacoco/index.html in a browser
```

**What to aim for as a student exercise:** 100% line coverage on `Calculator.java` is realistic and a good target, since it's four small pure functions. Check the report's per-method breakdown — if `divide`'s `if (b == 0)` branch shows partial (yellow) coverage, it means you're missing a test for one side of that condition.

---

## 7. Part 6: Build a Docker Image and Push to Docker Hub

### 7.1 Dockerfile

Use a multi-stage build so the final image doesn't carry the entire Maven/JDK toolchain:

```dockerfile
# Stage 1: build the jar
FROM maven:3.9.6-eclipse-temurin-21 AS build
WORKDIR /build

COPY pom.xml .
RUN mvn dependency:go-offline

COPY . .
RUN mvn package

# Stage 2: slim runtime image
FROM eclipse-temurin:21-jre
WORKDIR /app
COPY --from=build /build/target/cal.jar /app/cal.jar

CMD ["java", "-jar", "/app/cal.jar"]
```

### 7.2 Build and tag

Docker Hub requires images to be tagged as `<your-dockerhub-username>/<repository-name>:<tag>`.

```bash
docker build -t yourdockerhubusername/calculator:1.0 .
docker build -t yourdockerhubusername/calculator:latest .
```

### 7.3 Log in and push

```bash
docker login
docker push yourdockerhubusername/calculator:1.0
docker push yourdockerhubusername/calculator:latest
```

Verify by visiting `https://hub.docker.com/r/yourdockerhubusername/calculator` in a browser — you should see both tags listed.

> **Common student mistake:** forgetting to prefix the image name with your Docker Hub username. `docker push calculator:latest` will fail — Docker Hub needs to know which account's namespace the image belongs to.

---

## 8. Part 7: Run the Console App in a Browser Sandbox (iximiuz Labs)

[iximiuz Labs](https://labs.iximiuz.com/playgrounds/docker) gives you a real Docker environment in the browser — useful if you don't have Docker installed locally, or want to test on a lab machine.

1. Go to **https://labs.iximiuz.com/playgrounds/docker** and start the playground (free account required, sign up if needed).
2. You'll get a terminal running inside a real Linux VM with Docker pre-installed.
3. Pull and run your published image directly from Docker Hub:

```bash
docker pull yourdockerhubusername/calculator:latest
docker run -it yourdockerhubusername/calculator:latest
```

4. Because this is the **console** version (reads from `Scanner`), the `-it` flags are essential — they attach an interactive terminal so `Scanner` has something to read from. Without `-it`, the container will crash immediately with `NoSuchElementException`, the same error you'd get running it non-interactively in Kubernetes (see Part 9's troubleshooting notes).
5. Type numbers when prompted and see the four results print, exactly as it would locally.

This step is a good way for students to confirm the *published* Docker Hub image genuinely works, independent of their own machine's Docker setup.

---

## 9. Part 8: From Console App to Web App

A console app that blocks on `Scanner.nextDouble()` cannot be "opened in a browser" — there's no HTTP server listening on any port. To make the calculator browser-accessible (and to prepare for Part 9's Kubernetes `Service`), add a small web server class that reuses your existing `Calculator` logic untouched.

**`src/main/java/CalculatorServer.java`** — uses only Java's built-in `com.sun.net.httpserver`, so no new Maven dependency is needed:

```java
import com.sun.net.httpserver.HttpExchange;
import com.sun.net.httpserver.HttpHandler;
import com.sun.net.httpserver.HttpServer;

import java.io.IOException;
import java.io.OutputStream;
import java.net.InetSocketAddress;
import java.net.URLDecoder;
import java.nio.charset.StandardCharsets;
import java.util.HashMap;
import java.util.Map;

public class CalculatorServer {

    public static void main(String[] args) throws IOException {
        int port = 8087;
        HttpServer server = HttpServer.create(new InetSocketAddress(port), 0);

        server.createContext("/", new HomeHandler());
        server.createContext("/calculate", new CalculateHandler());

        server.setExecutor(null);
        server.start();
        System.out.println("Calculator server started on port " + port);
    }

    static class HomeHandler implements HttpHandler {
        @Override
        public void handle(HttpExchange exchange) throws IOException {
            String html = "<html><body>"
                    + "<h2>Calculator</h2>"
                    + "<form action=\"/calculate\" method=\"get\">"
                    + "A: <input type=\"text\" name=\"a\"><br>"
                    + "B: <input type=\"text\" name=\"b\"><br>"
                    + "Operation: "
                    + "<select name=\"op\">"
                    + "<option value=\"add\">Add</option>"
                    + "<option value=\"subtract\">Subtract</option>"
                    + "<option value=\"multiply\">Multiply</option>"
                    + "<option value=\"divide\">Divide</option>"
                    + "</select><br>"
                    + "<input type=\"submit\" value=\"Calculate\">"
                    + "</form></body></html>";
            sendResponse(exchange, 200, html);
        }
    }

    static class CalculateHandler implements HttpHandler {
        @Override
        public void handle(HttpExchange exchange) throws IOException {
            Map<String, String> params = parseQuery(exchange.getRequestURI().getQuery());
            try {
                double a = Double.parseDouble(params.get("a"));
                double b = Double.parseDouble(params.get("b"));
                String op = params.get("op");

                double result;
                switch (op) {
                    case "add": result = Calculator.sumMe(a, b); break;
                    case "subtract": result = Calculator.subtractMe(a, b); break;
                    case "multiply": result = Calculator.multiply(a, b); break;
                    case "divide": result = Calculator.divide(a, b); break;
                    default:
                        sendResponse(exchange, 400, "Unknown operation: " + op);
                        return;
                }

                String html = "<html><body><h2>Result</h2><p>"
                        + a + " " + op + " " + b + " = " + result
                        + "</p><a href=\"/\">Back</a></body></html>";
                sendResponse(exchange, 200, html);

            } catch (NullPointerException | NumberFormatException e) {
                sendResponse(exchange, 400, "Invalid input. Provide numeric a, b and a valid op.");
            }
        }
    }

    private static Map<String, String> parseQuery(String query) {
        Map<String, String> result = new HashMap<>();
        if (query == null) return result;
        for (String pair : query.split("&")) {
            String[] kv = pair.split("=", 2);
            if (kv.length == 2) {
                result.put(
                    URLDecoder.decode(kv[0], StandardCharsets.UTF_8),
                    URLDecoder.decode(kv[1], StandardCharsets.UTF_8)
                );
            }
        }
        return result;
    }

    private static void sendResponse(HttpExchange exchange, int statusCode, String body) throws IOException {
        byte[] bytes = body.getBytes(StandardCharsets.UTF_8);
        exchange.getResponseHeaders().set("Content-Type", "text/html; charset=UTF-8");
        exchange.sendResponseHeaders(statusCode, bytes.length);
        OutputStream os = exchange.getResponseBody();
        os.write(bytes);
        os.close();
    }
}
```

**Update `pom.xml`** so the jar launches the server instead of the console app:

```xml
<mainClass>CalculatorServer</mainClass>
```

> **This is the single most common mistake students make at this stage:** adding `CalculatorServer.java` but forgetting to update `<mainClass>` in `pom.xml`. Java only ever runs the class named in the manifest — if it still says `CalculatorConsole`, the jar will keep trying to read from `Scanner`, which has no input in a container or a Kubernetes pod, and will crash instantly with `NoSuchElementException`. Always double check this tag any time you add or rename an entry-point class.

Rebuild:

```bash
mvn clean package
```

---

## 10. Part 9: Deploy to Kubernetes with minikube

![PODS](/Images/yamls.jpg)

The Deployment (teal box) owns and manages the pods — restarting them, keeping the replica count steady. Each pod carries the label app=calculator. The Service (coral box) doesn't know about the Deployment at all — it only knows to look for anything with a matching label, which is why label consistency between the two is critical. Once it finds the pods, it exposes a stable port (80) that forwards to each pod's actual container port (8087), giving you one consistent address to reach even as individual pods get replaced.

### 10.1 Start minikube

```bash
minikube start
minikube status
```

### 10.2 Build the image inside minikube's own Docker daemon

Kubernetes on minikube does not automatically see images built by your regular Docker install. Point your shell at minikube's internal Docker first:

```bash
# macOS/Linux
eval $(minikube docker-env)

# Windows PowerShell
minikube docker-env | Invoke-Expression
```

Then build (from the project root, same Dockerfile as Part 7):

```bash
docker build -t yourdockerhubusername/calculator:latest .
```

> Do this every time you change the code and want to redeploy — a stale image is the #1 cause of "I fixed the bug but it's still broken in Kubernetes."

### 10.3 `deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: calculator-deployment
  labels:
    app: calculator
spec:
  replicas: 2
  selector:
    matchLabels:
      app: calculator
  template:
    metadata:
      labels:
        app: calculator
    spec:
      containers:
        - name: calculator
          image: yourdockerhubusername/calculator:latest
          imagePullPolicy: Never   # Use the local minikube-built image, don't pull from Docker Hub
          ports:
            - containerPort: 8087
          resources:
            requests:
              memory: "256Mi"
              cpu: "250m"
            limits:
              memory: "512Mi"
              cpu: "500m"
```

> `imagePullPolicy: Never` tells Kubernetes to only use the image already present in minikube's Docker (from step 10.2) and never attempt to pull from Docker Hub. If you instead want Kubernetes to pull your published image from Docker Hub, remove this line (or set it to `Always`) — but then you must have actually `docker push`ed the latest version there (Part 7.3), or it will run whatever old version is currently published.

### 10.4 `service.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: calculator-service
spec:
  selector:
    app: calculator
  ports:
    - port: 87
      targetPort: 8087
      protocol: TCP
  type: LoadBalancer
```

### 10.5 Apply and expose

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

kubectl get pods
```

Wait until both pods show `1/1 Running`. Then:

```bash
minikube service calculator-service
```

This opens a tunnel and should launch your default browser directly at the running calculator form. On Windows with the Docker driver, **keep that terminal window open** — the tunnel only works while it's running.

### 10.6 Redeploying after a code change

Every time you edit the Java code:

```bash
# 1. Rebuild inside minikube's Docker (see 10.2)
eval $(minikube docker-env)          # or the PowerShell equivalent
docker build -t yourdockerhubusername/calculator:latest .

# 2. Force the running pods to pick up the new image
kubectl rollout restart deployment calculator-deployment
kubectl get pods --watch
```

Because the tag stays `:latest` and `imagePullPolicy: Never`, Kubernetes won't notice the image changed on its own — `rollout restart` is what forces it to recreate the pods with the freshly built image.

---

## 11. Part 10: Run the App on localhost (No Kubernetes)

For quick local testing, skip Docker and Kubernetes entirely:

```bash
mvn clean package
java -jar target/cal.jar
```

You should see:

```
Calculator server started on port 8087
```

Open a browser to:

```
http://localhost:8087
```

You'll see the calculator form. This is the fastest way to sanity-check code changes before rebuilding Docker images or redeploying to Kubernetes.

---

## 12. Troubleshooting Cheat Sheet

| Symptom | Likely Cause | Fix |
|---|---|---|
| `Error: Unable to access jarfile /app/cal.jar` | Dockerfile's `COPY --from=build` path doesn't match `finalName` in `pom.xml`, or the `CMD` path is wrong | Confirm `<finalName>cal</finalName>` in pom.xml matches `target/cal.jar` in the Dockerfile `COPY` line |
| `NoSuchElementException` at `Scanner.nextDouble()` inside a pod/container | The jar is still running the **console** class (`CalculatorConsole`), not `CalculatorServer` — there's no interactive input in a pod | Update `<mainClass>` in `pom.xml` to `CalculatorServer`, rebuild, redeploy |
| `ErrImagePull` / `ImagePullBackOff` | Image tag in `deployment.yaml` doesn't match what you actually built, or you're not logged in to a private registry | Check the `image:` field matches your `docker build -t ...` tag exactly, including the username prefix |
| `image can't be pulled` with `imagePullPolicy: Never` | The image was built in your **host** Docker, not minikube's internal Docker | Run `minikube docker-env` (or PowerShell equivalent) in the same terminal before `docker build` |
| Browser shows `ERR_CONNECTION_RESET` on the `minikube service` URL | Pods are crashing (`CrashLoopBackOff`) — nothing is listening behind the tunnel | Run `kubectl get pods` and `kubectl logs <pod-name>` to find the real error first |
| `org.junit.jupiter:junit-jupiter-api:jar:${...} was not found` | Maven property didn't resolve, or a stale negative-resolution cache | Hardcode the version instead of a property, and rerun with `mvn clean test -U` |
| `job.batch "x" already exists` on reapply | Kubernetes `Job` objects (unlike Deployments) don't auto-replace on `apply` | `kubectl delete job <name>` before reapplying |

---

## 13. Minikube comments:
1. Get already running services:
```
   -- kubectl get svc
   -- minikube service <calculator-service-name>

```
2. Accessing by forwarding ports
```
  -- kubectl get svc
  -- kubectl port-forward svc/<calculator-service-name> 8080:80
```   
3. Usefull  Pods comments
```
    -- kubectl get pods    
    -- kubectl describe pod <problem-pod>   # check Events section
    -- kubectl logs <problem-pod>

```  
4. Applying development

```
    -- kubectl apply -f <file>.yaml     # create or update from YAML
    -- kubectl delete -f <file>.yaml    # delete what's defined in YAML
    -- kubectl rollout restart deployment <name>   # force re-pull/restart pods
    -- kubectl rollout status deployment <name>    # watch rollout progress
    -- kubectl rollout undo deployment <name>      # revert last rollout
```
