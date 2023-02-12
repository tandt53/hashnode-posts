---
title: Do not put assertions in Page Objects, why?
subtitle: It is not a good practice when placing assertions in Page Objects.
tags: general, automation-test, assertions, page-objects, POM, best-practice,
domain: tandt53.com
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1676186567586/nvcrBWl1m.jpg?auto=compress
ignorePost: false
---

If you search for best practices for page object model in automation tests, you will find that many people suggest not using verify commands in Page Object (PO) methods. However, there are no any details' explanation to the reasons. Even the post on [https://martinfowler.com/bliki/PageObject.html](https://martinfowler.com/bliki/PageObject.html) doesn’t clarify the issue.

> I favor having no assertions in page objects. I think you can avoid duplication by providing assertion libraries for common assertions — which can also make it easier to provide good diagnostics. [3]
> 
> 3: One form of assertions is fine even for people like me who generally favor a no-assertion style. These assertions are those that check the invariants of a page or the application at this point, rather than specific things that a test is probing.

My goal with this post is to clarify the reasons behind this suggestion based on my experience and to help you to understand it more clearly.

## Bullet No. 1
We should define a specific `responsibility` for each layer in our automation project architecture. According to [my last post](https://tandt53.com/test-automation-architecture), assertion commands are normally placed in the test layer. However, if they are also placed in the page object layer, there is a mixture of verification functions in 2 layers. That situation should be avoided.

![Test layer and Business Layer](https://cdn.hashnode.com/res/hashnode/image/upload/v1676186004771/3-a5ar4H4.webp?auto=compress)

## Bullet No. 2
You can consider methods implemented at Page Objects like APIs that communicate between client (test scenario) and server (System Under Test — SUT). These methods are usually carrying the requests (and parameters) from test layer, to automatically interact with SUT. Then they can get values from SUT and return to test layer for verification purpose. Obviously, you have never seen any APIs contains verification, don’t you?
![Page object methods like APIs](https://cdn.hashnode.com/res/hashnode/image/upload/v1676186056175/ynip0STzt.webp?auto=compress)

## Bullet No. 3
The last reason is also a complicated case that you might encounter if you put assertions at the PO level.
![Problem](https://cdn.hashnode.com/res/hashnode/image/upload/v1676186079585/CRXZUbwqD.webp?auto=compress)

Since PO methods are reusable, let’s consider the case where 2 tests use the same PO method, but that method contains verification commands for only 1 test. If that verification is failed, what will happen to these test cases? Both are failed, but:
- `Test01` is fine because the test purpose is covered.
- For `Test02`, it’s not fine because its checkpoints are not checked. Main checkpoints may come after another PO method is called. Therefore, the checkpoints for `Test02` are not covered, and we will waste time checking whether the failure is related to `Test02`.

![Solution](https://cdn.hashnode.com/res/hashnode/image/upload/v1676186111257/HSQGaCsvx.webp?auto=compress)

To resolve `Test02` problem, we should put assertion commands in proper test cases. As the result, we will have below advantages:
- **Tests are atomic**: each test has its own checkpoints
- The **checkpoints are all visible at the test level** so that a reviewer can see the whole test without digging into the lower layers.
- **Reduce execution time**: if the verification needs to get data from the SUT, time will vary based on the number of times the PO method is used, and the connection between the test and cloud services.

## Summary
With the 3 bullets above, hopefully now you have a much better understanding of why assertion commands should be placed outside of Page Objects. In addition, it’s vital to remember that separated `responsibility` applies not just to methods, but also to layers in `architecture`.

Cover Photo by <a href="https://unsplash.com/@freestocks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">freestocks</a> on <a href="https://unsplash.com/photos/40k6ZqbsXuo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>