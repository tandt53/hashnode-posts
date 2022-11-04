---
title: State management in automation test scenario
subtitle: The post will give a hint to manage state efficiently and well-organized 
tags: general, automation-test, state-management
domain: tandt53.com
ignorePost: false
---

Along with well-organized code in a scenario, it is necessary to manage values, especially in End-2-End scenarios. Thus, this post will suggest a flow of those values inside an automation test case.
![State-Management](https://cdn.hashnode.com/res/hashnode/image/upload/v1665497896647/PUTI3h7X30.png?auto=compress)

There are 2 main levels:
- **Test level** includes all steps related to behavior and verification. The values will be referred to as entities or DAOs.
- **Page Objects level**, as defined in [Martin Fowler's post](https://martinfowler.com/bliki/PageObject.html), includes methods inside a page or fragment/layout of a page.

## State
The term `state` is related to the status of an application during execution. It can be represented by individual values or combined properties to form object(s). In a test case, it is usually created, transferred, updated for business actions, and checked for verification.

## Example 01
Let's take a simple example:
```java
@Test
public void happyCase(){
  // Arrange 
  System.setProperty("webdriver.chrome.driver", "chromedriver.exe");
  WebDriver driver = new ChromeDriver();
  CalculationPage page = new CalculationPage(driver);
  int result;
  int a = 4;
  int b = 6;
  int expected = 10;

  // Actions
  page.sum(a, b);
  result = page.getResult();

  // Assert 
  Assert.assertEquals(result, expected)
}
```

As can be seen from above code, the variables `a`, `b` and `result` are considered as states.
- `a` and `b` are created and initialized in *Arrange* stage.
- Then these variables are passed to the `sum()` method of `CalculationPage`
- After method consumes variables, it will return `result`.
- The returned value is compared with the expected value.

## Example 02
Let's create a more complicated example. Assume that we have to design a test case to test the payment flow. So let's create some classes below:
```java
// using lombok annotation
@Setter
@Getter
public class TransactionInfo {
    private String name;
    private String id;
    private double amount;
    private String note;
    private String currency;
}
```

Then we have test like this:
```java
@Test
public void transactionTest() {
    // Arrange
    TransactionInfo txn = new TransactionInfo();
    txn.setAmount(10000);
    txn.setCurrency("USD");
    txn.setNote("testing transaction");

    WebDriver driver = new ChromeDriver();
    TransactionPage txnPage = new TransactionPage(driver);
    TransactionResultPage txnResultPage = new TransactionResultPage(driver); // this page will fully display all transaction info

    // Act
    String txnId = txnPage.make(txn);
    txn.setId(txnId);

    TransactionInfo actualTxn = txtResultPage.getTransaction();

    // Assert
    Assert.assertEquals(txn, actualTxn);
}
```

Now state contains properties of `TransactionInfo` object. As you can see:
- Some properties of `txn` are defined first, in *Arrange* stage. 
- Then `txn` variable are passed to method `make()` to get back transaction id and set to the `id`.
- Next is getting a new instance of transaction info in `txtResultPage.getTransaction()`.
- Finally, compare state `txn` with `actualTxn`. Because putting assert command in page object is not good practice, so we will get transaction from page object level to test level.

## Summary 
Hopefully, after observing the 2 examples above, you now have a clear view of state management in automation testing. There are some principles that you can follow:
- State values should live within only 1 test case, because all test cases should be independent of each other.
- State can be created at the beginning of a test case, and added or edited during the test steps.
- State can be shared between steps in a scenario, but should not be shared between pages directly. If you want to transfer state from page 1 (of step 1) to page 2 (of step 2), page 1 should return to step 1, and then step 2 will take state and pass it to page 2.
```
- Don't: page 1 -> page 2
- Do: page 1 -> step 1 -> step 2 > page 2
```
- Stage is normally provided by steps and consumed by pages. Sometimes, the state comes from returned values of page object like Example 2 getTransaction().
