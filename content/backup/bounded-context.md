---
title: "Bounded Context"
date: '2025-02-07T15:33:58+08:00'
tags: ["Martin Fowler"]
---

[Source](https://martinfowler.com/bliki/BoundedContext.html)

Bounded Context is a central pattern in Domain-Driven Design. It is the focus of DDD's strategic design section which is all about dealing with large models and teams. DDD deals with large models by dividing them into different Bounded Contexts and being explicit about their interrelationships.

限界上下文（Bounded Context）是領域驅動設計（Domain-Driven Design, DDD）的核心模式。它是 DDD 戰略設計部分的焦點，主要處理大型模型和團隊。DDD 通過將大型模型劃分為不同的限界上下文，並明確定義它們之間的相互關係，來管理這些龐大的模型。

![](https://martinfowler.com/bliki/images/boundedContext/sketch.png)

DDD is about designing software based on models of the underlying domain. A model acts as a [UbiquitousLanguage](https://martinfowler.com/bliki/UbiquitousLanguage.html) to help communication between software developers and domain experts. It also acts as the conceptual foundation for the design of the software itself - how it's broken down into objects or functions. To be effective, a model needs to be unified - that is to be internally consistent so that there are no contradictions within it.

DDD 是基於底層領域模型來設計軟體。一個模型充當了通用語言（Ubiquitous Language），幫助軟體開發者和領域專家之間進行溝通。它同時也作為軟體設計本身的概念基礎 - 即如何將軟體分解為物件或函式。為了有效，模型需要保持統一 - 也就是在內部保持一致，確保模型內部沒有矛盾。

As you try to model a larger domain, it gets progressively harder to build a single unified model. Different groups of people will use subtly different vocabularies in different parts of a large organization. The precision of modeling rapidly runs into this, often leading to a lot of confusion. Typically this confusion focuses on the central concepts of the domain. Early in my career I worked with a electricity utility - here the word “meter” meant subtly different things to different parts of the organization: was it the connection between the grid and a location, the grid and a customer, the physical meter itself (which could be replaced if faulty). These subtle [polysemes](http://en.wikipedia.org/wiki/Polysemy) could be smoothed over in conversation but not in the precise world of computers. Time and time again I see this confusion recur with polysemes like “Customer” and “Product”.

隨著您嘗試對更大的領域進行建模，建立一個統一的模型變得越來越困難。在大型組織的不同部門中，人們會使用微妙不同的詞彙。建模的精確性很快就會遇到這種情況，常常導致大量混淆。通常這種混淆集中在領域的核心概念上。在我職業生涯的早期，我曾與一家電力公司合作 - 在那裡，"電表"一詞在組織的不同部門中有微妙的不同含義：它是指電網和位置之間的連接，還是電網和客戶之間的連接，又或是實體電表本身（可能會在故障時被更換）。這些微妙的多義詞在交談中可以被平滑處理，但在電腦的精確世界中卻無法如此。我一再看到這種混淆重複出現，尤其是像"客戶"和"產品"這樣的多義詞。

In those younger days we were advised to build a unified model of the entire business, but DDD recognizes that we've learned that “total unification of the domain model for a large system will not be feasible or cost-effective” 1. So instead DDD divides up a large system into Bounded Contexts, each of which can have a unified model - essentially a way of structuring [MultipleCanonicalModels](https://martinfowler.com/bliki/MultipleCanonicalModels.html).

在早期，我們曾被建議為整個業務建立一個統一的模型，但 DDD 認識到我們已經明白「對於大型系統，對整個領域模型進行全面統一是不可行或不符合成本效益的」。因此，DDD 將大型系統劃分為限界上下文，每個上下文都可以有一個統一的模型 - 本質上是一種構建多重標準模型的方法。

Bounded Contexts have both unrelated concepts (such as a support ticket only existing in a customer support context) but also share concepts (such as products and customers). Different contexts may have completely different models of common concepts with mechanisms to map between these polysemic concepts for integration. Several DDD patterns explore alternative relationships between contexts.

限界上下文既包含不相關的概念（例如支持工單僅存在於客戶支持上下文中），也包含共享的概念（如產品和客戶）。不同的上下文可能對共同概念有完全不同的模型，並通過機制在這些多義概念之間進行映射以實現整合。多個 DDD 模式探索了上下文之間的各種替代關係。

Various factors draw boundaries between contexts. Usually the dominant one is human culture, since models act as Ubiquitous Language, you need a different model when the language changes. You also find multiple contexts within the same domain context, such as the separation between in-memory and relational database models in a single application. This boundary is set by the different way we represent models.

各種因素會在不同的情境之間劃分界限。通常，主導因素是人類文化。由於模型充當了通用語言（Ubiquitous Language）的角色，因此當語言發生變化時，就需要使用不同的模型。在相同的領域情境中，你也會發現多個不同的子情境，例如在同一個應用程式中，內存模型和關聯式資料庫模型之間的分隔。這種界限是由我們表示模型的方式不同而決定的。

DDD's strategic design goes on to describe a variety of ways that you have relationships between Bounded Contexts. It's usually worthwhile to depict these using a context map.

DDD 的策略設計進一步描述了有界上下文（Bounded Context）之間各種關係的方式。通常，使用上下文地圖（Context Map）來呈現這些關係是值得的。