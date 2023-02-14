---
title: Automated test case structure
subtitle: 
tags: general, automation-test, architecture
domain: tandt53.com
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1676368093008/wIzvfHNPB.jpg?auto=compress
ignorePost: false
---
Photo by <a href="https://unsplash.com/@flowforfrank?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Ferenc Almasi</a> on <a href="https://unsplash.com/photos/EWLHA4T-mso?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
  

# Introduction 
I have been asked many times recently how to organize code within an automated test cases. In my experience, I have created and reviewed thousands of automated test cases, and I see many people struggle with code structure/flow not only in a case but also in a suite. As the result, the code becomes hard to read, review, adjust and maintain. Thus, I made this post to share my points of view to answer "how to effectively organize code in automated test case?"

## Requirement 
I will give an example of a business requirement as follows: `As a normal user, I want to calculate the sum of 2 numbers, and the result should be displayed`.
- Developer will implement a method to meet the functional requirement, and create unit tests as well.
- Tester will verify the "sum" function on the UI of Web or Mobile

# The Unit test rules

For Java developers, they usually use JUnit to create unit tests that verify components in their libraries or frameworks. To define a method as a test case, it will be annotated with `@Test`. This allows JUnit to distinguish test cases from those without test annotations as normal methods.

Within a test method, developers usually follow conventions for arranging instructions. It is `AAA`:
- `Arrange` is used to create, instantiate or set up conditions for the next steps to be executed. It can be called "Precondition", a term more familiar to testers.
- `Actions` are the main steps in a test case. The `sum` method will be invoked with the appropriate parameters.
- `Assert` includes verify commands to compare the expected and the actual result that is returned from the method(s) under test.

E.g: `sum` method and its unit test 
```java
// Method to test
public int sum(int a, int b){
  return a + b;
}

// test
@Test
public void happyCase(){
  // Arrange 
  int a = 4;
  int b = 6;
  int result;
  int expected = 10;

  // Actions 
  result = sum(a, b);

  // Assert
  Assert.assertEquals(result, expected)
}
```

You can see the `Arrange` stage include defining the input values and the expected values as well. In fact, it's not mandatory to define at that stage, you can put it right in assert parameter. However, I would recommend you to define it right after input values so that reviewer can easy understand input-output within the test.

# Manual and automated test cases
Now turn back to the normal tasks of testers, creating test cases on a test management system, like Excel or Testrail, etc. 
- **Manual test case**  

| id | test case name | purpose | precondition | steps | expected result |  
| -- | ------------- | -------- | -------------------- | ------ | ----------------------- |  
| 01 | happy case | verify sum of 2 numbers | the 1st number is 4 </br> the 2nd number is 6 | 1. Input 2 numbers in 2 text boxes </br>2. Click on sum button | Result of sum is 10 should be displayed |  


Above is a typical test scenario that will be executed manually on the Web or Mobile UI. Matching with information from the unit test, you can see data from 3 column in manual test:
- Precondition
- Steps
- Expected result
Then automated test case also use those information in test method.

- **Automated test case**
Now let's create an automated test:

```java
@Test
public void happyCase(){
  // Arrange 
  String url = "https://some.url";
  System.setProperty("webdriver.chrome.driver", "chromedriver.exe");
  WebDriver driver = new ChromeDriver();
  CalculationPage page = new CalculationPage(driver);
  page.open(url)
  int result;
  int a = 4;
  int b = 6;
  int expected = 10;

  // Act
  page.sum(a, b);
  result = page.getResult();

  // Assert 
  Assert.assertEquals(result, expected)

  // Release
  driver.quit();
}
```

Besides `AAA` stages, this automated test has 1 more stage `Release`. The questions are why do we need this stage? And why does the unit test not include?  
The answers are:
- We usually create automation tests at the integration or system level. To ensure that our tests are independent, we should release or roll back the status of the system under test (SUT) as it was before the tests.
- Unit tests are for small components, and do not affect SUT status, so they are not required at this stage.

## More examples
You might think if putting this sample in a suite containing dozens of automated tests, some of the code to set System property, start/stop WebDriver, and close Page will be moved into the `@Before` & `@After` methods. Do we still need a `Release` stage? In which cases will we need this? 
Yes, your thoughts are right. This example is just a hint to think about `status of SUT before and after testing`.

| Before test | During test | After test |
|-----------|-----------|-------------|
| No browser window launched | - Launched applications</br>- Navigate to url</br>- Sum | Browser window is closed

If implement whole automated test suite, the above sample become:
```java
private String url = "https://some.url";
private WebDriver driver;
private CalculationPage page;

@Before
public void setup(){
  System.setProperty("webdriver.chrome.driver", "chromedriver.exe");
  WebDriver driver = new ChromeDriver();
  CalculationPage page = new CalculationPage(driver);
}

@After
public void teardown(){
  driver.quit();
}
@Test
public void happyCase(){
  // Arrange 
  int result;
  int a = 4;
  int b = 6;
  int expected = 10;

  // Act
  page.sum(a, b);
  result = page.getResult();

  // Assert 
  Assert.assertEquals(result, expected)

  // Release
  
}
```

There's no command at "Release" stage in @Test method, because there is no special state to care about. Practically, we see some similar situations, like creating or deleting accounts. Otherwise, in some cases we need to have the `Release` state:
- Ensure the shopping cart is empty after running some steps adding some items to the cart
- Maintain the number of items in a list
- For CRUD functions

## Summary
- Automated test should go through 4 stages, Arrange-Act-Assert-Release. Even so, AAA principle still applies when it is not necessary to keep state before testing.
- State of SUT is a crucial factor in automation test that should be carefully monitored within whole test suite or even in each test method.
- Test cases (automation or manual) present the business requirement, they are not the step-by-step instructions. So the `Act` stage should be carefully created. E.g.
```java 
@Test
public void loginTest(){
  // Act: Don't
  loginPage.enterUsername("username");
  loginPage.enterPassword("password");
  loginPage.clickLogin();

  // Act: Do 
  loginPage.login("username", "password"); // method login() in LoginPage class contains 3 above commands
}
```
- Each test should have only 1 check point that match with the test purpose. We can add 1 or more assert commands at the final `Assert` stage. So don't mix `Act - Assert - Act - Assert`.
- `Assert` is the stage in test method, not the page object method.






