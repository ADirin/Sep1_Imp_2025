#Week 4 in-class assignment
This week’s assignment is a continuation of last week’s. If you have not completed it, you can find a sample code here as well…

### Your Tasks in this assignment.
You need to extend last week’s assignment by adding JaCoCo code coverage to the temperature JUnit test report. To do this, follow these steps:

- Copy the following sample JaCoCo plugin and add it to your pom.xml, as demonstrated during the lecture.

 ```
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
````
- Run Maven → clean and then Maven → install

- Make sure the target folder is created Open the index.html file and make sure the code coverage is done

- Upload it to the public_html folder and share the link in the Moodle folder (Follow the exact step that is shared during the lecture)

- ---------------------------------------------------------------------------

## Example: Temperature Conversion

###  Implement the Functionality
Now that the test is failing, implement the simplest solution to pass the test.

1. **Create a New Project**:
   - Open IntelliJ IDEA.
   - Go to `File > New > Project`.
   - Choose `Java` as the project type.
   - Name the project `OTP1_inclass_assignment` and set the location.
   - Click `Finish`.

2. Create the TemperatureConverter Class:
- Right-click on the src directory and select New > Java Class.
- Name the class TemperatureConverter.
- Implement the fahrenheitToCelsius method:

```java
public class TemperatureConverter {
    public double fahrenheitToCelsius(double fahrenheit) {
        return (fahrenheit - 32) * 5 / 9;
    }
}
````
3. Implement the celsisusToFarenheit Method:
- Add the celsiusToFahrenheit method to the TemperatureConverter class:
```java
public double celsiusToFahrenheit(double celsius) {
    return (celsius * 9 / 5) + 32;
}

````
3. Implement the Method:
- Add the following method to the TemperatureConverter class:
```java
public boolean isExtremeTemperature(double celsius) {
    return celsius < -40 || celsius > 50;
}

````

### Step 2: create the Test
- Right click to the java class and select `Goto`-->`Test`-->`create new test`
- Select the functions which you want to test
### Step 2: Write the First Test
Start by writing a simple test to convert Fahrenheit to Celsius.

   ```java
   import org.junit.Test;
   import static org.junit.Assert.*;

   public class TemperatureConverterTest {

       @Test
       public void testFahrenheitToCelsius() {
           TemperatureConverter converter = new TemperatureConverter();
           assertEquals(0, converter.fahrenheitToCelsius(32), 0.01);
       }
   }

````
### Explanation:
- The test asserts that 32°F is equivalent to 0°C.

2. **Run the Test:**
- Right-click on the TemperatureConverterTest class and select Run 'TemperatureConverterTest'.

### Step 4: Add More Tests

1. Test for Celsius to Fahrenheit:
- Add the following test to the TemperatureConverterTest class:
```java
@Test
public void testCelsiusToFahrenheit() {
    TemperatureConverter converter = new TemperatureConverter();
    assertEquals(32, converter.celsiusToFahrenheit(0), 0.01);
}

```
Explanation:

- This test checks if 0°C is converted to 32°F.


### Step 5: Add a Test for Edge Cases
1. Test for Extreme Temperatures:
- Add a test method to check for extreme temperatures:
```Java
@Test
public void testIsExtremeTemperature() {
    TemperatureConverter converter = new TemperatureConverter();
    assertTrue(converter.isExtremeTemperature(-50));
    assertFalse(converter.isExtremeTemperature(20));
    assertTrue(converter.isExtremeTemperature(60));
}

```
2. Run the Test:

```

- All tests should pass, demonstrating the successful implementation of the functionality.




```java
@Test
public void testFahrenheitToCelsius() {
    TemperatureConverter converter = new TemperatureConverter();
    
    // Test normal case
    assertEquals(0.0, converter.fahrenheitToCelsius(32.0), 0.01);
    
    // Test negative Fahrenheit
    assertEquals(-40.0, converter.fahrenheitToCelsius(-40.0), 0.01);
    
    // Test very high Fahrenheit
    assertEquals(100.0, converter.fahrenheitToCelsius(212.0), 0.01);
}
```

Testing celsiusToFahrenheit Method
```java
@Test
public void testCelsiusToFahrenheit() {
    TemperatureConverter converter = new TemperatureConverter();
    
    // Test normal case
    assertEquals(32.0, converter.celsiusToFahrenheit(0.0), 0.01);
    
    // Test negative Celsius
    assertEquals(-40.0, converter.celsiusToFahrenheit(-40.0), 0.01);
    
    // Test very high Celsius
    assertEquals(212.0, converter.celsiusToFahrenheit(100.0), 0.01);
}


```
Testing isExtremeTemperature Method
````java

@Test
public void testIsExtremeTemperature() {
    TemperatureConverter converter = new TemperatureConverter();
    
    // Test extreme temperatures
    assertTrue(converter.isExtremeTemperature(-41.0));
    assertTrue(converter.isExtremeTemperature(51.0));
    
    // Test non-extreme temperatures
    assertFalse(converter.isExtremeTemperature(0.0));
    assertFalse(converter.isExtremeTemperature(30.0));
}


`````

### Test Overall Conversion Accuracy
```java
@Test
public void testOverallConversionAccuracy() {
    TemperatureConverter converter = new TemperatureConverter();
    
    // Test a series of conversions to ensure accuracy
    double fahrenheit = 100.0;
    double celsius = converter.fahrenheitToCelsius(fahrenheit);
    assertEquals(fahrenheit, converter.celsiusToFahrenheit(celsius), 0.01);
}


```
Test Extreme Temperature Handling
```java
@Test
public void testExtremeTemperatureHandling() {
    TemperatureConverter converter = new TemperatureConverter();
    
    // Ensure extreme temperature checking is correct
    assertTrue(converter.isExtremeTemperature(-50.0));
    assertTrue(converter.isExtremeTemperature(60.0));
    assertFalse(converter.isExtremeTemperature(-30.0));
    assertFalse(converter.isExtremeTemperature(40.0));
}
```


