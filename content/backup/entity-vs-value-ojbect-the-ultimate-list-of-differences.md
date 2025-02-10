---
date: '2025-02-10T08:46:31+08:00'
title: 'Entity vs Value Object - the ultimate list of differences'
tags: ["Vladimir Khorikov"]
---

[Source](https://enterprisecraftsmanship.com/posts/entity-vs-value-object-the-ultimate-list-of-differences/)

I wrote about [entities](https://enterprisecraftsmanship.com/2014/11/08/domain-object-base-class/) and [value objects](https://enterprisecraftsmanship.com/2015/01/03/value-objects-explained/) some time ago. In this post, I’d like to talk about differences between Entity vs Value Object in more detail.

我之前寫過有關[實體 (Entity)](https://enterprisecraftsmanship.com/2014/11/08/domain-object-base-class/)和[值物件 (Value Object)](https://enterprisecraftsmanship.com/2015/01/03/value-objects-explained/)的文章。在這篇文章中，我想更詳細地探討實體與值物件之間的差異。

I know, the topic isn’t new and there are a lot of articles on the Internet discussing it already. Nevertheless, I didn’t find any with an exhaustive, comprehensive description, so I decided to create my own.

我知道，這個主題並不新穎，網路上已有許多相關討論的文章。儘管如此，我並未找到任何內容詳盡且全面的描述，因此我決定自己撰寫一篇。

## 1. Entity vs Value Object: types of equality

To define the differences between entities and value objects, we need to introduce three types of equality which come into play when we need to compare objects to each other.

為了定義實體與值物件之間的差異，我們需要介紹三種類型的「相等性」，這些在我們需要比較物件時會派上用場。

**Reference equality** means that two objects are deemed to be equal if they reference the same address in the memory:

**引用相等性 (Reference equality)** 指的是兩個物件在記憶體中引用相同的地址時，便被視為相等：

![Reference equality](https://enterprisecraftsmanship.com/images/2016/2016-01-11-1.png)

Here’s how we can check that in code:

```csharp
object object1 = new object();
object object2 = object1;
bool areEqual = object.ReferenceEquals(object1, object2); // returns true
```

**Identifier equality** implies a class has an id field. Two instances of such a class would be equal if they have the same identifiers:

**識別相等性 (Identifier equality)** 意味著一個類別具有一個識別欄位 (id)。當該類別的兩個實例具有相同的識別符時，便視為相等：

![Identifier equality](https://enterprisecraftsmanship.com/images/2016/2016-01-11-2.png)

And finally, with **structural equality**, we consider two objects equal if all of their members match:

最後，**結構相等性 (Structural equality)** 是指當兩個物件的所有成員都相符時，它們被視為相等：

![Structural equality](https://enterprisecraftsmanship.com/images/2016/2016-01-11-3.png)

The main difference between entities and value objects lies in the way we compare their instances to each other. **The concept of identifier equality refers to entities, whereas the concept of structural equality - to value objects**. In other words, entities possess inherent identity while value objects don’t.

實體和值物件之間的主要差異在於我們比較其實例的方式。**識別相等性 (Identifier equality) 的概念適用於實體，而結構相等性 (Structural equality) 的概念則適用於值物件**。換句話說，實體具有內在的身份，而值物件則沒有。

In practice it means that value objects don’t have an identifier field and if two value objects have the same set of attributes we can treat them interchangeably. At the same time, if data in two entity instances is the same (except for the Id property), we don’t deem them as equivalent.

在實務中，這意味著值物件沒有識別欄位，如果兩個值物件具有相同的屬性集合，我們可以將它們視為可互換的。同時，如果兩個實體實例中的資料相同（除了 Id 屬性以外），我們並不認為它們是等同的。

You can think of it in a similar way you would think of two people bearing the same name. You don’t treat them as the same person because of that. Both of the people have their own inherent identity. Yet, if a person has a 1 dollar bill, they don’t care if this physical piece of paper is the same as they had yesterday. Until it’s still 1 dollar, they are fine with replacing this note with another one. The concept of money in such case would be a value object.

你可以將其類比為兩個擁有相同名字的人。你不會因為名字相同就認為他們是同一個人，因為每個人都有自己內在的身份。然而，如果某人擁有一張1美元鈔票，他並不在意這張紙是否與他昨天持有的是同一張。只要它仍然是1美元，他就願意用另一張替換它。在這種情況下，金錢的概念就是一個值物件。

## 2. Entity vs Value Object: lifespan

Another distinction between the two notions is the lifespan of their instances. Entities _live in continuum_, so to speak. They have a history (even if we don’t store it) of what happened to them and how they changed during their lifetime.

這兩個概念之間的另一個區別是其實例的生命周期。實體可以說是「持續存在」的，它們有一個歷史（即使我們沒有將其存儲），記錄著它們在生命週期中發生的變化和經歷的事件。

Value objects, at the same time, have a zero lifespan. We create and destroy them with ease. That’s a corollary of being interchangeable. If this 1 dollar bill is the same as another one, why bother? We can just replace the existing object with the one we just instantiated and forget about it altogether.

值物件同時具有零生命週期。我們可以輕易地創建和銷毀它們。這是可互換性的必然結果。如果這1塊錢的鈔票和另一張完全相同，為什麼還要費心？我們可以直接用新實例化的物件替換現有物件，然後完全忘記它。

A guideline that flows from this distinction is that value objects cannot live by their own, they should always belong to one or several entities. The data a value object represents has a meaning only in the context of an entity it refers to. In the example above with people and money, the question "How much money?" doesn’t make any sense because it doesn’t convey proper context. At the same time, questions "How much money Peter has?" or "How much money all our users possess?" are perfectly valid.

從這種區別中得出的一個準則是，值物件（Value objects）不能獨立存在，它們必須隸屬於一個或多個實體（entities）。值物件所代表的資料，只有在與其相關的實體上下文中才具有意義。以人和金錢的例子來說，"有多少錢？"這個問題本身是毫無意義的，因為它缺乏適當的脈絡。與此同時，"彼得有多少錢？"或"我們所有用戶總共擁有多少錢？"這些問題都是完全合理且有效的。

Another corollary here is that we don’t store value objects separately. The only way for us to persist a value object is to attach it to an entity (more about it in a minute).

另一個由此衍生的結論是，我們不會單獨儲存值物件（Value objects）。對我們來說，持久化值物件的唯一方法是將它附加到一個實體（entities）上（稍後會詳細介紹）。

## 3. Entity vs Value Object: immutability

The next difference is immutability. **Value objects should be immutable** in a sense that if we need to change such an object, we construct a new instance based on the existing object rather than changing it. On the contrary, entities are almost always mutable.

下一個區別是不可變性。**值物件應該是不可變的**，意味著當我們需要改變這樣的物件時，是基於現有物件構建一個新實例，而不是直接修改它。相比之下，實體（entities）幾乎總是可變的。

The question whether or not value objects should always be immutable is a subject of a dispute. Some programmers argue that this rule is not as strict as the previous one, and value objects can indeed be mutable in some cases. I used to adhere to this point of view as well.

值物件是否應該始終保持不可變，這個問題是存在爭議的。一些程式設計師認為，這個規則並不像前面的規則那麼嚴格，在某些情況下值物件確實可以是可變的。我過去也堅持這種觀點。

Nowadays, I find the connection between immutability and the ability to replace a value object with another one is deeper that I thought. By mutating an instance of a value object, you assume it has its own life cycle. And that assumption, in turn, leads to a conclusion that the value object has its own inherent identity, which contradicts the definition of that DDD notion.

如今，我發現不可變性與替換值物件的能力之間的聯繫，比我過去想像的更加深刻。通過改變值物件的實例，你實際上默認它具有自己的生命週期。而這種假設反過來又導致一個結論：值物件具有自身固有的身份，這與領域驅動設計（DDD）中對值物件的定義是相互矛盾的。

This simple mental exercise makes immutability an intrinsic part of Value Object. If we accept value objects have a zero lifetime, meaning that they are just snapshots of some state and nothing more, then we have to admit they are allowed to represent only a single variant of that state.

這個簡單的思維練習使不可變性成為值物件的內在特性。如果我們接受值物件具有零生命週期，意味著它們只是某種狀態的快照，別無其他，那麼我們必須承認它們只能呈現該狀態的單一變體。

That leads us to the following rule of thumb: **if you can’t make a value object immutable, then it is not a value object**.

這引導我們得出以下經驗法則：**如果你無法讓一個值物件保持不可變，那麼它就不是一個值物件**。
## 4. How to recognize a value object in your domain model?

It’s not always clear if a concept in your domain model is an entity or a value object. And unfortunately, there are no objective attributes you could use to get to know it. Whether or not a notion is a value object fully depends on the problem domain: a concept can be an entity in one domain model and a value object in another.

在你的領域模型中，一個概念是實體（entity）還是值物件（value object）並不總是那麼明確。不幸的是，並沒有客觀的屬性可以用來判斷它。一個概念是否為值物件完全取決於問題域：在一個領域模型中是實體的概念，在另一個領域模型中可能就是值物件。

In the example above, we treat money interchangeably, which makes this concept a value object. At the same time, if we build a software for tracking cash flow in the entire country, we would need to treat every single bill separately to gather statistics for each of them. In this case, the notion of money would be an entity, although we would probably name it Note or Bill.

在上面的例子中，我們可以互換地看待金錢，這使得這個概念成為一個值物件。同時，如果我們要建立一個追蹤整個國家現金流動的軟體，我們就需要單獨追蹤每一張鈔票，以便為每張鈔票收集統計數據。在這種情況下，金錢的概念將成為一個實體，儘管我們可能會將其命名為「鈔票」（Note）或「票據」（Bill）。

Despite the lack of objective traits, you can still employ some technique in order to attribute a concept to either entities or value objects. We discussed the notion of identity: if you can safely replace an instance of a class with another one which has the same set of attributes, that’s a good sign this concept is a value object.

儘管缺乏客觀特徵，你仍然可以採用一些技巧來判斷一個概念是實體（entities）還是值物件（value objects）。我們已經討論過身份的概念：如果你可以安全地用另一個具有相同屬性集的實例替換某個類別的實例，這是一個很好的跡象，表明這個概念是一個值物件。

A simpler version of that technique is to compare a value object to an integer. Do you really care if the integer 5 is the same 5 that you used in another method? Definitely not, all fives in your application are the same regardless of how they were instantiated. That makes an integer essentially a value object. Now, ask yourself, is this notion in your domain looks like integer? If the answer is yes, then it’s a value object.

這個技巧的簡化版本是將值物件與整數進行比較。你真的在意在另一個方法中使用的5是不是同一個5嗎？當然不會，你的應用程式中所有的5都是一樣的，不管它們是如何被實例化的。這使得整數本質上是一個值物件。現在，問問自己，你領域中的這個概念是否看起來像整數？如果答案是肯定的，那麼它就是一個值物件。
## 5. How to store value objects in the database?

Let’s say we have two classes in our domain model: Person entity and Address value object:

讓我們假設在我們的領域模型中有兩個類別：人（Person）實體和地址（Address）值物件：

```csharp
// Entity
public class Person
{
    public int Id { get; set; }
    public string Name { get; set; }
    public Address Address { get; set; }
}
 
// Value Object
public class Address
{
    public string City { get; set; }
    public string ZipCode { get; set; }
}
```

How would the database structure look like in this case? One option that comes to mind is to create separate tables for each of them, like this:

在這種情況下，資料庫的結構會是什麼樣子呢？一個想到的選項是為每個對象建立單獨的表，如下所示：

![Storing the value object in a separate table](https://enterprisecraftsmanship.com/images/2016/2016-01-11-4.png)

Such design, albeit being perfectly valid from the database standpoint, has two major drawbacks. First of all, the Address table contains an identifier. It means that we will have to introduce a separate Id field in the Address value object to work with such table correctly. This, in turn, means we are providing the Address class with some identity. And that violates the definition of Value Object.

這樣的設計，雖然在資料庫的角度看是完全有效的，但有兩個主要缺點。首先，`Address` 表包含一個識別欄位 (Id)。這意味著我們必須在 `Address` 值物件中引入一個單獨的 Id 欄位，才能正確地處理這樣的表。這反過來表示我們為 `Address` 類別提供了一些身份，而這違反了值物件的定義。

The other drawback is that with this solution, we can potentially detach value objects from entities. The Address value object can now live by its own because we are able to delete a Person row from the database without deleting the corresponding Address row. That would violate another rule which states that the lifetime of value objects should fully depend on the lifetime of their parent entities.

另一個缺點是，採用這種解決方案可能導致值物件與實體分離。`Address` 值物件現在可以獨立存在，因為我們可以從資料庫中刪除一個 `Person` 的列，而不刪除對應的 `Address` 列。這將違反另一條規則，即值物件的生命周期應完全依賴於其父實體的生命周期。

It turns out that the best solution is to inline the fields from the Address table into the Person table, like this:

事實證明，最好的解決方案是將 `Address` 表中的欄位內嵌到 `Person` 表中，如下所示：

![Inlining the value object](https://enterprisecraftsmanship.com/images/2016/2016-01-11-5.png)

This would solve all the problems I stated earlier: Address doesn’t have an identity anymore and its lifetime now fully depends on the lifetime of the Person entity.

這將解決我之前提到的所有問題：`Address` 不再具有身份，其生命周期現在完全依賴於 `Person` 實體的生命周期。

This design also makes sense if you mentally replace the fields that regard to Address with a single integer as I suggested previously. Do you create a separate table for an integer? Of course not, you just inline that integer to the table you want it to be in. The same applies to the value objects. **Don’t introduce separate tables for value objects**, just inline them into the parent entity’s table.

如果你在心中將與 `Address` 有關的欄位替換為一個單一的整數（如我先前建議的），這種設計也很合理。你會為一個整數建立一個單獨的表嗎？當然不會，你只會將該整數內嵌到你想要的表中。同樣的道理也適用於值物件。**不要為值物件建立單獨的表**，而是將它們內嵌到父實體的表中。

## 6. Prefer value objects over entities

When it comes to working with entities and value objects, an important guideline comes into play: always prefer value objects over entities. Value objects are immutable and more lightweight than entities. Because of that, they are extremely easy to work with. Ideally, you should always put most of the business logic into value objects. Entities in this situation would act as wrappers upon them and represent more high-level functionality.

在處理實體和值物件時，一個重要的指導原則是：**永遠優先選擇值物件而非實體**。值物件是不可變的，且比實體更輕量化。因此，它們非常易於使用。理想情況下，你應該將大部分的業務邏輯放入值物件中。實體在這種情況下則充當值物件的包裝器，並代表更高層次的功能性。

Also, it might be that a concept you saw as an entity at first essentially is a value object. For example, the Address class in your code base could be introduced as an entity initially. It may have its own Id field and a separate table in the database. After revisiting it, you might notice that in your domain, addresses don’t actually have their own inherent identity and can be used interchangeably. In this case, don’t hesitate to refactor your domain model and convert the entity into a value object.

此外，最初你可能認為某個概念是實體，但實際上它本質上是一個值物件。例如，你的程式碼中可能最初將 `Address` 類別引入為實體。它可能有自己的 Id 欄位和資料庫中的獨立表。但在重新檢視後，你可能會發現，在你的領域中，地址實際上並沒有自己的內在身份，而且是可以互換使用的。在這種情況下，不要猶豫，重構你的領域模型，將該實體轉換為值物件。

## 7. Summary

Alright, I think I covered every aspect that regards to the topic of Entity vs Value Object. Let’s summarize it with the following:

好的，我認為已經涵蓋了有關實體與值物件主題的各個方面。我們用以下內容來總結：

- Entities have their own intrinsic identity, value objects don’t.
- The notion of identity equality refers to entities; the notion of structural equality refers to value objects; the notion of reference equality refers to both.
- Entities have a history; value objects have a zero lifespan.
- A value object should always belong to one or several entities, it can’t live by its own.
- Value objects should be immutable; entities are almost always mutable.
- To recognize a value object in your domain model, mentally replace it with an integer.
- Value objects shouldn’t have their own tables in the database.
- Always prefer value objects over entities in your domain model.

- 實體具有自身的內在身份，值物件則沒有。
- 識別相等性 (Identity Equality) 的概念適用於實體；結構相等性 (Structural Equality) 的概念適用於值物件；引用相等性 (Reference Equality) 的概念適用於兩者。
- 實體具有歷史；值物件的生命周期為零。
- 值物件應始終隸屬於一個或多個實體，不能獨立存在。
- 值物件應為不可變的；實體幾乎總是可變的。
- 在你的領域模型中，若要辨識一個值物件，可以在心中將其替換為一個整數。
- 值物件不應該在資料庫中擁有自己的表格。
- 在你的領域模型中，應始終優先選擇值物件而非實體。

## 8. Related articles:

- [Entity base class](https://enterprisecraftsmanship.com/2014/11/08/domain-object-base-class/)
- [Value object base class](https://enterprisecraftsmanship.com/2015/01/03/value-objects-explained/)
- [Domain-Driven Design in Practice course on Pluralsight](https://enterprisecraftsmanship.com/ps-ddd-in-practice)
- [Representing a collection as a Value Object](https://enterprisecraftsmanship.com/2016/08/04/representing-a-collection-as-a-value-object/)
- [Nesting a Value Object inside an Entity](https://enterprisecraftsmanship.com/2016/08/09/nesting-a-value-object-inside-an-entity/)