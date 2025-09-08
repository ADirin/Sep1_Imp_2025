# 📝Introduction to Maven
Apache Maven is a build automation and project management tool primarily used for Java projects. It uses a Project Object Model (POM) to manage project dependencies, build configurations, and other project-related settings.

* Maven is a powerful build automation tool primarily used for Java projects. It simplifies managing dependencies, building, and distributing Java-based projects. Maven uses a declarative approach to describe project structure, dependencies, and build processes, allowing developers to focus on coding rather than project configuration. Why use Maven:
  - Dependency Management: Automatically downloads and manages project dependencies.
  - Standardized Build Process: Provides a consistent build process across projects.
  - Extensibility: Supports a wide range of plugins to extend its functionality.
  - Convention Over Configuration: Follows conventions to minimize the need for configuration.
    
# How Maven Works in IntelliJ IDEA
* IntelliJ IDEA  recognizes Maven projects and provides built-in support for executing Maven commands directly from the IDE. IntelliJ automatically downloads dependencies specified in the pom.xml file and manages the project's build lifecycle.
  
## Sample Intellij maven project structure
```
MyMavenProject/  
├── pom.xml                     # Project Object Model (Maven configuration file)  
├── src/  
│   ├── main/  
│   │   ├── java/               # Application source code  
│   │   │   └── com/  
│   │   │       └── example/  
│   │   │           └── App.java  
│   │   └── resources/          # Application resources (config files, etc.)  
│   │       └── application.properties  
│   └── test/  
│       ├── java/               # Test source code  
│       │   └── com/  
│       │       └── example/  
│       │           └── AppTest.java  
│       └── resources/          # Test resources (if needed)  
│  
└── target/                     # Generated build output (compiled classes, JARs, reports)  


```
### 👉 This is the default Maven + IntelliJ project layout:

- pom.xml — the Maven build file.
- src/main/java — main application source code.
- src/test/java — unit tests (e.g., JUnit).
- resources — configuration files, properties, static resources.
- target — build output (created after running Maven).

## Key concpet of Maven

### 1. Project Object Model (POM)
The **pom.xml** file is the core of a Maven project. It contains the project’s configuration details such as dependencies, plugins, goals, and build settings.

Basic Structure of pom.xml:
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/POM/4.0.0/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>my-project</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>
    <dependencies>
        <!-- Dependencies go here -->
    </dependencies>
    <build>
        <plugins>
            <!-- Plugins go here -->
        </plugins>
    </build>
</project>



```
- groupId: Defines the group or organization that the project belongs to.
- artifactId: Defines the name of the project.
- version: Specifies the version of the project.
- packaging: Defines the type of artifact (e.g., JAR, WAR).
- dependencies: Lists the external libraries required by the project.
- build: Contains build settings and plugins.

### 2. Dependencies
**Dependencies** are external libraries or frameworks that your project needs to compile and run. Maven manages these dependencies and their versions.

- To download dependencies, it is not needed to visit the official website of each software. 
  - It is enough to visit
           [Maven Repo](https://mvnrepository.com/)

Adding Dependencies:

```xml
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>5.8</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```
- groupId: The group ID of the dependency.
- artifactId: The artifact ID of the dependency.
- version: The version of the dependency.
- scope: The scope of the dependency (e.g., compile, test).

### 3. Plugins
**Plugins** extend Maven's functionality. They perform specific tasks such as compiling code, running tests, packaging applications, etc.

Configuring Plugins:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
            </configuration>
        </plugin>
    </plugins>
</build>
```
- groupId: The group ID of the plugin.
- artifactId: The artifact ID of the plugin.
- version: The version of the plugin.
- configuration: Plugin-specific configuration options.

## Example of Plugin (JACOCO)

```xml
 <build>
        <plugins>
            <!-- JaCoCo plugin for code coverage -->
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
        </plugins>
    </build>


```
4. Build Lifecycle
Maven has a default build lifecycle consisting of several phases. Each phase represents a stage in the build process.

## Phases of the Default Lifecycle:
- validate: Validate the project’s structure.
- compile: Compile the source code.
- test: Run tests.
- package: Package the compiled code into a distributable format (e.g., JAR).
- verify: Perform any necessary verification (e.g., integration tests).
- install: Install the package into the local repository.
- deploy: Deploy the package to a remote repository.
--------------------------------------------------------------
### 📌Lecture Demo (SpeedCalculator)
- [Source Code](https://github.com/ADirin/week4_inclass_speedcalculator)
  
