---
date: '2025-03-13T23:51:39+08:00'
title: 'Test Contra-variance'
tags: ["Uncle Bob"]
---

[Source](https://blog.cleancoder.com/uncle-bob/2017/10/03/TestContravariance.html)

Do you write unit tests? 
你寫單元測試嗎？

> _Yes, of course!_
> 是的，當然！

Do you write them first?
你會先寫單元測試嗎？

> _Yes, I follow [the three laws of TDD](http://www.softwaretestingmagazine.com/knowledge/the-three-rules-of-test-driven-development/)._
> 是的，我遵循[TDD 的三個法則](https://www.google.com/url?sa=E&q=http%3A%2F%2Fwww.softwaretestingmagazine.com%2Fknowledge%2Fthe-three-rules-of-test-driven-development%2F)。

What is the difference in module structure between your tests and your code?
你的測試和程式碼之間的模組結構有什麼不同？

> _I create one test class per production class._
> 我為每個生產類別建立一個測試類別。

So if you have a production class named `User` you will have a test class named `UserTest`?
所以如果你有一個名為 User 的生產類別，你就會有一個名為 UserTest 的測試類別？

> _Yes, almost always._
> 是的，幾乎總是這樣。

So the structure of your tests, and the structure of your code are _covariant_.
所以你的測試結構和程式碼結構是共變的。

> _Um. I suppose so, yes._
> 嗯，我想是的。

And so you are coupling the structure of your tests to the structure of your production code.
因此，你正在將測試的結構與生產程式碼的結構耦合。

> _I hadn’t thought about it being a coupling before; but, yes, I suppose I am._
> 我以前沒想過這是一種耦合；但是，是的，我想我是。

So when you refactor the class structure of your production code, without changing any behavior; do your tests all break?
所以當你重構生產程式碼的類別結構，而不改變任何行為時；你的所有測試都會失敗嗎？

> _Well, yes. Of course._
> 嗯，是的。當然。

And that means you can’t run your tests while you are refactoring, doesn’t it?
這意味著你在重構時無法運行你的測試，不是嗎？

> _Yes, yes, it does._
> 是的，是的，確實如此。

So then you can’t really call it refactoring, can you?
那麼你真的不能稱之為重構，對吧？

> _Why not?_
> 為什麼不能？

Because refactoring is defined as a sequence of small changes that keep the tests passing at all times.
因為重構被定義為一系列小的變更，這些變更始終保持測試通過。

> _Well, OK. I guess by that definition, the changes aren’t refactoring._
> 嗯，好吧。我想根據這個定義，這些變更不是重構。

Instead, you have to commit yourself to a big change and hope you can put everything back together again – including the tests.
相反，你必須投入到一個大的變更中，並希望你能將一切重新組合在一起——包括測試。

> _Yes, yes. What of it?_
> 是的，是的。那又怎樣？

That is an example of the Fragile Test Problem.
這就是脆弱測試問題的一個例子。

> _The Fragile Test Problem?_
> 脆弱測試問題？

Yes. A common complaint amongst people who try TDD for the first time. They note that small changes to the production code can cause large changes to the tests.
是的。第一次嘗試 TDD 的人普遍抱怨的問題。他們注意到生產程式碼的微小變更可能導致測試的巨大變更。

> _Yeah. That’s really frustrating. I almost gave up on TDD when I first encountered it._
> 是的。這真的很令人沮喪。我第一次遇到它時幾乎放棄了 TDD。

Unfortunately, that is a common reaction.
不幸的是，這是一種常見的反應。

> _OK, but what can be done about it?_
> 好吧，但是可以做些什麼呢？

Design the structure of your tests to be contra-variant with the structure of your production code.
設計你的測試結構，使其與生產程式碼的結構逆變。

> _Contra-variant?_
> 逆變？

Yes. The structure of your tests should not be a mirror of the structure of your code. The fact that you have a class named `X` should not automatically imply that you have a test class named `XTest`.
是的。你的測試結構不應該是你的程式碼結構的鏡像。你有一個名為 X 的類別的事實並不意味著你必須有一個名為 XTest 的測試類別。

> _But wait. That breaks the rules!_
> 等等。這違反了規則！

What rules?
什麼規則？

> _The rules that say that there should be one test class per class._
> 說每個類別應該有一個測試類別的規則。

There is no such rule.
沒有這樣的規則。

> _There isn’t? I’m sure I’ve read it and seen it._
> 沒有嗎？我確信我讀過也見過。

Not everything your read and see is a rule.
並不是你讀到和看到的一切都是規則。

> _Fair enough. But if the structure of the code and the tests must be, um. contra-variant, then how should the tests be structured?_
> 說的也是。但是如果程式碼和測試的結構必須是，嗯，逆變的，那麼測試應該如何構建？

First, let’s agree on a basic fact. If a small change in one module of a system causes large changes in many other modules of the system, the system has a design problem.
首先，讓我們就一個基本事實達成一致。如果一個系統的某個模組中的微小變更導致系統的許多其他模組發生巨大變更，則該系統存在設計問題。

> _Yes, I think that’s obvious – software design 101 so to speak._
> 是的，我認為這很明顯——可以說是軟體設計 101。

Then, clearly, if a small change to the production code causes large changes to the tests, there is a design problem.
那麼，顯然，如果對生產程式碼的微小變更導致測試的巨大變更，則存在設計問題。

> _I see that point, yes._
> 我明白了，是的。

Therefore the tests must have their own design. They cannot simply follow along with the structure of the production code.
因此，測試必須有自己的設計。它們不能簡單地遵循生產程式碼的結構。

> _Hmmm. I see. If the two designs are the same, then they are coupled; and that coupling causes fragility._
> 嗯。我明白了。如果兩個設計相同，那麼它們就是耦合的；而這種耦合會導致脆弱性。

Right. The coupling between the tests and the production code must be minimized.
沒錯。測試和生產程式碼之間的耦合必須最小化。

> _But wait! The tests and the code must be coupled because they both describe the same behavior._
> 等等！測試和程式碼必須耦合，因為它們都描述相同的行為。

Correct. Their behavior is coupled; but their structure need not be coupled. And even the behavioral coupling need not be as tight as you think.
正確。它們的行為是耦合的；但是它們的結構不需要耦合。甚至行為耦合也不需要像你想像的那麼緊密。

> _Can you give me an example?_
> 你能給我一個例子嗎？

Suppose I begin writing a new class. Call it `X`. I first write a new test class named `XTest`.
假設我開始編寫一個新的類別。稱它為 X。我首先編寫一個名為 XTest 的新測試類別。

> _But you said we shouldn’t do that._
> 但你說我們不應該這樣做。

Bear with me. We’ve just begun. As I add more and more unit tests to `XTest` I add more and more code to `X`.
耐心點。我們才剛開始。當我向 XTest 添加越來越多的單元測試時，我向 X 添加越來越多的程式碼。

> _And you refactor that code!_
> 然後你重構那些程式碼！

Indeed I do. I refactor it by extracting private methods from the original functions that are called by `XTest`.
的確如此。我透過從 XTest 呼叫的原始函數中提取私有方法來重構它。

> _And you refactor the tests too, right?_
> 你也重構了測試，對吧？

Absolutely! I look at the coupling between `XTest` and `X` and I work to minimize it. I might do this by adding constructor arguments to `X` or raising the abstraction level of the arguments I pass into `X`. I may even impose a polymorphic interface between `XTest` and `X`.
當然！我查看 XTest 和 X 之間的耦合，並努力將其最小化。我可能會透過向 X 添加建構子參數或提高傳遞給 X 的參數的抽象層級來做到這一點。我甚至可能在 XTest 和 X 之間實施多型的介面。

> _You’d do all that just for a test?_
> 你只會為了測試而做所有這些嗎？

Think of it this way. The `XTest` is just the first client of `X`. I always want to decrease the coupling between clients and servers. So I used the same techniques I would use in normal production code to reduce the coupling between the `XTest` and `X`.
這樣想。XTest 只是 X 的第一個客戶端。我總是想減少客戶端和伺服器之間的耦合。因此，我使用了與在正常的生產程式碼中使用的相同技術來減少 XTest 和 X 之間的耦合。

> _OK, but the structure of the tests is still the same as the structure of the code. You still have `X` and `XTest`._
> 好吧，但是測試的結構仍然與程式碼的結構相同。你仍然有 X 和 XTest。

Yes, at the class level they are the same; and that’s about to change. Before we explore that change, however, I want you to note that there are already profound structural differences at the method level.
是的，在類別層級它們是相同的；而且這即將改變。然而，在我們探索這種改變之前，我想讓你注意到方法層級上已經存在深刻的結構差異。

> _Um. Sure. `XTest` is just using the public methods of `X`; but most of the code is now in the private methods that you extracted._
> 嗯。當然。XTest 只是使用 X 的公共方法；但大多數程式碼現在都在你提取的私有方法中。

Right! The structural symmetry is already broken. But now it’s going to break even more.
沒錯！結構對稱性已經被打破。但現在它將變得更加嚴重。

> _How so?_
> 怎麼會？

As I look at all those private method in `X` I will inevitably see that there are ways to group those methods into different classes. One group of methods will use a particular subset of the fields of `X`. That group can be extracted as a class.
是的！提取越來越多的函數。發現越來越多的類別。過了一段時間，我們在生產程式碼中擁有一個完整的類別家族，它們位於 X 的簡單 API 之後。

> _But you don’t write a new test for that class, do you?_
> 但你沒有為該類別編寫新的測試，對嗎？

No! Because every bit of the code within that new class is being covered by the tests that are still just using the public API of `X`.
不！因為該新類別中的每一段程式碼都受到仍然只使用 X 的公共 API 的測試的覆蓋。

> _And this process continues, doesn’t it?_
> 並且這個過程會一直持續下去，不是嗎？

Yes! More and more functions are extracted. More and more classes are discovered. After awhile we have a whole family of classes in the production code that sit behind that simple API of `X`.
是的！提取越來越多的函數。發現越來越多的類別。過了一段時間，我們在生產程式碼中擁有一個完整的類別家族，它們位於 X 的簡單 API 之後。

> _And they are all tested by `XTest`._
> 並且它們都由 XTest 測試。

Right! The structure has been almost perfectly decoupled. What’s more the API of `X` has been successively refined to be so narrow and abstract that it is minimally coupled to the clients that use it; including `XTest`.
沒錯！結構幾乎完全解耦。更重要的是，X 的 API 已經被連續地細化，變得如此狹窄和抽象，以至於它與使用它的客戶端（包括 XTest）的耦合最小。

> _OK, I see that. I see that the structure of the tests can vary independently from the structure of the production code. And I agree that that’s a good thing. But what about the behavior. They are still strongly coupled by behavior._
> 好吧，我明白了。我看到測試的結構可以獨立於生產程式碼的結構而變化。我同意這是件好事。但是行為呢？它們仍然被行為強烈耦合。

Think about what’s going while `X` is being developed. What’s happening to `XTest`?
想想在開發 X 時發生了什麼。XTest 發生了什麼？

> _Well, more and more tests are being added to it; and the interface with `X` is being progressively narrowed and abstracted._
> 嗯，越來越多的測試被添加到其中；並且與 X 的介面正在逐漸縮小和抽象。

Right. Now say that first part again.
沒錯。現在再說第一部分。

> _More and more tests are being added?_
> 正在添加越來越多的測試？

Right. And each one of those tests is entirely concrete. Each one of those tests is a small specification of a very particular behavior. Taken together the sum of all the tests is…
沒錯。並且每個測試都是完全具體的。每個測試都是非常特定的行為的小規範。將所有測試加在一起的總和是…

> _The specification of the behavior of the `X` API._
> X API 行為的規範。

Right! So as development proceeds the test suite becomes more and more of a specification – it becomes more and more _specific_.
沒錯！因此，隨著開發的進行，測試套件變得越來越像一個規範——它變得越來越_具體_。

> _Sure. I see that._
> 當然。我明白了。

But what is happening to the classes behind the `X` API? What would any good software designer do when confronted with an ever growing list of specifications?
但是 X API 背後的類別發生了什麼？當任何優秀的軟體設計師遇到不斷增長的規範列表時，會做些什麼？

> _Well, of course, the way we deal with complex specifications is to generalize._
> 嗯，當然，我們處理複雜規範的方式是概括。

Correct! Instead of writing code for each and every case of every paragraph of every specification, we find ways to make the code _generic_.
正確！我們不是為每個規範的每個段落的每個案例編寫程式碼，而是找到使程式碼_通用_的方法。

> _Why does this matter to the coupling of the behavior?_
> 這對行為的耦合有什麼影響？

As development proceeds, the behavior of the tests becomes ever more specific. The behavior of the production code becomes ever more generic. The two behaviors move in opposite directions along the generalization axis.
隨著開發的進行，測試的行為變得越來越具體。生產程式碼的行為變得越來越通用。這兩種行為沿著概括軸朝相反的方向移動。

> _And this reduces coupling?_
> 這會減少耦合嗎？

Yes. Because while the behavior of the production code _satisfies_ the specifications within the tests, it also has the ability to satisfy a whole spectrum of unspecified behaviors.
是的。因為雖然生產程式碼的行為_滿足_測試中的規範，但它也能夠滿足一系列未指定的行為。

And that last bit is absolutely essential, because no test suite can specify every required behavior. The production code _must_ generalize the subset of behaviors specified by the tests to _all_ the behaviors required of the system.
最後一點絕對至關重要，因為沒有測試套件可以指定所有需要的行為。生產程式碼必須概括測試指定的行為子集，以_所有_系統需要的行為。

> _So you are saying that the test suite in incomplete?_
> 所以你說測試套件是不完整的？

Of course! It is entirely impractical to specify absolutely everything. So what happens instead is that we gradually increase the generality of the production code until every test that we could possibly write will pass.
當然！指定絕對所有內容是不切實際的。因此，相反，我們逐漸提高生產程式碼的通用性，直到我們可能編寫的每個測試都會通過。

> _Woah! We keep writing failing tests in order to drive the generality of the production code to a point where it becomes impossible to write another failing test. Woah!_
> 哇！我們不斷編寫失敗的測試，以驅動生產程式碼的通用性，直到無法再編寫另一個失敗的測試。哇！

Woah indeed. But here’s the thing. The act of generalizing _is the act of decoupling_. We decouple by generalizing!
確實令人驚嘆。但這裡有一件事。概括的行為就是解耦的行為。我們通過概括來解耦！

> _Oh, wow! And so we decouple structure AND behavior. Wow._
> 哦，哇！所以我們解耦了結構和行為。哇。

Right! Now, give me a recap.
沒錯！現在，給我一個回顧。

> _OK. Um. The structure of the tests must not reflect the structure of the production code, because that much coupling makes the system fragile and obstructs refactoring. Rather, the structure of the tests must be independently designed so as to minimize the coupling to the production code._
> 好吧。嗯。測試的結構不能反映生產程式碼的結構，因為太多的耦合會使系統脆弱並阻礙重構。相反，測試的結構必須被獨立設計，以最大限度地減少與生產程式碼的耦合。

Good! And what about behavior?
好！那麼行為呢？

> _As the tests get more specific, the production code gets more generic. The two streams of code move in opposite directions along the generality axis until no new failing test can be written._
> 隨著測試變得更加具體，生產程式碼變得更加通用。兩種程式碼流沿著通用性軸朝相反的方向移動，直到無法編寫新的失敗測試。

Good! I think you’ve got it.
好！我想你明白了。

> _Yeah! Contra-variance FTW!_
> 是的！逆變 FTW！