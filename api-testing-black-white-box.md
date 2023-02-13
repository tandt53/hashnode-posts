---
title: Is API testing black box or white box? 
subtitle: It is not a good practice when placing assertions in Page Objects.
tags: API, api-testing, white-box, black-box
domain: tandt53.com
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1676258301265/aboTmcfOE.jpg?auto=compress
ignorePost: false
---
This is the question you might be asked during an interview for QA position. And you may have an answer and a reason for that answer. In this post, I will provide my response to this question and explain my reasoning. 

Please note that the term API mentioned in this post refers to web services like REST, SOAP.

## Black and white box testing definition
I will discuss the definition of 2 testing types. According to the Guru99 post, Difference Between Black Box and White Box Testing mentioned:
> **Black Box testing**: It is a testing approach which is used to test the software without the knowledge of the internal structure of program or application.
> 
> **White Box testing**: It is a testing approach in which the internal structure is known to the tester.

I would want to rephrase the above definitions a bit: if you can see the internal structure or implementation of the test target, that means it's a white box, otherwise a black box.

## Debate
If you ask this question to your colleagues, you might get some different answers like these:
- Black box: When testing APIs, testers typically use Postman or a framework to send requests, then check the response. That's obviously a black box because testers do not care about how APIs are created.
- White box: As a counterpoint, sometimes testers can see the source code of APIs as well as understand the flow of information through that code. So it should be a white box.
- Grey box/gray box: Another example would be that testers are verifying functions on UI (Web or Mobile) and they can know exactly what APIs are called under that UI. So, it can be a grey box.

## My answer
I'd like to discuss some factors to support my answer:
- Test target (or test object): API testing means targets are APIs, not UI. Testers can use API to support UI testing, but API is not the target of testing activities, it is UI. Therefore, the gray box with the given explanation is not correct.

- Black box and white box are two sets of techniques used during tests.
  - Black box techniques include Equivalence partition, Boundary Value, Decision Table...
  - White box techniques include Statement Coverage, Branch Coverage,...

With the above discussion, the question seems to try to match a testing type to a testing technique, which is not appropriate. When asked this concern, we (QA/testers) tend to mention that API testing involves sending APIs and checking the response by using Postman or any similar tool/framework. This is a black box, because we'll use some black box techniques to test it. In contrast, it could be white box, if we apply coverage techniques to verify APIs.

## Summary
In this post, I have presented my answers to the question in the title. The choice of black box or white box depends on which techniques will be used to ensure the API quality.

Cover Photo by <a href="https://unsplash.com/@yogidan2012?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Daniele Levis Pelusi</a> on <a href="https://unsplash.com/photos/BXn8J4yjh3M?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>