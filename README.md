# Project Structure 

cucumber-jvm-testng-integration-master

1. pom.xml 
2. testng.xml 

3. src ..... main ..... java ..... com.cucumber.testng.examples ..... DateCalculator.java 

4. src ..... test ..... java ..... com.cucumber.testng.examples ..... DateStepdefs.java 
5. src ..... test ..... java ..... com.cucumber.testng.examples ..... GenerateReport.java
6. src ..... test ..... java ..... com.cucumber.testng.examples ..... RunCukesByCompositionGrp1_Test1.java
7. src ..... test ..... java ..... com.cucumber.testng.examples ..... RunCukesByCompositionGrp1_Test2.java
8. src ..... test ..... java ..... com.cucumber.testng.examples ..... RunCukesByFeatureAndCompositionTest1.java
9. src ..... test ..... java ..... com.cucumber.testng.examples ..... RunCukesByFeatureAndCompositionTest2.java 
10. src ..... test ..... java ..... com.cucumber.testng.examples ..... RunCukesTest.java    
11. src ..... test ..... java ..... com.cucumber.testng.examples ..... TestNGExecutionListener.java

12. src ..... test ..... resources ..... com.cucumber.testng.examples ..... date_calculator1.java
13. src ..... test ..... resources ..... com.cucumber.testng.examples ..... date_calculator2.java


                                    
                                    
                                    
                                    
                                    



# Integration of Cucumber-JVM and TestNG

Cucumber-JVM has native integration with Junit and it uses Junit as a test runner. But if you want to integrate TestNG with Cucumber-JVM
then there is no proper documentation available to do this. So I decided to create a sample project where I can demonstrate this integration along with some better reporting.

## Basic Integration

To have a simple integration you just need to create a runner class and just extend it from AbstractTestNGCucumberTests.

###Here is the basic code:

```java
package com.cucumber.testng.examples;

import cucumber.api.Scenario;
import cucumber.api.java.After;
import cucumber.api.java.Before;

public class BaseStepDefs {
    @Before()
    public void before(Scenario scenario) {
        scenario.getId();
        System.out.println("This is before Scenario: " + scenario.getName().toString());
    }

    @After
    public void after(Scenario scenario) {
        System.out.println("This is after Scenario: " + scenario.getName().toString());
    }
}
```

If you will run this java file as a testNG class file then it will simply pick the features mentioned in the @CucumberOptions.
We can point to a folder here with multiple feature files or we can point to a specific feature file.
For Reporting, we are using the native cucumber-jvm reports which will generate a cucumber1.json file and will also generate a
pretty html file.

## Advanced Integration

To have full-fledged integration with TestNg where you can use testng xml along with parallel execution. Please refer these files:
/testng.xml
/src/test/java/com/cucumber/testng/examples/RunCukesByCompositionGrp1_Test1.java
/src/test/java/com/cucumber/testng/examples/RunCukesByCompositionGrp1_Test2.java

###Here is the sample code:
```java
@CucumberOptions(features = "src/test/resources/com.cucumber.testng.examples/date_calculator1.feature", 
plugin = "json:target/cucumber2.json")
public class RunCukesByCompositionGrp1_Test2 {
    @Test(groups = "examples-testng", description = "Example of using TestNGCucumberRunner to invoke Cucumber")
    public void runCukes() {
        new TestNGCucumberRunner(getClass()).runCukes();
    }
}
```
Here we are creating a test method that will be invoked by TestNG and invoke the Cucumber runner within that method. 
So this test method can point to a group of features and will generate a dedicated results json file. 
Similarly we can have another similar file "RunCukesByCompositionGrp1_Test2.java" which will point to a different batch of features.
This way we can run this both groups of features parallelly using testng.xml.

### To see this whole thing running simply checkout this project and run this command:
mvn clean test
