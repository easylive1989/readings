---
date: '2025-02-10T08:48:57+08:00'
title: 'Domain services vs Application Service'
tags: ["Vladimir Khorikov"]
---

[Source](https://enterprisecraftsmanship.com/posts/domain-vs-application-services/)

In this post, we’ll take a look at domain services: what differs them from application services and when it is preferable to use one in addition to an application service.

在這篇文章中，我們將探討領域服務（Domain Services）：它們與應用服務（Application Services）的區別，以及什麼時候適合在應用服務之外使用領域服務。

## 1. Domain services vs Application services

It is often said that domain services carry domain knowledge that doesn’t naturally fit entities and value objects. There’s another reason, however, why you may want to introduce a domain service. That reason is related to domain model isolation. More on that in just a minute.

人們常說，領域服務承載著無法自然地融入實體和值對象的領域知識。然而，還有另一個理由可能促使您引入領域服務，那就是**領域模型的隔離性**。稍後我們會更深入地探討。

So, what differs domain services from application services? Both these concepts assume stateless classes which can work on top of domain entities and value objects, but that’s pretty much as far as their similarities go. The main difference between them is that **domain services hold domain logic whereas application services don’t**.

那麼，領域服務和應用服務有什麼不同？這兩者都假設是無狀態（stateless）的類別，並可基於領域實體和值對象運作。然而，它們的相似點僅止於此。兩者的主要區別在於： **領域服務包含領域邏輯，而應用服務則不包含。**

As we discussed in a [previous post](https://enterprisecraftsmanship.com/2016/08/25/what-is-domain-logic/), domain logic is everything that is related to business decisions. Domain services, therefore, participate in the decision-making process the same way entities and value objects do. And application services orchestrate those decisions the same way they orchestrate decisions made by entities and value objects.

如我們在[上一篇文章](https://enterprisecraftsmanship.com/2016/08/25/what-is-domain-logic/)中所討論的，領域邏輯涵蓋了與業務決策相關的所有內容。因此，領域服務與實體和值對象一樣，參與決策過程；而應用服務則負責協調這些由實體和值對象作出的決策。

Let’s take this example:

```csharp
public void WithdrawMoney(decimal amount)
{
    _atm.DispenseMoney(amount);
    decimal amountWithCommission = _atm.CalculateAmountWithCommission(amount);
 
    _paymentGateway.ChargePayment(amountWithCommission);
    _repository.Save(_atm);
}
```

The `WithdrawMoney` method is part of an application service and comprises a customer-facing API. It tells an ATM entity to dispense some amount of money first, then asks it to calculate the amount of money with a commission, uses the calculation result to charge the payment through a payment gateway and finally saves the entity to the database.

`WithdrawMoney` 方法是應用程式服務的一部分，並且包含一個面向客戶的 API。它首先指示 ATM 實體取出一定數量的金錢，然後要求其計算帶有手續費的金額，使用計算結果通過支付閘道進行扣款，最後將該實體儲存到資料庫中。

Does this application service look good or should we extract some code out of it into a domain service? Let’s see.

這個應用程式服務看起來不錯，還是我們應該將其中的一些程式碼提取到領域服務中？讓我們來看看。

In the first two lines, the method uses the Atm domain entity to make some business decisions. With the last two lines, it transforms those decisions into visible side effects. It calls the payment gateway and modifies the state of the database.

在前兩行中，該方法使用了 Atm 的領域實體來進行一些業務決策。而在最後兩行中，這些決策被轉化為可見的副作用：呼叫支付閘道並修改資料庫的狀態。

The first two lines here are about delegating the decision-making process to the domain model. But isn’t the sole act of knowing which two lines of code to use a domain knowledge in itself? Shouldn’t we extract it into a domain service? For example, like this:

這裡的前兩行是關於將決策過程委派給領域模型的。但僅僅是知道要使用哪兩行程式碼，這本身難道不是一種領域知識嗎？我們是不是應該將它提取到領域服務中呢？例如，可以像這樣：

```csharp
public void WithdrawMoney(decimal amount)
{
    decimal amountWithCommission = _atmService.DispenseAndCalculateCommission(
        _atm, amount);
 
    _paymentGateway.ChargePayment(amountWithCommission);
    _repository.Save(_atm);
}

public sealed class AtmService // Domain service
{
    public decimal DispenseAndCalculateCommission(Atm atm, decimal amount)
    {
        atm.DispenseMoney(amount);
        return atm.CalculateAmountWithCommission(amount);
    }
}
```

Not really. The mere fact of using more than one line of code that is somehow related to the domain model doesn’t constitute a domain knowledge. What matters is whether or not those lines of code are responsible for making business decisions.

不完全是。僅僅是使用了多行與領域模型相關的程式碼，並不構成領域知識。關鍵在於，這些程式碼是否負責進行業務決策。

In the sample above, the `DispenseAndCalculateCommission` method has a cyclomatic complexity of 1. There are no branches (`if` statements) that signalize the code makes any decisions whatsoever, it just asks the domain entity to do two separate things.

在上述範例中，`DispenseAndCalculateCommission` 方法的循環複雜度為 1。程式碼中沒有任何分支（如 `if` 語句）表明它在進行決策，它只是請求領域實體執行兩個獨立的操作。

The order in which these two methods are called doesn’t matter either. If we were to re-arrange them and put the commission calculation query above the dispense money command, nothing would change in terms of the `Atm`'s invariants, it would still remain valid. It’s a strong sign there are no leaking [implementation details](https://enterprisecraftsmanship.com/2016/07/27/what-is-an-implementation-detail/) either.

這兩個方法的呼叫順序也無關緊要。如果我們重新排列，把計算手續費的查詢放在取款命令之前，就 `Atm` 的不變性而言，結果也不會有任何改變，`Atm` 的狀態仍然是有效的。這強烈表明，這裡也沒有洩漏任何[實作細節](https://enterprisecraftsmanship.com/2016/07/27/what-is-an-implementation-detail/)。

Now, let’s change the code sample a little bit and say that there’s also a validation involved:

現在，讓我們稍微修改一下程式碼範例，假設其中還涉及驗證：

```csharp
public void WithdrawMoney(decimal amount)
{
    if (!_atm.CanDispenseMoney(amount))
        return ;
 
    _atm.DispenseMoney(amount);
    decimal amountWithCommission = _atm.CalculateAmountWithCommission(amount);
 
    _paymentGateway.ChargePayment(amountWithCommission);
    _repository.Save(_atm);
}
```

The cyclomatic complexity in this example is higher than one because we now have an "if" statement. Doesn’t it mean the application service now contains domain knowledge?

這個範例中的循環複雜度比 1 更高，因為我們現在有了一個 "if" 條件判斷。這是否意味著應用程式服務現在包含了領域知識？

Also, no. The actual decision-making process still resides in Atm. The entity and entity alone decides whether it can dispense any money. The application service just orchestrates that decision and either continues the execution or not.

答案仍然是否定的。實際的決策過程仍然位於 Atm 中。只有該實體（Atm）決定它是否可以提取金額。應用程式服務僅負責協調該決策，然後決定是繼續執行還是中止。

As long as the `DispenseMoney` method in Atm has a pre-condition stating that `CanDispenseMoney` must hold true prior to dispensing cash, all invariants stay protected. The precondition itself can be implemented as simple as this:

只要 Atm 的 `DispenseMoney` 方法具有一個前置條件，聲明在提款之前 `CanDispenseMoney` 必須為真，所有的不變性都會被保護。這個前置條件本身可以實現得非常簡單，例如：

```csharp
public void DispenseMoney(decimal amount)
{
    if (!CanDispenseMoney(amount))
        throw new InvalidOperationException();
 
    /* ... */
}
```

So, even if the application service ignores the decision made by `CanDispenseMoney`, the Atm entity will not enter an inconsistent state. It will throw an exception thus adhering to the [fail fast principle](https://enterprisecraftsmanship.com/2015/09/15/fail-fast-principle/).

因此，即使應用程式服務忽略了由 `CanDispenseMoney` 做出的決策，Atm 實體也不會進入不一致的狀態。它會拋出異常，從而遵守[快速失敗原則](https://enterprisecraftsmanship.com/2015/09/15/fail-fast-principle/)。

## 2. When to extract a domain service?

The application service in the sample above doesn’t make any business decisions, it delegates those decision to the domain model. Note that the domain model is isolated: the Atm entity doesn’t save itself to the database and doesn’t directly charge payments through the payment gateway. We have a nice separation of concerns here: business logic is attributed to the domain model, while interactions with the external world - to the application service.

上述範例中的應用程式服務並未做出任何業務決策，而是將這些決策委派給了領域模型。請注意，領域模型是被隔離的：Atm 實體不會將自己保存到資料庫中，也不會直接通過支付閘道進行扣款。我們在這裡實現了良好的職責分離：業務邏輯歸屬於領域模型，而與外部世界的交互則歸應用程式服務負責。

You can notice a pattern in most code bases that adhere to such a guideline. Their execution flow goes as follows:

大多數遵循此類準則的程式碼庫中，都可以發現以下模式。其執行流程如下：

- Prepare all information needed for a business operation: load participating entities from the database and retrieve any required data from other external sources.
- Execute the operation. The operation consists of one or more business decisions made by the domain model. Those decisions result in either changing the model’s state, generating some artifacts (`amountWithCommission` value in the sample above), or both.
- Apply the results of the operation to the outside world.

- 準備業務操作所需的所有資訊：從資料庫中加載參與的實體，並從其他外部來源檢索所需的資料。
- 執行操作。該操作由領域模型做出一個或多個業務決策。這些決策可能導致改變模型的狀態、生成某些工件（如上述範例中的 `amountWithCommission` 值），或者兩者兼而有之。
- 將操作的結果應用到外部世界。

Only the 1st and the 3rd steps involve the work with external dependencies. The 2nd step is closed under the data retrieved in the first step. The arguments it accepts and the output it generates consist of entities, value objects, and primitive types only.

只有第 1 步和第 3 步涉及與外部依賴的工作。第 2 步僅基於第 1 步中檢索到的資料進行。它接受的參數和生成的輸出只包括實體、值物件和基本類型。

Note that in simple CRUD applications, there’s no 2nd step because there are no decisions to make. In this case, all operations can be performed solely by application services, no need to delegate them to a domain model. In fact, there can be no rich domain model whatsoever. An anemic domain model would work just fine in such situations.

請注意，在簡單的 CRUD 應用程式中，沒有第 2 步，因為不需要做出任何決策。在這種情況下，所有操作都可以完全由應用程式服務執行，無需委派給領域模型。事實上，在這種情況下可能根本不存在豐富的領域模型。貧血領域模型在這些情境中完全足夠。

Now, let’s modify our code sample to a more realistic scenario. Let’s assume that the payment charge can fail due to insufficient balance and that we shouldn’t dispense any cash if that happens.

現在，讓我們將程式碼範例修改為一個更現實的場景。假設支付扣款可能因餘額不足而失敗，而如果發生這種情況，我們不應該提取任何現金。

This is where the nice separation of concerns I described above starts to break apart. The decision process now depends on information unavailable by the time we start that decision process. Here’s the code:

這正是上述良好職責分離開始崩解的地方。決策過程現在取決於在啟動該決策過程時尚不可用的資訊。以下是程式碼：

```csharp
public void WithdrawMoney(decimal amount)
{
    if (!_atm.CanDispenseMoney(amount))
        return ;
 
    decimal amountWithCommission = _atm.CalculateAmountWithCommission(amount);
    Result result = _paymentGateway.ChargePayment(amountWithCommission);
 
    if (result.IsFailure)
        return ;
 
    _atm.DispenseMoney(amount);
    _repository.Save(_atm);
}
```

In this version, the second `if` statement we introduced does represent domain logic. It decides whether we will be dispensing cash to the user. However, unlike in the first conditional operator, it’s not the Atm entity who makes that decision. It’s the application service itself.

在這個版本中，我們引入的第二個 `if` 條件語句確實代表了領域邏輯。它決定我們是否要向使用者發放現金。然而，與第一個條件運算符不同的是，這個決策並非由 Atm 實體做出，而是由應用程式服務本身負責。

It is possible now to take cash from the Atm even if the payment has failed prior to it. The domain entity doesn’t hold this invariant for us. And it’s impossible to introduce such an invariant without violating the entity’s isolation because in order to check this precondition it will have to call the 3rd party service.

現在，即使支付在之前失敗，也可以從 Atm 中取現金。領域實體無法為我們維護這個不變性。而且，在不違反實體隔離性的情況下，無法引入這樣的不變性，因為要檢查這個前置條件，實體必須呼叫第三方服務。

So, what to do in such situation? This is where domain services can be helpful. You can attribute to them any business decisions which require additional information from the external world and which cannot be made by entities and value objects because of that:

那麼，在這種情況下應該怎麼做呢？這正是領域服務能發揮作用的地方。對於任何需要從外部世界獲取額外資訊，並且因為這些資訊無法由實體和值物件完成的業務決策，都可以交由領域服務處理：

```csharp
public void WithdrawMoney(decimal amount)
{
    Atm atm = _repository.Get();
    _atmService.WithdrawMoney(atm, amount);
    _repository.Save(_atm);
}

public sealed class AtmService // Domain service
{
    public void WithdrawMoney(Atm atm, decimal amount)
    {
        if (!atm.CanDispenseMoney(amount))
            return ;
 
        decimal amountWithCommission = atm.CalculateAmountWithCommission(amount);
        Result result = _paymentGateway.ChargePayment(amountWithCommission);
 
        if (result.IsFailure)
            return;
 
        atm.DispenseMoney(amount);
    }
}
```

The domain service here is a middle ground between impurity and the amount of complexity / business logic held. On one hand, we can’t make this service completely isolated because it has to work with the payment gateway in order to do its work. On the other, we don’t attribute too much domain logic to it, only the knowledge regarding how to exchange cash for credit.

這裡的領域服務是純度與所承載的複雜性/業務邏輯之間的一個折衷。一方面，我們無法讓這個服務完全隔離，因為它需要與支付閘道交互來完成其工作；另一方面，我們沒有將過多的領域邏輯歸屬於它，而只是包含如何將現金兌換為信用的相關知識。

We still attribute as much logic as possible to the entity. For example, the act of dispensing cash is still a responsibility of Atm. And we still try to isolate the domain service as much as possible. For example, we don’t make it work with the repository as it is not required to make the business decision. The impurity and the domain logic introduced to the service are the bare minimum here. Just enough for it to work properly.

我們仍然盡可能將邏輯歸屬於實體。例如，取現這一行為仍然是 Atm 的責任。而且，我們依然嘗試盡可能隔離領域服務。例如，我們沒有讓它與儲存庫（repository）交互，因為這對做出業務決策並非必要。服務中引入的不純性和領域邏輯保持在最低限度，只是足以讓它正常運行。

This extraction may look questionable. Isn’t it just a shift of responsibilities with no practical benefits? There are some benefits, however. The code in the domain service is more testable than in the application service. It has fewer external dependencies and therefore we need to use fewer test doubles in order to unit test it. This service is of course not as testable as the entity but still. The second benefit is that this way we prevent domain knowledge leakage and keep all domain logic within the domain model boundary which may be helpful for readability.

這樣的提取看起來可能有些令人質疑：這難道不只是職責的轉移而沒有實際益處嗎？然而，它確實有一些好處。領域服務中的程式碼比應用程式服務中的程式碼更容易進行測試。它的外部依賴更少，因此在單元測試中需要使用的測試替身（test doubles）也更少。當然，這個服務的可測試性不如實體，但依然有一定改善。第二個好處是，我們以這種方式防止了領域知識的洩漏，並將所有領域邏輯維持在領域模型的邊界內，這對於提升可讀性可能會有所幫助。

I wouldn’t say that in this particular example these two benefits play a significant part. It’s mostly fine to keep a little bit of logic that doesn’t fit an entity in an application service itself and not introduce a separate domain service every single time. Make sure, however, that this logic is not duplicated and that it is not too complex. If you see the DRY principle is violated or the application service becomes too complex, you need to definitely introduce a domain service.

在這個具體範例中，我不會說這兩個好處是決定性的。保留一點不適合實體的邏輯在應用程式服務本身，並不總是需要每次都引入一個單獨的領域服務。然而，要確保這些邏輯沒有重複，且不過於複雜。如果你發現 DRY 原則被違反了，或者應用程式服務變得過於複雜，那麼你就一定需要引入一個領域服務。

## 3. Injecting a domain service into an entity

A question I sometimes hear is: can a domain service be injected into an entity?

我有時會聽到這樣一個問題：領域服務可以注入到實體中嗎？

I personally distinct two types of domain services: pure (isolated) and impure (non-isolated). The former is closed under entities and value objects and doesn’t depend on the external world. `AtmService` is an example of an impure domain service.

我個人將領域服務區分為兩種類型：純粹的（隔離的）和不純粹的（非隔離的）。純粹的領域服務只涉及實體和值物件，並且不依賴外部世界。`AtmService` 則是非純粹領域服務的一個例子。

Injection of an impure domain service into entities breaks the isolation, so I’d recommend against it. On the other hand, a pure domain service doesn’t do any harm, so it’s totally fine to refer to them from your entities and value objects.

將不純粹的領域服務注入到實體中會破壞其隔離性，因此我建議不要這樣做。另一方面，純粹的領域服務並不會造成任何問題，因此從實體和值物件中引用它們是完全可以接受的。

## 4. Summary

- Domain services carry domain knowledge; application services don’t (ideally).
- Domain services hold domain logic that doesn’t naturally fit entities and value objects.
- Introduce domain services when you see that some logic cannot be attributed to an entity/value object because that would break their isolation.

- 領域服務承載領域知識；應用程式服務則不承載（理想情況下）。
- 領域服務負責處理不自然適合歸屬於實體和值物件的領域邏輯。
- 當你發現某些邏輯無法歸屬於實體/值物件，因為那會破壞它們的隔離性時，應該引入領域服務。

## 5. Related articles
- [What is domain logic?](https://enterprisecraftsmanship.com/2016/08/25/what-is-domain-logic/)
- [Domain model isolation](https://enterprisecraftsmanship.com/2016/09/01/domain-model-isolation/)
- [Services in Domain-Driven Design](http://gorodinski.com/blog/2012/04/14/services-in-domain-driven-design-ddd/) by Lev Gorodinski