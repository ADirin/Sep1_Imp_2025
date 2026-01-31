# Unit Testing and Code Coverage

## Objective:
The objective of this assignment is to familiarize yourself with  JUnit 5 testing environment and to determine test coverage.

## Steps:
1. **Create a Maven Project:**
   

2. **Import Code:**
   - Clone the code from [this Repo](https://github.com/ADirin/Wee4_InClass_Calculator.git)

### PART 1:  JUnit 5

1. **Fix Errors and Enhance Testing:**
   - Correct the intentional errors in the existing tests.
   - Complete missing including  `product()` and `squareroot()`.
   - Ensure comprehensive test coverage and add additional test methods where necessary.

2. **Parameterized Test:**
   - Refactor the `ExtraTest` class to use parameterized arrays for testing.

3. **Handle Floating-Point Numbers:**
   - Modify the calculator to handle double-precision floating-point numbers.
   - Provide a delta value when comparing floating-point numbers in assert routines.

4. **Regression Testing:**
   - After making changes, conduct regression testing by running the test suite to ensure all tests pass.

5. **Run Tests with Maven:**
   - Run tests with Maven:
     - Right-click on the project name and select `Run As` | `Maven test`.
   - View test results in the `target/surefire-reports` directory.


 6. **Run Application and Tests:**
   - first Verify  that there are no errors in the implementation (code)/.
   - Run the JUnit tests:
   - Some tests will pass while others will not.


### PART 2 OF THE TASK: Coverage

> Important to note that you need to add proper reporting plugins, e.g., JACOCO in `POM.XML`, here is an example of such `POM.XML` file, make sure that you have the
right jdk and pluging versions.
 ```xml

    <dependencies>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>5.10.2</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <release>21</release>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.jacoco</groupId>
                <artifactId>jacoco-maven-plugin</artifactId>
                <version>0.8.9</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>prepare-agent</goal>
                        </goals>
                    </execution>
                    <execution>
                        <id>report</id>
                        <goals>
                            <goal>report</goal>
                        </goals>
                        <phase>verify</phase>
                    </execution>
                </executions>
                <configuration>
                    <excludes>
                        <exclude>sun/util/resources/**</exclude>
                        <exclude>sun/text/resources/**</exclude>
                        <exclude>sun/util/cldr/**</exclude>
                        <exclude>sun/util/resources/provider/LocaleDataProvider</exclude>
                        <exclude>sun/text/resources/cldr/ext/FormatData_fi</exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>


```
4. **Determine Test Coverage:**
   - Run the coverage analysis using Maven.
   - Generate the JaCoCo report.
   - Upload the generated HTML report to your public HTML directory.


## Resources:
- [Code coverage](http://en.wikipedia.org/wiki/Code_coverage)
- [Java Code Coverage Tools](http://en.wikipedia.org/wiki/Java_Code_Coverage_Tools)

## Submission:
- Add the HTML file (the whole site folder) into your metropolia HTML folder.
- Attach a screenshot of the test results along with you public HTML link to Moodle week4 home assignment folder.

