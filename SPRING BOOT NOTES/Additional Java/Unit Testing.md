Using JUnit
TDD -> Test Driven Development

 New JUnit Test Case

Maven Dependency : 
```xml
<dependency>  
    <groupId>junit</groupId>  
    <artifactId>junit</artifactId>  
    <version>4.13.2</version>  
    <scope>test</scope>  
</dependency>
```

Code for which tests will be done : 
```java
public class Calculator {
	public int add(int a, int b){
		return a + b;
	}
}
```

Test class
in src/test/java/.../TestCalculator
```java
package com.quotes.demo;  
import org.junit.Before;  
import org.junit.Test;  

import static org.junit.Assert.assertEquals;

public class TestCalculator {  

    Calculator calculator;  

    @Before
    public void setUp(){
        calculator = new Calculator();
    }

    @Test  
    public void testAdd(){
        assertEquals(5, calculator.add(2, 3));
    }
}
```

@Test : Marks the method as a test case
@Before  : calls this method before tests 
@After : calls this method after the tests
@BeforeAll :
@BeforeEach : 
@AfterAll :
@AfterEach : 
@Disabled  -> If we have multiple tests in the class and one test (method) marked with @Disabled annotation. If we run the whole class the marked test won't execute
assertEquals(expected result, actual result) 


assertThrows(ExceptionClass, expression that throws the exception)
- ExceptionClass -> Name of the class of which Exception you expect to be thrown
Ex : 
```java
assertThrows(IllegalArgumentException.class, () - > {
	calculator.determineGrade(-1);
})
```

some annotations and methods

### Assert class:

| assertEquals(expected, actual)      | Checks that two objects are equal                       |
| ----------------------------------- | ------------------------------------------------------- |
| assertNotEquals(expected, actual)   | Checks that two objects are not equal                   |
| assertArrayEquals(expected, actual) | Checks that two arrays are equal                        |
| assertTrue(condition)               | Checks that a condition is true                         |
| assertFalse(condition)              | Checks that a condition is false                        |
| assertNull(object)                  | Checks that an object is null                           |
| assertNotNull(object)               | Checks that an object is not null                       |
| assertSame(expected, actual)        | Checks that two objects refer to the same object        |
| assertNotSame(expected, actual)     | Checks that two objects do not refer to the same object |


### JUnit Test cases class

| countTestCases()       | Counts the number of test cases executed by the run(TestResult tr) method                                              |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| createResult()         | Creates a TestResult object                                                                                            |
| getName()              | Returns the name of the TestCase as a string                                                                           |
| run()                  | Executes a test and returns a TestResult object.                                                                       |
| run(TestResult result) | Executes a test using a TestResult object but doesn't return anything.                                                 |
| setName(String name)   | Sets the name of the TestCase.                                                                                         |
| setUp()                | Used to write resource association code, such as creating a database connection.                                       |
| tearDown()             | Used to write resource release code, such as releasing a database connection after performing a transaction operation. |

### JUnit TestResult Class 

| runCount()                            | This method returns the total number of tests that were run.                                             |
| ------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| failureCount()                        | This method returns the number of failures that occurred while running the tests.                        |
| errorCount()                          | This method returns the number of errors that occurred while running the tests.                          |
| wasSuccessful()                       | This method returns true if all the tests completed successfully (i.e., no errors or failures occurred). |
| addListener(TestListener listener)    | This method registers a TestListener                                                                     |
| stop()                                | This method marks that the test run should stop.                                                         |
| removeListener(TestListener listener) | This method unregisters a TestListener                                                                   |

### JUnit Test Suite Class

| countTestCases()                 | This method returns the number of test cases in the current test suite.                                               |
| -------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| getName()                        | This method returns the name of the current test suite.                                                               |
| run(TestResult result)           | This method runs all the test cases in the current test suite and reports the results to the given TestResult object. |
| testAt(int index)                | This method returns the test case at the given index in the current test suite.                                       |
| testCount()                      | This method returns the number of test cases in the current test suite.                                               |
| tests()                          | This method returns an enumeration of all the test cases in the current test suite.                                   |
| toString()                       | This method returns a string representation of the current test suite.                                                |
| addTest(Test test)               | This method adds a new test case or test suite to the current test suite.                                             |
| addTestSuite(Class<?> testClass) | This method adds all the test cases in a given test class to the current test suite.                                  |
