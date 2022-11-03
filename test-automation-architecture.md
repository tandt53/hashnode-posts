---
title: Test Automation Architecture
subtitle: Overview the architecture for automation test
tags: general, automation-test, architecture
domain: tandt53.com
ignorePost: false
---

As an automation test engineer, you might have confused what a good scenario is, how well our automation test are organized? or just asking yourself where should the line/method/module should be placed? This post will disclose a clear view for you about architecture of automation test.

## Layered architecture 
Layered architecture is well known pattern that is applied for many enterprise software solutions. The components in this architecture are layered horizontally, each one has own responsibility for application's features. Normally it has 3 or 4 layers:
- Presentation layer
- Business layer
- Persistence layer
- Database layer

![Layered architecture pattern](https://cdn.hashnode.com/res/hashnode/image/upload/v1667495549657/_r48nZGtM.png?auto=compress)

Image is cited from the book "Software architecture patterns" by Mark Richards. 

This post does not go deeply about this pattern, because there's tone of document on the internet you can find and read. Test automation is like a software solution/product, so it can definitely follow this kind of pattern to inherit all its advantages.

## Test automation layers 
I will list down some basic layers in test automation, with code samples, and how to define which layer is line/method of code belong to. 

Here is basic example for web automation using Selenium Webdriver:
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
- `Test layer` simply are the set of all test classes, methods that arrange scenarios and checkpoint verifications into test cases and suites. During test execution, this layer will call methods from business layer (`loginPage.login()`) and from core layer (`Assert.assertTrue()`). A test method is usually represented for `test scenario(s)` that includes use cases and expected result verification.
- `Business layer` covers all the business feature of the application that and converts into programmable methods with parameters (if needed). These methods describe user steps in user scenario, not test scenario, so assert commands should never appeared in this level. Normally `page objects` is considered as in business layer.
- `Core layer` contains all the supported methods from libraries or frameworks to execute test on application. Theses methods can come from:
  - Test libraries/frameworks: `Assert.assertTrue()`, `Assert.assertEquals()`
  - web automation: sendKeys(), click(), etc. from Selenium WebDriver
  - API automation: given(), when(), then() from Rest Assured.
  - etc.

## Layers Principles
Beside the layer's responsibilities, there is a rule to place a layer on the top, middle or bottom of the architecture. The key is `dependency`.
To make the right position, you should answer some basic questions:
- `Responsibility - what is it for?` for test or business or support.
- `Dependencies - what are its dependencies?` test layer has 2 dependencies: business and core layers. But business layer depends on only core.
- `Dependents - what are its dependents?` test layer does not have any dependent, business has 1 dependent (test layer), core layer has 2 dependents (business and test layers)

 
## Summary
This post just presents 3 basic layers in the architecture for test automation. It is a good and fundamental guideline for you to structure test automation code. In fact, it is not easy to categorize everything in an automation project that is belong to one of 3 mentioned layers. However, just remember keyword `DEPENDENCY` and you can freely create new layer(s) as you want.