---
title: Stop init Driver instance before test
subtitle: 
tags: general, automation-test, code-structure
domain: tandt53.com
ignorePost: false
---

A lot of documents and posts have been published on the internet for web automation using Selenium Webdriver that are showing `WebDriver driver = new ChromeDriver()` in beforeTest annotated methods. This concept will remain with you months or years later, not just me. As a result of studying [test automation architecture](https://tandt53.com/test-automation-architecture), I have changed my mind. In this post, I will help you correct a wrong concept.

```java
public class LoginTest {
    private WebDriver driver;
    private LoginPage loginPage;
    private HomePage homePage;

    @BeforeTest
    public void setup(){
        driver = new ChromeDriver();
        loginPage = new LoginPage(driver);
        homePage = new HomePage(driver);
    }
}

class LoginPage {
    public LoginPage(WebDriver driver) {
        PageFactory.initElements(driver, this);
    }
}

class HomePage {
    public HomePage(WebDriver driver) {
        PageFactory.initElements(driver, this);
    }
}
```

# Remind 
Remind you to ask yourself 3 questions from [last post](https://tandt53.com/test-automation-architecture#heading-layers-principles) to define which layer should driver initialization be placed.
- `Responsibility` - *what is it for?* For testing, business, automating or support.
- `Dependencies` - *what are its dependencies?* Test layer has 2 dependencies: the business and core layers. But the business layer depends on the core.
- `Dependents` - *what are its dependents?* Test layer does not have any dependents, business layer has 1 dependent (test layer), core layer has 2 dependents (business and test layers).

# Answers
- `Responsibility` - what Webdriver instance is for? to find and interact with elements shortly. It's nothing with test.
- `Dependencies` - what are its dependencies? driver's dependencies are capabilities, not the test itself.
- `Dependents` - what are its dependents? Page need driver instance to factory elements, tests do no use it directly. 

# Review 
As the above answers demonstrate, the driver instance is useless at test level. In the before-test method, you create and pass it to the page object's constructor. Why do we have to put it in the test? It also causes high coupling between test objects and page objects, so it's not a wise idea.

# Solution

Now it's clear to not start the driver at test level, right? We should do that at the page object level. However it's often needed whenever a constructor is invoked, so we will create a driver instance inside constructors.

```java
public class LoginTest(){
    private LoginPage loginPage;
    private HomePage homePage;
    public void beforeTest(){
        loginPage = new LoginPage();
        homePage = new HomePage();
    }
}
class LoginPage {
    public LoginPage() {
        WebDriver driver = new ChromeDriver();
        PageFactory.initElements(driver, this);
    }
}

class HomePage {
    public HomePage() {
        WebDriver driver = new ChromeDriver();
        PageFactory.initElements(driver, this);
    }
}
```

Now that you see the `driver` has disappeared from the `LoginTest` class, that means the dependency has been removed. However, the page objects' constructors duplicate driver initialization. So it should separate it into another class, like a driver manager.

```java
class DriverManager {
    private static WebDriver driver;

    public static WebDriver getDriver() {
        if (driver == null) {
            driver = new ChromeDriver();
        }
        return driver;
    }
}
class LoginPage {
    public LoginPage() {
        WebDriver driver = DriverManager.getDriver();
        PageFactory.initElements(driver, this);
    }
}

class HomePage {
    public HomePage() {
        WebDriver driver = DriverManager.getDriver();
        PageFactory.initElements(driver, this);
    }
}
```

`DriverManager.getManager()` handles code that prevents driver initialization multiple times. 

## Summary

In this post, we discussed why we should not create driver instances at the test level. No matter what browser is running, it helps keep tests clean and limited to business features. Furthermore, the coupling between test and WebDriver is lost.
