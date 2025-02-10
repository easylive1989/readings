---
date: '2025-02-10T08:50:17+08:00'
title: 'Domain model purity vs. domain model completeness (DDD Trilemma)'
tags: ["Vladimir Khorikov"]
---

[Source](https://enterprisecraftsmanship.com/posts/domain-model-purity-completeness/)

I’ve been meaning to write this article for a long time and, finally, here it is: the topic of domain model purity versus domain model completeness.

我一直想寫這篇文章，終於完成了！主題是關於領域模型的純粹性與領域模型的完整性。
## Domain model completeness

In this article, we’ll talk about a trilemma that comes up in each and every project. In fact, I received a dozen or so questions about this trilemma during the last year or two (slightly embarrassing to realize how long some article ideas spend in my write-up queue).

在這篇文章中，我們將探討每個專案中都會出現的一個三難問題。事實上，在過去的一兩年裡，我收到過十幾個關於這個三難問題的提問（稍微有點尷尬，想到有些文章構想在我的撰寫清單中待了這麼久）。

To best describe this trilemma, we need to take an example. Let’s say that we’ve got a user management system with one use case so far: changing the user email. Here’s how the `User` domain class looks:

為了更好地描述這個三難問題，我們需要一個範例。假設我們有一個用戶管理系統，目前有一個使用案例：更改用戶的電子郵件。以下是 `User` 領域類別的樣貌：

```csharp
public class User : Entity
{
    public Company Company { get; private set; }
    public string Email { get; private set; }

    public Result ChangeEmail(string newEmail)
    {
        if (Company.IsEmailCorporate(newEmail) == false)
            return Result.Failure("Incorrect email domain");

        Email = newEmail;

        return Result.Success();
    }
}

public class Company : Entity
{
    public string DomainName { get; }

    public bool IsEmailCorporate(string email)
    {
        string emailDomain = email.Split('@')[1];
        return emailDomain == DomainName;
    }
}
```

And this is the controller that orchestrates this use case:

```csharp
public class UserController
{
    public string ChangeEmail(int userId, string newEmail)
    {
        User user = _userRepository.GetById(userId);

        Result result = user.ChangeEmail(newEmail);
        if (result.IsFailure)
            return result.Error;

        _userRepository.Save(user);

        return "OK";
    }
}
```

This is an example of a rich domain model: all business rules (also known as domain logic) are located in the domain classes. There’s one such rule currently — that we can only assign to the user an email that belongs to the corporate domain of that user’s company. There’s no way for the client code to bypass this invariant — a hallmark of a highly encapsulated domain model.

這是一個豐富領域模型（rich domain model）的範例：所有業務規則（也稱為領域邏輯）都位於領域類別中。目前有一條規則 —— 我們只能將屬於用戶公司企業域的電子郵件分配給該用戶。客戶端程式碼無法繞過這個不變性（invariant），這是高度封裝的領域模型的特徵。

We can also say that our domain model is **_complete_**. A complete domain model is a model that contains all the application’s domain logic. In other words, there’s no domain logic fragmentation.

我們也可以說，我們的領域模型是**完整的**。完整的領域模型是一個包含應用程式所有領域邏輯的模型。換句話說，沒有領域邏輯的分散情況。

**_Domain logic fragmentation_** is when the domain logic resides in layers other than the domain layer. In our example, the `UserController` (which belongs to the application services layer) doesn’t contain any such logic, it serves solely as a coordinator between the domain layer and the database.

**領域邏輯分散**是指領域邏輯存在於非領域層的其他層中。在我們的範例中，`UserController`（屬於應用程式服務層）不包含任何此類邏輯，它僅僅作為領域層與資料庫之間的協調器。

## Domain model purity

Let’s now say that we need to implement another business rule: before changing the user email, the system has to check whether the new email is already taken.

現在，假設我們需要實現另一條業務規則：在更改用戶的電子郵件之前，系統必須檢查新電子郵件是否已被使用。

Here’s a common way to verify the email uniqueness:

以下是一種常見的檢查電子郵件唯一性的方法：

```csharp
// UserController
public string ChangeEmail(int userId, string newEmail)
{
    /* The new validation */
    User existingUser = _userRepository.GetByEmail(newEmail);
    if (existingUser != null && existingUser.Id != userId)
        return "Email is already taken";

    User user = _userRepository.GetById(userId);

    Result result = user.ChangeEmail(newEmail);
    if (result.IsFailure)
        return result.Error;

    _userRepository.Save(user);

    return "OK";
}
```

This gets the job done, but this solution introduces domain logic fragmentation. The domain layer no longer contains all the business rules, one of them has drifted to the controller. It’s also possible now to change the user email without checking for its uniqueness first, which means our domain model is not fully encapsulated.

這種方法可以完成任務，但它引入了領域邏輯的分散問題。領域層不再包含所有的業務規則，其中一條規則已經轉移到了控制器中。現在也有可能在未先檢查電子郵件唯一性的情況下更改用戶的電子郵件，這意味著我們的領域模型不再是完全封裝的。

Is there a way to restore domain model completeness?

有辦法恢復領域模型的完整性嗎？

There is. We can move the responsibility to verify the email uniqueness inside the `User` class, like this:

是有的。我們可以將檢查電子郵件唯一性的責任移到 `User` 類別內，例如以下做法：

```csharp
// User
public Result ChangeEmail(string newEmail, UserRepository repository)
{
    if (Company.IsEmailCorporate(newEmail) == false)
        return Result.Failure("Incorrect email domain");

    User existingUser = repository.GetByEmail(newEmail);
    if (existingUser != null && existingUser != this)
        return Result.Failure("Email is already taken");

    Email = newEmail;

    return Result.Success();
}

// UserController
public string ChangeEmail(int userId, string newEmail)
{
    User user = _userRepository.GetById(userId);

    Result result = user.ChangeEmail(newEmail, _userRepository);
    if (result.IsFailure)
        return result.Error;

    _userRepository.Save(user);

    return "OK";
}
```

This version gets rid of domain model fragmentation, but at the expense of another important property: **_domain model purity_**. A pure domain model is a model that doesn’t reach out to out-of-process dependencies. To be pure, domain classes should only depend on primitive types or other domain classes.

這個版本消除了領域模型的分散問題，但卻犧牲了另一個重要特性：**領域模型的純粹性（domain model purity）**。純粹的領域模型是一個不依賴於進程外部依賴的模型。為了保持純粹性，領域類別應該只依賴於基本類型或其他領域類別。

In our example, we’ve lost purity because the `User` now talks to the database. And no, replacing `UserRepository` with an `IUserRepository` interface won’t help:

在我們的範例中，純粹性已經丟失，因為 `User` 現在需要與資料庫交互。而且，不要以為用 `IUserRepository` 介面替換 `UserRepository` 就能解決這個問題：

```csharp
public Result ChangeEmail(string newEmail, IUserRepository repository)
```

Replacing it with a delegate won’t help either:

將其替換為委派（delegate）也無濟於事：

```csharp
public Result ChangeEmail(string newEmail, Func<string, bool> isEmailUnique)
```

Both of these alternatives still make the `User` class reach out to the database, and thus [don’t bring domain model purity back](https://enterprisecraftsmanship.com/posts/how-to-know-if-your-domain-model-is-properly-isolated/).

這兩種替代方案仍然讓 `User` 類別需要訪問資料庫，因此[無法恢復領域模型的純粹性](https://enterprisecraftsmanship.com/posts/how-to-know-if-your-domain-model-is-properly-isolated/)。

This is where the choice between domain model completeness and purity comes from. You can’t have both at the same time.

這就是領域模型完整性與純粹性之間的抉擇由來。這兩者無法同時兼得。

## The trilemma 三難困境

Then why did I call it trilemma and not dilemma? That’s because there’s a third component here, **_application performance_**, and sometimes you can give it up in favor of having both domain model purity and completeness.

那麼，為什麼我稱之為三難困境，而不是兩難困境？因為這裡還有第三個要素：**應用程式效能（application performance）**。有時，為了同時實現領域模型的純粹性和完整性，你可能需要犧牲效能。

In theory, you could load all the existing users into memory and pass them to `User` as an argument:

理論上，你可以將所有現有的用戶加載到記憶體中，並將它們作為參數傳遞給 `User`：

```csharp
// User
public Result ChangeEmail(string newEmail, User[] allUsers)
{
    if (Company.IsEmailCorporate(newEmail) == false)
        return Result.Failure("Incorrect email domain");

    bool emailIsTaken = allUsers.Any(x => x.Email == newEmail && x != this);
    if (emailIsTaken)
        return Result.Failure("Email is already taken");

    Email = newEmail;

    return Result.Success();
}

// UserController
public string ChangeEmail(int userId, string newEmail)
{
    User[] allUsers = _userRepository.GetAll();

    User user = allUsers.Single(x => x.Id == userId);

    Result result = user.ChangeEmail(newEmail, allUsers);
    if (result.IsFailure)
        return result.Error;

    _userRepository.Save(user);

    return "OK";
}
```

This version’s domain model is pure — `User` now only depends on other users. It is also complete — all the validations are located in the domain layer. But of course, it’s not practical from the performance standpoint, because we have to query all existing users on each email modification.

這個版本的領域模型是純粹的——`User` 現在只依賴其他用戶。而且它也是完整的——所有的驗證邏輯都位於領域層。然而，從效能的角度來看，這並不實用，因為每次更改電子郵件時都需要查詢所有現有用戶。

That’s where the trilemma comes into play. You can’t have all 3 of the following attributes:

這就是三難困境的由來。你無法同時具備以下三個屬性：

- **_Domain model completeness_** — When all the application’s domain logic is located in the domain layer, i.e. not fragmented.
- **_Domain model purity_** — When the domain layer doesn’t have out-of-process dependencies.
- **_Performance_**, which is defined by the presence of unnecessary calls to out-of-process dependencies.

- **領域模型的完整性**——當應用程式的所有領域邏輯都位於領域層，而不是分散在其他層。
- **領域模型的純粹性**——當領域層不依賴於進程外部的依賴。
- **效能**——由於避免了不必要的進程外部依賴呼叫所帶來的效能優勢

You have 3 options here, but each of them only gives you 2 out of the 3 attributes:

你有三個選擇，但每個選擇只能實現其中兩個屬性：

- **_Push all external reads and writes to the edges of a business operation_** — Preserves domain model completeness and purity but concedes performance.
- **_Inject out-of-process dependencies into the domain model_** — Keeps performance and domain model completeness, but at the expense of domain model purity.
- **_Split the decision-making process between the domain layer and controllers_** — Helps with both performance and domain model purity but concedes completeness. With this approach, you need to introduce decision-making points (business logic) in the controller.

- **將所有的外部讀取和寫入操作推到業務操作的邊緣**——保留領域模型的完整性和純粹性，但犧牲效能。
- **將進程外部的依賴注入到領域模型中**——保留效能和領域模型的完整性，但犧牲純粹性。
- **將決策過程分散在領域層和控制器之間**——保留效能和領域模型的純粹性，但犧牲完整性。這種方法需要在控制器中引入決策點（業務邏輯）。

![There’s no single solution that satisfies all three attributes: domain model completeness, domain model purity, and performance. You have to choose two out of the three.](https://enterprisecraftsmanship.com/images/2020/2020-08-06-trilemma.png)

The first approach (pushing external reads and writes to the edges of a business operation) is sometimes acceptable. It works best when a business operation naturally follows the read-decide-act structure, where it has three distinct stages:

第一種方法（將所有的外部讀取和寫入操作推到業務操作的邊緣）有時是可以接受的。這種方法在業務操作自然遵循「讀取－決策－執行」結構時效果最佳，該結構有三個明確的階段：

- Retrieving data from storage
- Executing business logic
- Persisting data back to the storage

- 從存儲中檢索數據
- 執行業務邏輯
- 將數據持久化回存儲

![There’s no single solution that satisfies all three attributes: domain model completeness, domain model purity, and performance. You have to choose two out of the three.](https://enterprisecraftsmanship.com/images/2020/2020-08-06-read-decide-act.png)

This is what we had in the initial version of our user management system, before we introduced the check for email uniqueness.

這正是我們在用戶管理系統初始版本中的做法，也就是引入電子郵件唯一性檢查之前的版本。

There are a lot of situations where these stages aren’t as clearcut, though. You might need to query additional data from an out-of-process dependency based on an intermediate result of the decision-making process. Writing to the out-of-process dependency often depends on that result, too.

然而，在許多情況下，這些階段並不那麼清晰。你可能需要根據決策過程的中間結果，從進程外部依賴中查詢額外的數據。而且，是否需要將數據寫回進程外部依賴，通常也取決於這個結果。

![But more often than not, you need to refer to out-of-process dependencies in the middle of the business operation.](https://enterprisecraftsmanship.com/images/2020/2020-08-06-read-decide-act2.png)

Or, like in our case, you might not be able to query the data necessary to make a decision in the domain model at all, because that’s simply not practical. Because of that, the first approach is usually out of the question.

或者像我們的例子中，你可能根本無法在領域模型中查詢作出決策所需的數據，因為這並不實際。由於這些原因，第一種方法通常不可行。

The decision, then, comes down to the choice between the second (injecting out-of-process dependencies into the domain model) and the third (splitting the decision-making process between the domain layer and controllers) options.

因此，決策最終落在第二種方法（將進程外部依賴注入到領域模型中）與第三種方法（將決策過程分散在領域層和控制器之間）之間。

Which one is better?
哪種方法更好？

I strongly recommend that you choose domain model purity over domain model completeness, and go with the third approach: splitting the decision-making process between the domain layer and controllers. Domain logic fragmentation is a lesser evil than merging the responsibilities of domain modeling and communication with out-of-process dependencies.

我強烈建議選擇領域模型純粹性，而非完整性，並採用第三種方法：將決策過程分散在領域層和控制器之間。領域邏輯分散的影響要比將領域建模與進程外部依賴的交互職責合併起來小得多。

Business logic is the most important part of the application. It’s also the most complex part of it. Mixing it with the additional responsibility of talking to out-of-process dependencies makes that logic’s complexity grow even bigger. Avoid this as much as possible. The domain layer should be exempted from all responsibilities other than the domain logic itself.

業務邏輯是應用程式中最重要的部分，也是最複雜的部分。如果將其與處理進程外部依賴的額外職責混在一起，邏輯的複雜性會進一步增加。應盡可能避免這種情況。領域層應專注於領域邏輯本身，避免承擔其他責任。

Splitting the decision-making process between the domain layer and controllers is the approach which Functional Programming, Unit Testing, and (arguably) Domain-Driven Design all converge to, albeit for different reasons.

將決策過程分散在領域層和控制器之間，這種方法在函數式編程、單元測試以及（或許）領域驅動設計（DDD）中都被認可，雖然各自的原因不同：

- DDD advocates for this approach because it helps keep the application’s complexity manageable. As you know, DDD is all about [Tackling Complexity in the Heart of Software](https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215), where "heart" means the domain model.
- **DDD** 支持這種方法，因為它有助於保持應用程式的複雜性在可控範圍內。如你所知，DDD 的核心是[解決軟體核心的複雜性](https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215)，這裡的「核心」指的是領域模型。
- Functional Programming chooses this approach because it’s the only way to make your functions pure. Functional Programming is all about referential transparency and the avoidance of hidden inputs and outputs in [the functional core of your application](https://enterprisecraftsmanship.com/posts/immutable-architecture/) (querying the database, aka database I/O, is one of such hidden inputs).
- **函數式編程** 選擇這種方法，因為這是實現函數純粹性的唯一方法。函數式編程關注引用透明性，以及避免[應用程式的函數核心](https://enterprisecraftsmanship.com/posts/immutable-architecture/)中隱藏的輸入和輸出（如數據庫 I/O）。
- [Unit Testing advocates for it](https://enterprisecraftsmanship.com/book-amazon) because pure domain model means testable domain model. Without the separation between business logic and communication with out-of-process dependencies, your tests will be much harder to maintain as you will have to setup mocks and stubs, and then check interactions with them.
- **單元測試** 支持這種方法，因為純粹的領域模型意味著更易於測試的領域模型。如果業務邏輯與進程外部依賴的通信沒有分離，測試的維護難度將增加，因為你需要設置 mock 和 stub，並檢查它們的交互。

In our sample project, splitting the decision-making process between the domain layer and controllers means putting the the email uniqueness check into the `UserController` instead of the `User` class.

在我們的示例專案中，將決策過程分散在領域層和控制器之間，意味著將電子郵件唯一性檢查放到 `UserController` 中，而不是放在 `User` 類別內。

## UPDATE

As C. Shea mentioned in the comments, this trilemma can be called a CAP theorem for Domain Modeling. Love the analogy.

正如 C. Shea 在評論中提到的，這個三難困境可以被稱為領域建模的 CAP 定理（CAP theorem for Domain Modeling）。這個類比很貼切！
## Summary

- Domain model completeness is when your domain model contains all the application’s domain logic.
    - Domain logic fragmentation is the opposite of that — it’s when the domain logic resides in layers other than the domain layer.
- **領域模型的完整性**：當你的領域模型包含應用程式的所有領域邏輯時。
	- **領域邏輯分散**則與之相反，指的是領域邏輯存在於領域層以外的其他層中。
- Domain model purity is when your domain model doesn’t reach out to out-of-process dependencies.
- **領域模型的純粹性**：當你的領域模型不依賴進程外部依賴時。
- In most use cases, you can’t have all 3 of the following attributes:
    - Domain model completeness
    - Domain model purity
    - Performance
- 在大多數使用案例中，你無法同時擁有以下三個屬性：
	- 領域模型的完整性
	- 領域模型的純粹性
	- 效能
- There are three common approaches, but each of them only gives you 2 out of the 3 attributes:
    - Pushing all external reads and writes to the edges of a business operation — Preserves domain model completeness and purity but concedes performance.
    - Injecting out-of-process dependencies into the domain model — Keeps performance and domain model completeness, but at the expense of domain model purity.
    - Splitting the decision-making process between the domain layer and controllers — Helps with both performance and domain model purity but concedes completeness.
- 三種常見的方法中，每種方法只能實現其中兩個屬性：
	- **將所有外部讀取和寫入推到業務操作的邊緣**——保留領域模型的完整性和純粹性，但犧牲效能。
	- **將進程外部依賴注入到領域模型中**——保留效能和領域模型的完整性，但犧牲純粹性。
	- **將決策過程分散在領域層和控制器之間**——保留效能和領域模型的純粹性，但犧牲完整性。
- If you can push all external reads and writes to the edges of a business operation without much damage to application performance, choose this option.
- 如果能夠將所有外部讀取和寫入操作推到業務操作的邊緣，而不會對應用程式效能造成太大影響，請選擇這種方法。
- Otherwise, choose domain model purity over completeness.
- 否則，選擇領域模型的純粹性優先於完整性。

## Other articles in the series
- **Domain model purity vs. domain model completeness (this article)**
- [Domain model purity and the current time](https://enterprisecraftsmanship.com/posts/domain-model-purity-current-time/)
- [Domain model purity and lazy loading](https://enterprisecraftsmanship.com/posts/domain-model-purity-lazy-loading/)
    

## Related
- [How to know if your Domain model is properly isolated?](https://enterprisecraftsmanship.com/posts/how-to-know-if-your-domain-model-is-properly-isolated/)
- [Immutable architecture](https://enterprisecraftsmanship.com/posts/immutable-architecture/)