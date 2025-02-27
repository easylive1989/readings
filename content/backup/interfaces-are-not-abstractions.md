---
date: '2025-02-28T02:12:27+08:00'
title: 'Interfaces are not abstractions'
tags: ["Mark Seemann"]
---
[Source](https://blog.ploeh.dk/2010/12/02/Interfacesarenotabstractions/)

One of the first sound bites from the beloved book [Design Patterns](http://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612/) is this:

《設計模式》（[Design Patterns](https://www.google.com/url?sa=E&q=http%3A%2F%2Fwww.amazon.com%2FDesign-Patterns-Elements-Reusable-Object-Oriented%2Fdp%2F0201633612%2F)）這本經典書籍中，其中一句常被引用的話是：

> _Program to an interface, not an implementation_
> 
> 針對介面編程，而不是針對實作編程

It would seem that a corollary is that we can measure the quality of our code on the number of interfaces; the more, the better. However, that's not how it feels in reality when you are trying to figure out whether to use an IFooFactory, IFooPolicy, IFooPolicyFactory or perhaps even an IFooFactoryFactory.

一個推論似乎是，我們可以根據介面的數量來衡量程式碼的品質；介面越多，品質越好。然而，當你試圖弄清楚該使用 IFooFactory、IFooPolicy、IFooPolicyFactory，還是甚至是 IFooFactoryFactory 時，現實並非如此。

Do you extract interfaces from your classes to enable loose coupling? If so, you probably have a [1:1 relationship between your interfaces and the concrete classes that implement them](http://martinfowler.com/bliki/InterfaceImplementationPair.html). That's [probably not a good sign](http://simpleprogrammer.com/2010/11/02/back-to-basics-what-is-an-interface/), and violates the [Reused Abstractions Principle (RAP)](http://parlezuml.com/blog/?postid=934). I've been guilty of this and didn't like the result.

你是否為了實現鬆散耦合而從類別中提取介面？ 如果是，你可能在介面和實作它們的具體類別之間存在 [1:1 的關係](https://www.google.com/url?sa=E&q=http%3A%2F%2Fmartinfowler.com%2Fbliki%2FInterfaceImplementationPair.html)。這 [可能不是一個好兆頭](https://www.google.com/url?sa=E&q=http%3A%2F%2Fsimpleprogrammer.com%2F2010%2F11%2F02%2Fback-to-basics-what-is-an-interface%2F)，並且違反了 [重用抽象原則 (RAP)](https://www.google.com/url?sa=E&q=http%3A%2F%2Fparlezuml.com%2Fblog%2F%3Fpostid%3D934)。 我曾經犯過這個錯誤，而且不喜歡結果。

> Having only one implementation of a given interface is a code smell.
> 
> 對於給定的介面只有一個實作是一種程式碼異味。

Programming to an interface does not guarantee that we are coding against an abstraction. Interfaces are not abstractions. Why not?

針對介面編程並不能保證我們是針對抽象編程。介面不是抽象。為什麼？

An interface is just a language construct. In essence, it's just a _shape_. It's like a power plug and socket. In Europe we use one kind, and the US uses another, but it's only _by convention_ that we transmit 230V through European sockets and 110V through US sockets. Although plugs only fit in their respective sockets, nothing prevents us from sending 230V through a US plug/socket combination.

介面只是一種語言構造。 本質上，它只是一種_形狀_。 它就像一個電源插頭和插座。 在歐洲我們使用一種，在美國使用另一種，但 按照慣例 我們通過歐洲插座傳輸 230V，通過美國插座傳輸 110V。 雖然插頭只能插入各自的插座，但沒有什麼能阻止我們通過美國插頭/插座組合發送 230V。

[Krzysztof Cwalina](http://blogs.msdn.com/b/kcwalina/) already pointed this out in 2004: [interfaces are not contracts](http://blogs.msdn.com/b/kcwalina/archive/2004/10/24/246947.aspx). If they aren't even contracts, then how can they be abstractions?

[Krzysztof Cwalina](https://www.google.com/url?sa=E&q=http%3A%2F%2Fblogs.msdn.com%2Fb%2Fkcwalina%2F) 早在 2004 年就指出：[介面不是合約](https://www.google.com/url?sa=E&q=http%3A%2F%2Fblogs.msdn.com%2Fb%2Fkcwalina%2Farchive%2F2004%2F10%2F24%2F246947.aspx)。 如果它們甚至不是合約，那麼它們怎麼可能是抽象呢？

Interfaces _can_ be used as abstractions, but using an interface is in itself no guarantee that we are dealing with an abstraction. Rather, we have the following relationship between interfaces and abstractions:

介面可以用作抽象，但使用介面本身並不能保證我們正在處理抽象。 相反，我們在介面和抽象之間有以下關係：

![Abstractions, interfaces and their intersection](https://blog.ploeh.dk/content/binary/Windows-Live-Writer/Interfaces-are-not-abstractions_D282/image_5.png "Abstraction and interface sets")

There are basically two sets: a set of abstractions and a set of interfaces. In the following we will discuss the set of interfaces that does _not_ intersect the set of abstractions, saving the intersection for [another blog post](https://blog.ploeh.dk/2010/12/03/Towardsbetterabstractions).

基本上有兩個集合：一個抽象的集合和一個介面的集合。 在下面，我們將討論不與抽象集合相交的介面集合，將交集留給 [另一篇博文](https://www.google.com/url?sa=E&q=https%3A%2F%2Fblog.ploeh.dk%2F2010%2F12%2F03%2FTowardsbetterabstractions)。

There are many ways an interface can turn out to be a poor abstraction. The following is an incomplete list:

有很多方法可以使介面變成糟糕的抽象。 以下是一個不完整的清單：

### LSP Violations

Violating the [Liskov Substitution Principle](http://en.wikipedia.org/wiki/Liskov_substitution_principle) is a pretty obvious sign that the interface in use is a poor abstraction. This may be most obvious when the consumer of the interface needs to downcast an instance to properly work with it.

違反 [Liskov 替換原則](https://www.google.com/url?sa=E&q=http%3A%2F%2Fen.wikipedia.org%2Fwiki%2FLiskov_substitution_principle) 是一個非常明顯的跡象，表明正在使用的介面是一個糟糕的抽象。 當介面的使用者需要向下轉型一個實例才能正常工作時，這可能最為明顯。

However, as [Uncle Bob](http://www.objectmentor.com/omTeam/martin_r.html) [points out](http://www.objectmentor.com/resources/articles/lsp.pdf), even an interface as simple as this seemingly innocuous rectangle ‘abstraction' contains potential dangers:

然而，正如 [Uncle Bob](https://www.google.com/url?sa=E&q=http%3A%2F%2Fwww.objectmentor.com%2FomTeam%2Fmartin_r.html) [指出](https://www.google.com/url?sa=E&q=http%3A%2F%2Fwww.objectmentor.com%2Fresources%2Farticles%2Flsp.pdf)，即使是一個像這個看似無害的矩形「抽象」一樣簡單的介面也包含潛在的危險：

```csharp
public interface IRectangle
{
    int Width { get; set; }
    int Height { get; set; }
}
```

The issue becomes apparent when you attempt to let a Square class implement IRectangle. To protect the invariants of Square, you can't allow the Width and Height properties to differ. You have a couple of options, none of which are very good:

當你嘗試讓 Square 類別實作 IRectangle 時，問題就變得明顯了。 為了保護 Square 的不變性，你不能允許 Width 和 Height 屬性不同。 你有幾個選擇，但沒有一個很好：

- Update both Width and Height to the same value when one of them are being written.
- Ignore the write operation when the caller attempts to assign an invalid value.
- Throw an exception when the caller attempts to assign a Width which is different from the Height (and vice versa).
  
- 當其中一個被寫入時，將 Width 和 Height 都更新為相同的值。
- 當呼叫者嘗試分配無效值時，忽略寫入操作。
- 當呼叫者嘗試分配一個與 Height 不同的 Width 時（反之亦然），拋出一個例外。

From the point of view of a consumer of the IRectangle interface, all of these options would at the very least violate the [Principle of Least Astonishment](http://en.wikipedia.org/wiki/Principle_of_least_astonishment), and throwing exceptions would definitely cause the consumer to behave differently when consuming Square instances as opposed to ‘normal' rectangles.

從 IRectangle 介面的使用者的角度來看，所有這些選項至少會違反 [最小驚訝原則](https://www.google.com/url?sa=E&q=http%3A%2F%2Fen.wikipedia.org%2Fwiki%2FPrinciple_of_least_astonishment)，並且拋出例外肯定會導致使用者在使用 Square 實例時與使用「正常」矩形時表現不同。

The problem stems from the fact that the operations have side effects. Invoking one operation changes the state of a seemingly unrelated piece of data. The more members we have, the greater the risk is, so the [Interface Segregation Principle](http://en.wikipedia.org/wiki/Interface_segregation_principle) can, to a certain extent, help.

問題源於操作具有副作用的事實。 呼叫一個操作會更改一個看似不相關的資料片段的狀態。 我們的成員越多，風險就越大，因此 [介面隔離原則](https://www.google.com/url?sa=E&q=http%3A%2F%2Fen.wikipedia.org%2Fwiki%2FInterface_segregation_principle) 可以在一定程度上提供幫助。

### Header Interfaces

Since a higher number of members increases the risk of unexpected side effects and temporal coupling it should come as no surprise that interfaces mechanically extracted from _all_ members of a concrete class are poor abstractions.

由於較多的成員會增加意外的副作用和時間耦合的風險，因此從具體類別的 所有 成員中機械地提取的介面是不良的抽象，這並不奇怪。

As always, Visual Studio makes it very easy to do the wrong thing by offering the _Extract Interface_ refactoring feature.

與往常一樣，Visual Studio 通過提供 提取介面 重構功能，使得做錯事變得非常容易。

We call such interfaces [Header Interfaces](http://martinfowler.com/bliki/HeaderInterface.html) because they resemble C++ header files. They tend to simply state the same thing twice without apparent benefit. This is particularly true when you have only a single implementation, which tends to be very likely for interfaces with many members.

我們將這些介面稱為 [標頭介面](https://www.google.com/url?sa=E&q=http%3A%2F%2Fmartinfowler.com%2Fbliki%2FHeaderInterface.html)，因為它們類似於 C++ 標頭檔案。 它們往往只是重複陳述同一件事，而沒有明顯的好處。 當你只有一個實作時，尤其如此，這對於具有許多成員的介面來說往往非常可能。

### Shallow Interfaces

When you use the _Extract Interface_ refactoring feature in Visual Studio, even if you don't extract every member, the resulting interface is _shallow_ because it doesn't recursively extract interfaces from the concrete types exposed by the extracted members.

當你在 Visual Studio 中使用 提取介面 重構功能時，即使你不提取每個成員，產生的介面也是淺層的，因為它不會從提取的成員公開的具體類型中遞迴地提取介面。

An example I've seen more than once involves extracting an interface from a LINQ to SQL or LINQ to Entities context in order to define a Repository interface. As an example, here's an interface extracted from a very simple LINQ to Entities context:

我多次看到的一個例子涉及從 LINQ to SQL 或 LINQ to Entities 上下文中提取介面，以便定義一個 Repository 介面。 作為一個例子，這是一個從一個非常簡單的 LINQ to Entities 上下文中提取的介面：

```csharp
public interface IPostingContext
{
    void AddToPostings(Posting posting);
    ObjectSet<Posting> Postings { get; }
}
```


At first glance this may look useful, but it isn't. Even though it's an interface, it's still tightly coupled to a specific object context. Not only does ObjectSet\<T> reference the Entity Framework, but the Posting class is defined by a very specific, auto-generated Entity context.

乍一看，這可能看起來很有用，但事實並非如此。 即使它是一個介面，它仍然與特定的物件上下文緊密耦合。 ObjectSet\<T> 不僅引用了 Entity Framework，而且 Posting 類別是由一個非常特定的、自動生成的 Entity 上下文定義的。

The interface may give you the impression of working against loosely coupled code, but you can't easily (if at all) implement a different IPostingContext with a radically different data access technology. You'll be stuck with this particular PostingContext.

該介面可能會給你一種針對鬆散耦合程式碼工作的印象，但是你無法輕易地（如果可以的話）用完全不同的資料存取技術實作一個不同的 IPostingContext。 你將被困在這個特定的 PostingContext 中。

If you must extract an interface, you'll need to do it recursively.

如果你必須提取一個介面，你需要遞迴地進行。

### Leaky Abstractions

Another way we can create problems for ourselves is when our interfaces leak implementation details. A good example can be found in the [SystemWrapper](http://systemwrapper.codeplex.com/) project that provides extracted interfaces for various BCL types, such as [System.IO.FileInfo](http://msdn.microsoft.com/en-us/library/system.io.fileinfo.aspx). Those interfaces may enable mocking, but we shouldn't expect to ever be able to create another implementation of SystemWrapper.IO.IFileInfoWrap. In other words, those interfaces aren't very useful.

我們可以為自己製造問題的另一種方式是，當我們的介面洩漏了實作細節時。[SystemWrapper](https://www.google.com/url?sa=E&q=http%3A%2F%2Fsystemwrapper.codeplex.com%2F) 專案提供各種 BCL 類型的提取介面，例如 [System.IO.FileInfo](https://www.google.com/url?sa=E&q=http%3A%2F%2Fmsdn.microsoft.com%2Fen-us%2Flibrary%2Fsystem.io.fileinfo.aspx)。 這些介面可能可以進行模擬，但我們不應期望能夠建立另一個 SystemWrapper.IO.IFileInfoWrap 的實作。 換句話說，這些介面不是很實用。

Another example is this attempt at defining a Repository interface:

另一個例子是定義一個 Repository 介面的嘗試：

```csharp
public interface IFooRepository
{
    string ConnectionString { get; set; }
    // ...
}
```

Exposing a ConnectionString property strongly indicates that the repository is implemented on top of a database; this knowledge leaks through. If we wanted to implement the repository based on a web service, we might be able to repurpose the ConnectionString property to a service URL, but it would be a hack at best - and how would we define security settings in that scenario?

公開 ConnectionString 屬性強烈表明該 repository 是在資料庫之上實作的； 這個知識洩漏了出來。 如果我們想基於 Web 服務實作該 repository，我們或許可以將 ConnectionString 屬性重新用於服務 URL，但這充其量只是一種變通方法 - 我們又該如何在這種情況下定義安全設定呢？

Exposing a _FileName_ property on an interface that represents an abstract resource is another example of a [Leaky Abstraction](http://en.wikipedia.org/wiki/Leaky_abstraction).

在表示抽象資源的介面上公開一個 FileName 屬性是另一個 [滲漏的抽象](https://www.google.com/url?sa=E&q=http%3A%2F%2Fen.wikipedia.org%2Fwiki%2FLeaky_abstraction) 的例子。

Leaky Abstractions like these are often difficult to reuse. As an example, it would be difficult to implement a [Composite](http://en.wikipedia.org/wiki/Composite_pattern) out of the above IFooRepository - how do you aggregate a ConnectionString?

像這樣的滲漏的抽象通常很難重用。 例如，很難從上面的 IFooRepository 中實作一個 [Composite](https://www.google.com/url?sa=E&q=http%3A%2F%2Fen.wikipedia.org%2Fwiki%2FComposite_pattern) - 你如何聚合一個 ConnectionString？

### Conclusion

In short, using interfaces in no way guarantees that we operate with appropriate abstractions. Thus, the proliferation of interfaces that typically follow from TDD or use of DI may not be the pure goodness we tend to believe.

簡而言之，使用介面並不能保證我們以適當的抽象運作。 因此，通常由 TDD 或 DI 的使用引起的介面擴散可能不是我們傾向於相信的純粹的優點。

Creating good abstractions is difficult and requires skill. In a [future post](https://blog.ploeh.dk/2010/12/03/Towardsbetterabstractions), I'll look at some principles that we can use as guides.

創建好的抽象很困難，需要技巧。 在 [未來的一篇文章](https://www.google.com/url?sa=E&q=https%3A%2F%2Fblog.ploeh.dk%2F2010%2F12%2F03%2FTowardsbetterabstractions) 中，我將探討一些可以作為指導原則的原則。