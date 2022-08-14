author: Ganesh Roman
summary: Android J Unit Test Tutorial by Ganesh Roman
id: android-j-unit-testing
tags: test, JUnit, jUnit
categories: Android, Java, Kotlin, Codelabs
environments: Web
status: Published
feedback link: https://ganeshroman.github.io/

# Android J Unit by Ganesh Roman


## Summary
Duration: 0:03:00


<ul>
<li> **Publisher:**  Ganesh V Roman 
<li> **Technology:**  Android, Kotlin 
<li> **Summary:**  Here you will understand JUnit Test Cases & Syntax maintained by Ganesh.
<li> **Webpage:**  [http://ganeshroman.github.io](http://ganeshroman.github.io)
<li> **Created Date:** 15 August 2022
<li> **Last modified:** 15 August 2022 
<li> **Copyright:** © Ganesh V Roman 
</ul>






## The purpose of Software Tests
Duration: 0:04:00

### 2.1. Software Tests?
A software test is a piece of software, which executes another piece of software. It validates if that code results in the expected state (state testing) or executes the expected sequence of events (behavior testing).

### 2.2. How Software Tests are useful?
Software unit tests help the developer to verify that the logic of a piece of the program is correct.

Running tests automatically helps to identify software regressions introduced by changes in the source code. Having a high test coverage of your code allows you to continue developing features without having to perform lots of manual tests.

### 2.3. Testing Frameworks
There are several testing frameworks available for Java. The most popular ones are JUnit and TestNG

This description focuses on JUnit.






## Testing Terminology
Duration: 0:010:00
### 3.1. Code (or application) under test
The code which is tested is typically called the code under test. If you are testing an application, this is called the application under test.

### 3.2. Test Fixture
A test fixture is a fixed state in code which is tested used as input for a test. Another way to describe this is a test precondition.

For example, a test fixture might be a a fixed string, which is used as input for a method. The test would validate if the method behaves correctly with this input.

### 3.3. Unit Tests and Unit Testing
A unit test is a piece of code written by a developer that executes a specific functionality in the code to be tested and asserts a certain behavior or state.

The percentage of code which is tested by unit tests is typically called test coverage.

A unit test targets a small unit of code, e.g., a method or a class. External dependencies should be removed from unit tests, e.g., by replacing the dependency with a test implementation or a (mock) object created by a test framework.

Unit tests are not suitable for testing complex user interface or component interaction. For this, you should develop integration tests.

### 3.4. Integration Tests
An integration test aims to test the behavior of a component or the integration between a set of components. The term functional test is sometimes used as synonym for integration test. Integration tests check that the whole system works as intended, therefore they are reducing the need for intensive manual tests.

These kinds of tests allow you to translate your user stories into a test suite. The test would resemble an expected user interaction with the application.

### 3.5. Performance Tests
Performance tests are used to benchmark software components repeatedly. Their purpose is to ensure that the code under test runs fast enough even if it’s under high load.

### 3.6. Behavior vs. State Testing
A test is a behavior test (also called interaction test) if it checks if certain methods were called with the correct input parameters. A behavior test does not validate the result of a method call.

State testing is about validating the result. Behavior testing is about testing the behavior of the application under test.

If you are testing algorithms or system functionality, in most cases you may want to test state and not interactions. A typical test setup uses mocks or stubs of related classes to abstract the interactions with these other classes away Afterwards you test the state or the behavior depending on your need.







## Using JUnit
Duration: 0:07:00
### 4.1. The JUnit framework
JUnit in version 4.x is a test framework which uses annotations to identify methods that specify a test. The main websites for JUnit are the JUnit homepage (http://junit.org/) and the Github project page (https://github.com/junit-team/junit).

### 4.2. How to define a test in JUnit?
A JUnit test is a method contained in a class which is only used for testing. This is called a Test class. To write a JUnit 4 test with the you annotate a method with the @org.junit.Test annotation.

This method executes the code under test. You use an assert method, provided by JUnit or another assert framework, to check an expected result versus the actual result. These method calls are typically called asserts or assert statements.

You should provide meaningful messages in assert statements. That makes it easier for the user to identify and fix the problem. This is especially true if someone looks at the problem, who did not write the code under test or the test code.

### 4.3. Example JUnit test
The following code shows a JUnit test. This test assumes that the MyClass class exists and has a multiply(int, init) method.

```java
import static org.junit.Assert.assertEquals;

import org.junit.Test;

public class MyTests {

        @Test
        public void multiplicationOfZeroIntegersShouldReturnZero() {
                MyClass tester = new MyClass(); // MyClass is tested

                // assert statements
                assertEquals("10 x 0 must be 0", 0, tester.multiply(10, 0));
                assertEquals("0 x 10 must be 0", 0, tester.multiply(0, 10));
                assertEquals("0 x 0 must be 0", 0, tester.multiply(0, 0));
        }
}
```







## Basic JUnit code construction
Duration: 0:10:00
### 5.1. JUnit annotations
JUnit 4.x uses annotations to mark methods as test methods and to configure them. The following table gives an overview of the most important annotations in JUnit.

#### Table 1. Annotations

<table>
<tr> <th> Annotation </th><th> Description </th></tr>
<tr> <th> @Test <br>public void method() </th><th> The @Test annotation identifies a method as a test method. </th></tr>
<tr> <th> @Test <br>(expected = Exception.class) </th><th> Fails if the method does not throw the named exception. </th></tr>
<tr> <th> @Test(timeout=100) </th><th> Fails if the method takes longer than 100 milliseconds. </th></tr>
<tr> <th> @Before <br>public void method() </th><th> This method is executed before each test. It is used to prepare the test environment (e.g., read input data, initialize the class). </th></tr>
<tr> <th> @After <br>public void method() </th><th> This method is executed after each test. It is used to cleanup the test environment (e.g., delete temporary data, restore defaults). It can also save memory by cleaning up expensive memory structures. </th></tr>
<tr> <th> @BeforeClass <br>public static void method() </th><th> This method is executed once, before the start of all tests. It is used to perform time intensive activities, for example, to connect to a database. Methods marked with this annotation need to be defined as static to work with JUnit.  </th></tr>
<tr> <th> @AfterClass <br>public static void method() </th><th> This method is executed once, after all tests have been finished. It is used to perform clean-up activities, for example, to disconnect from a database. Methods annotated with this annotation need to be defined as static to work with JUnit.  </th></tr>
<tr> <th> @Ignore or @Ignore<br>("Why disabled") </th><th> Ignores the test method. This is useful when the underlying code has been changed and the test case has not yet been adapted. Or if the execution time of this test is too long to be included. It is best practice to provide the optional description, why the test is disabled. </th></tr>


</table>


### 5.2. Assert Statements
JUnit provides static methods to test for certain conditions via the Assert`class. These assert statements typically start with `assert . They allow you to specify the error message, the expected and the actual result. An assertion method compares the actual value returned by a test to the expected value. It throws an AssertionException if the comparison fails.

The following table gives an overview of these methods. Parameters in [] brackets are optional and of type String.

#### Table 2. Methods to assert test results

<table>
<tr> <th> Statement </th><th> Description </th></tr>

<tr> <td> fail(message)   </td><td> Let the method fail. Might be used to check that a certain part of the code is not reached or to have a failing test before the test code is implemented. The message parameter is optional.   </td></tr>
<tr> <td>  assertTrue([message,] boolean condition)   </td><td> Checks that the boolean condition is true.   </td></tr>
<tr> <td>  assertFalse([message,] boolean condition)  </td><td>  Checks that the boolean condition is false.  </td></tr>
<tr> <td>  assertEquals([message,] expected, actual)  </td><td> Tests that two values are the same. Note: for arrays the reference is checked not the content of the arrays.   </td></tr>
<tr> <td>  assertEquals([message,] expected, actual, tolerance)  </td><td>  Test that float or double values match. The tolerance is the number of decimals which must be the same.  </td></tr>
<tr> <td>  assertNull([message,] object)  </td><td> Checks that the object is null.   </td></tr>
<tr> <td>  assertNotNull([message,] object)  </td><td> Checks that the object is not null.   </td></tr>
<tr> <td>  assertSame([message,] expected, actual)  </td><td>  Checks that both variables refer to the same object.  </td></tr>
<tr> <td>  assertNotSame([message,] expected, actual)  </td><td> Checks that both variables refer to different objects.   </td></tr>

</table>

### 5.3 JUnit Methods:
You encounter 3 new methods provided by JUnit Framework:

#### Table 3. More Methods provided by JUnit Framework


<table>
<tr> <th> Statement </th><th> Description </th></tr>
<tr> <th> assertThat():  </th><th> As is evident by the name, this method lets you create custom assertions and not just true and false values. It takes in 3 arguments. A reason/description, input value to be checked, expected actual value.  </th></tr>
<tr> <th> Is(): </th><th> Is method returns a Matcher to match the source object to the one provided as the parameter of is(); </th></tr>
<tr> <th> equalTo(): </th><th> As the name suggests, this method checks for equality between the expected and actual value. </th></tr>
<tr> <th> When(): </th><th> This is a very powerful method which takes in a method call as its parameter. It takes in the method call which is to be stubbed/duplicated. Once the method stub is executed, “then()” is called. </th></tr>
<tr> <th> thenReturn(): </th><th> It is called after the method stub provided in when() method has finished running. It is used to return the result of the method, if it is not void. </th></tr>

</table>







## JUnit Classes
Duration: 0:03:00

- You can find the package for **Unit Tests** case written in the package test-> java -> com.example.testapplication (LogicUnitTest.java)
- You can find the package for **Mockito Tests** in test -> java -> com.example.testapplication.MockitoTest.java
- You can find the package for **Instrumentation Testing** (Mockito) androidTest -> java -> com.example.testapplication




