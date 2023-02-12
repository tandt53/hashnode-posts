---
title: Is API testing black box or white box? 
subtitle: It is not a good practice when placing assertions in Page Objects.
tags: API, api-testing, white-box, black-box
domain: tandt53.com
cover: 
ignorePost: false
---

This is the question you might be asked during the testing interview section. And you may have a answer and the supported reason for that answer. My answer is black box and white box. I will present the reasons for my answer in this post. 
Please note that the term API mentioned in this post are the web services like REST, SOAP.

## Black and White box testing definition

I will discuss the definition of 2 testing types.  
According to the Guru99 post [Difference Between Black Box and White Box Testing](https://www.guru99.com/back-box-vs-white-box-testing.html) mentioned:
- Black Box testing: It is a testing approach which is used to test the software `without the knowledge` of the internal structure of program or application. 
- White Box testing: It is a testing approach in which internal structure is `known` to the tester. 

I would want to rephrase the above definitions: if you can see the internal structure/implementation of test target, that means it's white box, otherwise black box.

## Answers
- Back box: When testing API, testers usually use postman or any tool or frameworks to send request, then check the response. That's obviously black box because you do not have any idea about how APIs created.

- White box: Counter with above answer, testers can see source code of APIs, also understand the flow of data go through that source code. So it should be white box.

- Grey box / gray box
Another could give an example that testers are verifying functions on UI (Web or Mobile) and they can know exactly what APIs called under that UI. So, it can be grey box.

## My answer
I would want to discuss some factor to support my answer:
- Test target (or test object): API testing mean targets are APIs, it is not UI. So tester can use API to support during UI test, but API is not target of testing activity, it's UI. Hence the answer grey box with given explanation is not correct.
- Test techniques: Black box and white box are the names of sets of technique that are used during test.
  - Black box techniques include Equivalence partition, Boundary Value, Decision Table...
  - White box techniques includes Statement Coverage, Branch Coverage,...

Thus, my answer is "API testing is black box" that mean I will use some techniques of black box. In contrast, "API testing is white box" that means I will use white box technique to verify APIs. Either answer is correct.

However, when asking this question, normally we tend to mention sending APIs and check the response by using postman or any similar tools/frameworks. That's black box.

The question is try to match a testing type to a testing technique, that is not not properly.

