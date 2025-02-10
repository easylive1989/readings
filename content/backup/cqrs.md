---
date: '2025-02-08T21:36:02+08:00'
title: 'CQRS'
tags: ["Martin Fowler"]
---

[Source](https://martinfowler.com/bliki/CQRS.html)

CQRS stands for **Command Query Responsibility Segregation**. It's a pattern that I first heard described by [Greg Young](https://twitter.com/gregyoung). At its heart is the notion that you can use a different model to update information than the model you use to read information. For some situations, this separation can be valuable, but beware that for most systems CQRS adds risky complexity.

CQRS 代表命令查詢責任分離（Command Query Responsibility Segregation）。這是一種由 Greg Young 首次描述的設計模式。其核心理念是，更新資料時可以使用一種模型，而讀取資料時可以使用另一種模型。在某些情況下，這種分離可能非常有價值，但要注意，對於大多數系統而言，CQRS 可能會增加不必要的複雜性和風險。

The mainstream approach people use for interacting with an information system is to treat it as a CRUD datastore. By this I mean that we have mental model of some record structure where we can **c**reate new records, **r**ead records, **u**pdate existing records, and **d**elete records when we're done with them. In the simplest case, our interactions are all about storing and retrieving these records.

主流的方法是將資訊系統視為一個 CRUD 資料存儲（CRUD datastore）來進行互動。也就是說，我們會有一種關於記錄結構的心智模型，可以創建新記錄、讀取記錄、更新現有記錄，並在不需要時刪除記錄。在最簡單的情況下，我們的互動僅僅是圍繞著存儲和檢索這些記錄。

As our needs become more sophisticated we steadily move away from that model. We may want to look at the information in a different way to the record store, perhaps collapsing multiple records into one, or forming virtual records by combining information for different places. On the update side we may find validation rules that only allow certain combinations of data to be stored, or may even infer data to be stored that's different from that we provide.

隨著需求變得更加複雜，我們會逐漸脫離這種模型。我們可能希望以不同於記錄存儲的方式來查看資訊，例如將多個記錄合併為一個，或通過結合來自不同地方的資訊形成虛擬記錄。在更新方面，我們可能會發現驗證規則只允許存儲某些特定的數據組合，甚至可能根據我們提供的數據推斷並存儲不同的數據。

![](https://martinfowler.com/bliki/images/cqrs/single-model.png)

As this occurs we begin to see multiple representations of information. When users interact with the information they use various presentations of this information, each of which is a different representation. Developers typically build their own conceptual model which they use to manipulate the core elements of the model. If you're using a Domain Model, then this is usually the conceptual representation of the domain. You typically also make the persistent storage as close to the conceptual model as you can.

隨著這種情況發生，我們開始看到資訊的多重表徵。當使用者與資訊互動時，他們會使用各種不同的呈現方式，而每種方式都是資訊的不同表徵。開發人員通常會建立自己的概念模型，用來操作模型的核心元素。如果您使用的是領域模型（Domain Model），那麼這通常是該領域的概念表徵。通常情況下，您也會讓持久化存儲盡可能接近概念模型。

This structure of multiple layers of representation can get quite complicated, but when people do this they still resolve it down to a single conceptual representation which acts as a conceptual integration point between all the presentations.

這種多層表徵的結構可能會變得相當複雜，但當人們這樣做時，他們仍然會將其簡化為一個單一的概念表徵，作為所有呈現方式之間的概念整合點。

The change that CQRS introduces is to split that conceptual model into separate models for update and display, which it refers to as Command and Query respectively following the vocabulary of [CommandQuerySeparation](https://martinfowler.com/bliki/CommandQuerySeparation.html). The rationale is that for many problems, particularly in more complicated domains, having the same conceptual model for commands and queries leads to a more complex model that does neither well.

CQRS 引入的變化是將該概念模型分為更新與顯示的獨立模型，分別稱為命令（Command）和查詢（Query），這名稱來源於命令查詢分離（Command Query Separation）的術語。其理由是，對於許多問題，尤其是在較為複雜的領域中，將命令和查詢共用同一個概念模型會導致模型更加複雜，且在兩方面都表現不佳。

![](https://martinfowler.com/bliki/images/cqrs/cqrs.png)

By separate models we most commonly mean different object models, probably running in different logical processes, perhaps on separate hardware. A web example would see a user looking at a web page that's rendered using the query model. If they initiate a change that change is routed to the separate command model for processing, the resulting change is communicated to the query model to render the updated state.

所謂的獨立模型，通常指的是不同的物件模型，可能運行在不同的邏輯流程中，甚至可能位於不同的硬體上。以網頁為例，使用者查看的網頁是透過查詢模型渲染的。如果他們發起變更，該變更會被路由到獨立的命令模型進行處理，處理後的結果再傳遞給查詢模型，以渲染更新後的狀態。

There's room for considerable variation here. The in-memory models may share the same database, in which case the database acts as the communication between the two models. However they may also use separate databases, effectively making the query-side's database into a real-time [ReportingDatabase](https://martinfowler.com/bliki/ReportingDatabase.html). In this case there needs to be some communication mechanism between the two models or their databases.

在這裡有相當大的變化空間。記憶體中的模型可能共享同一個資料庫，在這種情況下，資料庫充當兩個模型之間的通訊橋樑。然而，它們也可能使用獨立的資料庫，實際上將查詢端的資料庫變成一個即時的報告資料庫（Reporting Database）。在這種情況下，需要有某種通訊機制來連接這兩個模型或它們各自的資料庫。

The two models might not be separate object models, it could be that the same objects have different interfaces for their command side and their query side, rather like views in relational databases. But usually when I hear of CQRS, they are clearly separate models.

這兩個模型不一定是完全獨立的物件模型，可能是同一組物件針對命令端和查詢端提供了不同的介面，有點類似於關聯式資料庫中的視圖（views）。然而，通常當我聽到 CQRS 時，這些模型往往是明確獨立的模型。

CQRS naturally fits with some other architectural patterns.

- As we move away from a single representation that we interact with via CRUD, we can easily move to a task-based UI.
- CQRS fits well with [event-based programming models](https://martinfowler.com/eaaDev/EventNarrative.html). It's common to see CQRS system split into separate services communicating with [Event Collaboration](https://martinfowler.com/eaaDev/EventCollaboration.html). This allows these services to easily take advantage of [Event Sourcing](https://martinfowler.com/eaaDev/EventSourcing.html).
- Having separate models raises questions about how hard to keep those models consistent, which raises the likelihood of using [eventual consistency](http://www.allthingsdistributed.com/2008/12/eventually_consistent.html).
- For many domains, much of the logic is needed when you're updating, so it may make sense to use [EagerReadDerivation](https://martinfowler.com/bliki/EagerReadDerivation.html) to simplify your query-side models.
- If the write model generates events for all updates, you can structure read models as [EventPosters](https://martinfowler.com/bliki/EventPoster.html), allowing them to be [MemoryImages](https://martinfowler.com/bliki/MemoryImage.html) and thus avoiding a lot of database interactions.
- CQRS is suited to complex domains, the kind that also benefit from [Domain-Driven Design](https://www.amazon.com/gp/product/0321125215/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0321125215&linkCode=as2&tag=martinfowlerc-20).

CQRS 自然與其他一些架構模式相契合。

- 當我們從以 CRUD 操作的單一表徵模式轉移時，可以很容易地過渡到基於任務的使用者介面（Task-Based UI）。  
- CQRS 非常適合事件驅動的程式設計模型。CQRS 系統常被拆分為透過事件協作（Event Collaboration）進行通訊的獨立服務。這使得這些服務能夠輕鬆利用事件溯源（Event Sourcing）。  
- 使用獨立的模型會引發如何保持這些模型一致性的問題，這增加了採用最終一致性（Eventual Consistency）的可能性。  
- 對於許多領域而言，大部分邏輯集中於更新操作，因此採用「即時讀取派生」（Eager Read Derivation）來簡化查詢端模型可能是明智的選擇。  
- 如果寫入模型能為所有更新產生事件，那麼可以將讀取模型設計為事件接收器（Event Posters），使其成為記憶體快照（Memory Images），從而減少大量的資料庫交互。  
- CQRS 特別適合於複雜領域，這類領域也會從領域驅動設計（Domain-Driven Design）中受益。

## When to use it

Like any pattern, CQRS is useful in some places, but not in others. Many systems do fit a CRUD mental model, and so should be done in that style. CQRS is a significant mental leap for all concerned, so shouldn't be tackled unless the benefit is worth the jump. While I have come across successful uses of CQRS, so far the majority of cases I've run into have not been so good, with CQRS seen as a significant force for getting a software system into serious difficulties.

和任何設計模式一樣，CQRS 在某些場合有其用處，但在其他場合則未必適合。許多系統確實符合 CRUD 的心智模型，因此應以該方式實現。CQRS 對於所有相關人員來說都是一個重大的心智轉變，因此只有在其帶來的益處值得這種改變時才應考慮採用。雖然我遇到過一些成功應用 CQRS 的案例，但到目前為止，我接觸的大多數案例效果並不理想，CQRS 被認為是導致軟體系統陷入嚴重困境的重要原因之一。

In particular CQRS should only be used on specific portions of a system (a [BoundedContext](https://martinfowler.com/bliki/BoundedContext.html) in DDD lingo) and not the system as a whole. In this way of thinking, each Bounded Context needs its own decisions on how it should be modeled.

特別要注意的是，CQRS應該只用於系統的特定部分（在領域驅動設計中稱為「限界上下文」），而不是整個系統。按照這種思維方式，每個限界上下文都需要自行決定其建模方式。

So far I see benefits in two directions. Firstly is that a few complex domains may be easier to tackle by using CQRS. I must stress, however, that such suitability for CQRS is very much the minority case. Usually there's enough overlap between the command and query sides that sharing a model is easier. Using CQRS on a domain that doesn't match it will add complexity, thus reducing productivity and increasing risk.

到目前為止，我看到兩個方向的好處。第一是對於某些複雜的領域來說，使用CQRS可能會比較容易處理。然而，我必須強調，適合使用CQRS的情況是非常少數的。通常命令端和查詢端之間有足夠的重疊，使得共用一個模型會更容易。在不適合的領域使用CQRS會增加複雜度，因此會降低生產力並增加風險。

The other main benefit is in handling high performance applications. CQRS allows you to separate the load from reads and writes allowing you to scale each independently. If your application sees a big disparity between reads and writes this is very handy. Even without that, you can apply different optimization strategies to the two sides. An example of this is using different database access techniques for read and update.

另一個主要好處是在處理高性能應用程式時。CQRS允許您將讀取和寫入的負載分開，讓您能夠獨立地擴展兩者。如果您的應用程式在讀取和寫入之間存在很大的差異，這會非常有用。即使沒有這種差異，您也可以對兩端採用不同的優化策略。一個例子是對讀取和更新使用不同的資料庫存取技術。

If your domain isn't suited to CQRS, but you have demanding queries that add complexity or performance problems, remember that you can still use a [ReportingDatabase](https://martinfowler.com/bliki/ReportingDatabase.html). CQRS uses a separate model for all queries. With a reporting database you still use your main system for most queries, but offload the more demanding ones to the reporting database.

如果您的領域不適合使用CQRS，但您有一些會增加複雜度或造成效能問題的高需求查詢，請記住您仍然可以使用報表資料庫。CQRS對所有查詢都使用獨立的模型。而使用報表資料庫時，您仍然使用主系統處理大部分查詢，只將那些要求較高的查詢轉移到報表資料庫中。

Despite these benefits, **you should be very cautious about using CQRS**. Many information systems fit well with the notion of an information base that is updated in the same way that it's read, adding CQRS to such a system can add significant complexity. I've certainly seen cases where it's made a significant drag on productivity, adding an unwarranted amount of risk to the project, even in the hands of a capable team. So while CQRS is a pattern that's good to have in the toolbox, beware that it is difficult to use well and you can easily chop off important bits if you mishandle it.

儘管有這些好處，**您在使用CQRS時應該要非常謹慎**。許多資訊系統都很適合使用相同方式進行讀取和更新的資訊庫概念，在這樣的系統中加入CQRS可能會增加顯著的複雜度。我確實看過一些案例，即使是在有能力的團隊手中，它也明顯拖慢了生產力，為專案帶來了不必要的風險。因此，雖然CQRS是工具箱中很好的一個模式，但要注意它很難適當地使用，如果使用不當，您可能會輕易地搞砸重要的部分。

## Further Reading

- [Greg Young](http://codebetter.com/gregyoung/) was the first person I heard talking about this approach - this is [the summary from him](http://codebetter.com/gregyoung/2010/02/16/cqrs-task-based-uis-event-sourcing-agh/) that I like best.
- Udi Dahan is another advocate of CQRS, he has a [detailed description](http://www.udidahan.com/2009/12/09/clarified-cqrs/) of the technique.
- There is an [active mailing list](http://groups.google.com/group/dddcqrs) to discuss the approach.