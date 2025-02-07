---
date: '2025-02-07T17:19:54+08:00'
title: 'Tdd Best Practices'
tags: ["Vladimir Khorikov"]
---

[Source](https://enterprisecraftsmanship.com/posts/tdd-best-practices/)

Last week, we discussed the differences between stubs and mocks. Today, we’ll talk about some general tips and advice that regard to TDD and writing tests in general.

- Test-induced design damage or why TDD is so painful
- How to do painless TDD
- Integration testing or how to sleep well at nights
- The most important TDD rule
- Stubs vs Mocks
- **TDD best practices**

## Test-first vs code-first approach

There’s some dispute about how exactly tests should be written. While the TDD process itself insists on the test-first approach, I personally think that both ways are applicable in different circumstances. Let’s look at the pros and cons of each of them.

關於測試應該如何撰寫存在一些爭議。雖然 TDD（測試驅動開發）過程本身強調先寫測試的方式，但我個人認為在不同情境下這兩種方式都適用。讓我們來看看它們各自的優缺點。

One of the benefits of the test-first approach is that it allows us to easily validate the tests we create. If we follow the red-green-refactor routine, the first step would always be creating a "red" test and thus making sure it’s actually exposing problems with our code.

先寫測試的好處之一是我們可以輕鬆驗證所創建的測試。如果我們遵循紅-綠-重構的流程，第一步總是先創建一個「紅色」的測試，從而確保這個測試確實揭露了程式碼中的問題。

With the code-first approach, unit tests are prone to false positives. As we create them after the implementation is done, there’s no guarantee these tests actually do their job. In order to verify that, we often need to manually change the implementation of the SUT (system under test) to make sure the test fails. With the test-first approach, this check is already wired into the red-green-refactor process.

使用先寫程式碼的方法時，單元測試容易出現偽陽性。由於這些測試是在實作完成後才撰寫的，因此無法保證它們真正發揮作用。為了驗證這一點，我們通常需要手動更改被測系統（SUT，系統待測）的實作，確保測試能夠失敗。而先寫測試的方式，這個檢查已經內建在紅-綠-重構的流程中了。

Another great advantage the test-first approach provides us is the ability to sketch our thoughts out. Following this way, we can create several failing tests and thus make ourselves a road map of what is needed to be done. It’s especially helpful in an environment where you are constantly distracted. This approach allows you to not forget where you had stopped and easily return to the flow. It is also a nice source of endorphins: watching your tests turning green one by one is always a pleasure.

先寫測試的另一個重要優勢是能夠幫助我們整理思路。透過這種方式，我們可以創建多個失敗的測試，為自己劃出需要完成的工作路線圖。這在容易受到干擾的環境中特別有幫助。這種方式可以幫助我們記住自己停在哪裡，並輕鬆地重新進入工作流程。此外，觀看測試一個接一個地由紅轉綠，總是一件令人滿足的事情。

On the other hand, the code-first approach is helpful when you need to investigate some new areas in your application. Also, the TDD process is often redundant when building a prototype. Most likely, such prototype will be deleted along with the tests covering it.

另一方面，當你需要探索應用程式中的某些新領域時，先寫程式碼的方法會更有幫助。此外，在建立原型時，TDD 的流程往往是多餘的。大多數情況下，這種原型以及覆蓋它的測試可能最終都會被刪除。 ^vladimirtddbestpractice1

I believe TDD does a great job when you know the complete set of requirements for the code you are going to write. In such cases, the test-first approach would indeed be the best choice.

我認為，當你清楚要撰寫程式碼的完整需求時，TDD 能夠發揮極大的作用。在這種情況下，先寫測試的方式確實是最佳選擇。

At the same time, if you are experimenting with different ideas in code or not exactly sure how your code should look like, you are better off not trying to come up with unit tests up-front. Instead, just write the code and add unit tests later. Also, sometimes it’s reasonable to switch between the two approaches, even within a single piece of functionality.

然而，如果你正在程式碼中試驗不同的想法，或者不確定程式碼應該如何設計，那麼最好不要急於一開始就編寫單元測試。相反，先撰寫程式碼，稍後再補充單元測試。此外，即使是針對同一功能，有時候在兩種方法之間切換也是合理的。

## Test doubles are for external (volatile) dependencies only

While using stubs is a good practice when you don’t have control over some dependencies in your code, I often see developers substituting even internal (stable) dependencies. Usually, the reason for that is isolation. In order to isolate a class from other classes completely, people sometimes try to mock even those of them that comprise a [cohesive group](https://enterprisecraftsmanship.com/2015/09/02/cohesion-coupling-difference/) with the system under test.

當您無法掌控程式碼中的某些相依項時，使用存根（stub）是一種良好的做法。然而，我經常看到開發者甚至對內部（穩定的）相依項進行替代。通常，這樣做的原因是為了實現隔離。為了完全將一個類別與其他類別隔離，有時人們甚至試圖模擬那些與待測系統構成[內聚群組](https://enterprisecraftsmanship.com/2015/09/02/cohesion-coupling-difference/)的類別。

Let’s take an example:

```csharp
public class Order
{
    public decimal Amount
    {
        get { return _lines.Sum(x => x.Price); }
    }
 
    public void AddLine(IOrderLine line)
    {
        _lines.Add(line);
    }
}

public interface IOrderLine
{
    decimal Price { get; set; }
    ProductProduct { get; set; }
}

[Fact]
public void Amount_shows_order_amount()
{
    // Arrange
    var mock = new Mock<IOrderLine>();
    mock.Setup(x => x.Price).Returns(10);
    var mock2 = new Mock<IOrderLine>();
    mock2.Setup(x => x.Price).Returns(20);
    var order = new Order();
    order.AddLine(mock.Object);
    order.AddLine(mock2.Object);
 
    // Act
    decimal amount = order.Amount;
 
    // Assert
    Assert.Equal(30, amount);
}
```

Here, we use IOrderLine interface in the Order class and mock it in the unit test.

在這裡，我們在 `Order` 類別中使用了 `IOrderLine` 介面，並在單元測試中對其進行了模擬。

What is the problem with such type of isolation? **It loosens cohesion between the Order and the OrderLine classes**. We shouldn’t break our code in places where it doesn’t break. It this particular situation, it would be better to use OrderLine as is, without trying to substitute it with an interface:

這種隔離方式有什麼問題呢？**它削弱了 `Order` 類別與 `OrderLine` 類別之間的內聚性**。我們不應該在程式碼沒有問題的地方製造問題。在這種情況下，最好直接使用 `OrderLine`，而不是試圖用介面來替代它：

```csharp
public class Order
{
    // Other methods
 
    public void AddLine(decimal price, Product product)
    {
        _lines.Add(new OrderLine(price, product));
    }
}

[Fact]
public void Amount_shows_order_amount()
{
    // Arrange
    var order = new Order();
    order.AddLine(10, new Product());
    order.AddLine(20, new Product());
 
    // Act
    decimal amount = order.Amount;
 
    // Assert
    Assert.Equal(30, amount);
}
```

That way, we keep our domain model cohesive and don’t introduce unnecessary seams in our code.

這樣一來，我們保持了領域模型的內聚性，並且不在程式碼中引入不必要的斷點。

## A single unit for unit-testing is an aggregate

Another best practice for unit testing a domain model is treating [aggregates](http://martinfowler.com/bliki/DDD_Aggregate.html) in it as a single SUT. OrderLine in the previous example is a child entity of the Order aggregate. We shouldn’t try to test it separately as it would break the aggregate’s encapsulation. A better way would be to consider both the Order and the OrderLine classes a single unit and test them respectively.

對領域模型進行單元測試的另一個最佳實踐是將[聚合](http://martinfowler.com/bliki/DDD_Aggregate.html)視為單一的待測系統（SUT）。在前面的例子中，`OrderLine` 是 `Order` 聚合的子實體。我們不應該單獨測試它，因為這樣做會破壞聚合的封裝性。更好的方法是將 `Order` 和 `OrderLine` 視為一個單位，並對其進行相應測試。

## Sharing code between test classes

Another rule that I think is worth following is the one that regards to sharing code between test classes.

另一個值得遵循的規則是關於在測試類別之間共享程式碼。

There are two types of the code in unit tests that could potentially be extracted and reused over several test classes: arranges and assertions.

單元測試中的程式碼可以分為兩種，這些程式碼可能被提取並在多個測試類別中重複使用：準備部分（arranges）和斷言部分（assertions）**。

Assertions is a good candidate for that. An example here could be a hand-written stub/spy (you can look at the HandlerStub from the [previous post](https://enterprisecraftsmanship.com/2015/07/27/stubs-vs-mocks/)) which provides methods validating its internal state or a utility class with extension methods that help simplify the assertions.

斷言部分是適合提取的候選者。一個例子可能是一個手寫的存根（stub）/間諜（spy）（可以參考[前文](https://enterprisecraftsmanship.com/2015/07/27/stubs-vs-mocks/)中的 `HandlerStub`），它提供了用於驗證其內部狀態的方法，或者是一個包含擴展方法的工具類別，用於簡化斷言。

We could use such utility class to replace this code:

我們可以使用這樣的工具類別來取代以下程式碼：

```csharp
Assert.Equal(1, user.DomainEvents.Count);
Assert.True(user.DomainEvents.Any(x => x.GetType() ==typeof(SeatDeletedEvent)));
```

With a less verbose one-liner:

用更簡潔的一行程式碼：

```csharp
user.ShouldContainSingleEvent<SeatDeletedEvent>();
```

[Fluent Assertions library](https://github.com/dennisdoomen/fluentassertions/wiki) goes even further and offers a reusable set of extension methods that simplifies assertions for most of the basic BCL types.

[Fluent Assertions 函式庫](https://github.com/dennisdoomen/fluentassertions/wiki) 更進一步，提供了一組可重複使用的擴展方法，簡化了對大多數基本 BCL 類型的斷言。

At the same time, code in Arrange sections isn’t something that can be reused as freely. It might seem a good idea to gather all arrange methods in a base test class and use them in every unit test:

然而，**Arrange 部分的程式碼並不是能夠如此自由重用的東西**。將所有的準備方法集中在基礎測試類別中，並在每個單元測試中使用，乍看之下似乎是個好主意：

```csharp
[Fact]
public void Fires_event_after_provisioning()
{
    // Arrange (methods are in the TestBase class)
    Organization organization = CreateOrganization();
    User user = CreateUser(organization);
    Subscription subscription = CreateSubscription(organization);
 
    // Act
    organization.ProvisionUser(user, subscription);
 
    // Assert
    user.ShouldContainSingleEvent<SeatCreatedEvent>();
}
```

But this would introduce too much of fragility. It’s really hard to maintain tests that depend on each other: any change in one of such methods can potentially break all tests that rely on it.

但這樣做會導致測試變得非常脆弱。維護相互依賴的測試非常困難：對其中某個方法的任何更改都有可能破壞所有依賴它的測試。

At the same time, it isn’t a good idea to give up on reusability in the arrange section completely. Just as in any field of software development, there should be a balance here.

同時，完全放棄在 Arrange 部分的重用性也不是一個好主意。正如在軟體開發的其他領域一樣，這裡也需要保持平衡。

I personally tend to not use base classes in my unit tests and thus, not share the arrange code between test classes. However, I do share it inside a single test class. In the example above, Create methods are stored as private methods inside the UserSpecs class and reused across its test methods.

我個人傾向於不在單元測試中使用基底類別，因此也不在測試類別之間共享 Arrange 的程式碼。然而，我會在單個測試類別內部共享這些程式碼。在上面的例子中，`Create` 方法被儲存為 `UserSpecs` 類別中的私有方法，並在其測試方法中重複使用。

I believe it’s a reasonable trade-off between the reusability and the fragility of the code. It does lead to some duplications, though. For example, UserSpecs and OrganizationSpecs might have the same CreateOrganization method.

我認為這是在程式碼重用性和脆弱性之間合理的權衡。儘管如此，這確實會導致某些重複。例如，`UserSpecs` 和 `OrganizationSpecs` 可能擁有相同的 `CreateOrganization` 方法。

In test classes, using composition instead of inheritance helps reduce coupling between them.

在測試類別中，使用組合而非繼承有助於減少它們之間的耦合。

## Summary

Let’s recap:

- Use test-first (TDD) approach if you know exactly what you want your code to do. Use code-first approach otherwise.
- Use test doubles only to substitute external dependencies.
- A single SUT in your domain model should be an aggregate.
- Reuse code in the Assert sections.
- Limit the code reuse in the Arrange sections.
- Prefer composition over inheritance in your test classes.

- 如果清楚知道程式碼應該完成什麼，請使用先寫測試（TDD）的方法；否則，請使用先寫程式碼的方法。
- 僅在替代外部相依項時使用測試替身（test doubles）。
- 在領域模型中，單一的 SUT 應該是一個聚合。
- 重用斷言（Assert）部分的程式碼。
- 限制 Arrange 部分程式碼的重用。
- 在測試類別中，偏好組合勝過繼承。

That wraps up this article series. It turned out to be twice longer than I initially excepted. I hope you found it interesting.

這篇文章系列到此結束。結果它的篇幅比我最初預期的長了兩倍。希望你覺得有趣！