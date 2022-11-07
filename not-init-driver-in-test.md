---
title: Stop init Driver instance before test
subtitle: 
tags: general, automation-test, code-structure
domain: tandt53.com
ignorePost: true
---

There are tone of documents or posts on internet for new web automation using Selenium Webdriver that are showing like this `WebDriver driver = new ChromeDriver()` in beforeTest annotated methods. You will keep this concept along with you for months or years later, not excepted me. However, after seriously study about test automation architecture, I have changed my mind. Then this post, I will pull you out of wrong concept.

# Remind 
Remind to ask yourself 3 questions to define which layer should be used.
- `Responsibility` - *what is it for?* For testing, business, automating or support.
- `Dependencies` - *what are its dependencies?* Test layer has 2 dependencies: the business and core layers. But the business layer depends on the core.
- `Dependents` - *what are its dependents?* Test layer does not have any dependents, business layer has 1 dependent (test layer), core layer has 2 dependents (business and test layers).

# Answers
- `Responsibility` - what Webdriver instance is for? to find and interact with elements shortly. It's nothing with test.
- `Dependencies` - what are its dependencies? driver's dependencies are capabilities, not the test itself.
- `Dependents` - what are its dependents? Page need driver instance to factory elements, tests do no use it directly. 

# Review 
As the above answers, you can see that driver instance is useless in test level. Before test method just create and pass to page object's constructors. So why we have to create it in test level? or it causes highly coupling between tests and page objects, so it's not good.

# Solution

Now it's clear to not init driver in test level, right? We should do that in page object level. But it's often needed whenever constructor is invoked to factory elements.

```java
public class LoginPage(){

    public LoginPage(){
        
    }
}

```