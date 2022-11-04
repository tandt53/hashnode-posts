---
title: Test Automation Architecture
subtitle: Overview the architecture for automation test
tags: general, automation-test, architecture
domain: tandt53.com
ignorePost: false
---

As an automation test engineer, you might have wondered what a good scenario is and how well our automation tests are organized. Or just asking yourself where should the line/method/module be placed? This post will provide you with a clear understanding of the architecture of automation test.

## Layered architecture 
Layered architecture is a well-known pattern used in many enterprise software solutions. Components in this architecture are horizontally layered, with each component responsible for its own feature. Normally it has 3 or 4 layers:
- Presentation layer
- Business layer
- Persistence layer
- Database layer

![Layered architecture pattern](https://cdn.hashnode.com/res/hashnode/image/upload/v1667495549657/_r48nZGtM.png?auto=compress)

This image was taken from Mark Richard's book "Software architecture patterns.".

This post does not go into detail about this pattern, because there are many documents on the internet you can find and read. Test automation is like a software solution/product, so it can definitely follow this kind of pattern to inherit all its advantages.

## Test automation layers 
I will list down some basic layers in test automation, with code samples, and how to define which layer the line/method of code belongs to

Here is a basic example of web automation using Selenium Webdriver:
```java
public class LoginTest{
    @Test
    public void loginSuccessfully(){
        loginPage.login(username, password);
        Assert.assertTrue(homePage.isDisplayed());
    }
}
public class LoginPage(){
    public void login(String username, String password){
        this.txtUsername.sendKeys(username);
        this.txtPassword.sendKeys(password);
        this.btnLogin.click();
    }
}
```
- **Test layer** simply is the set of all test classes, methods that arrange scenarios and checkpoint verifications into test cases and suites. During test execution, this layer will call methods from the business layer (`loginPage.login()`) and from the core layer (`Assert.assertTrue()`). In general, a test method represents a set of scenario steps that include use cases and expected results.
- **Business layer** encapsulates all the business features of an application and converts them into programmable methods with parameters (if needed). These methods describe user steps in the user scenario, not the test scenario, so assert commands should never appear at this level. Normally **page objects** are considered part of the business layer.
- **Core layer** contains all the methods available in libraries or frameworks to execute tests on an application. These methods can come from:
  - Test libraries/frameworks: `Assert.assertTrue()`, `Assert.assertEquals()`
  - Web automation: `sendKeys()`, `click()`, etc. from Selenium WebDriver
  - API automation: `given()`, `when()`, `then()` from Rest Assured.
  - Etc.

## Layers Principles
As well as layer responsibilities, there is a rule for placing layers at the top, middle, or bottom of an architecture. The key is **dependency**. To choose the right position, you should answer some basic questions:
- `Responsibility` - *what is it for?* For testing, business, automating or support.
- `Dependencies` - *what are its dependencies?* Test layer has 2 dependencies: the business and core layers. But the business layer depends on the core.
- `Dependents` - *what are its dependents?* Test layer does not have any dependents, business layer has 1 dependent (test layer), core layer has 2 dependents (business and test layers).

 
## Summary
This post summarizes the 3 basic layers of the architecture for test automation. It can serve as a helpful and fundamental outline for structuring test automation code. It is not easy to categorize everything that falls under one of the 3 layers in an automation project. However, just remember the keyword **DEPENDENCY** and you can freely create new layer(s) as you want.

