---
date: '2025-03-13T23:57:10+08:00'
title: 'Testing Like the TSA'
tags: ["Uncle Bob"]
---
[Source](https://blog.cleancoder.com/uncle-bob/2017/03/06/TestingLikeTheTSA.html)

I was very glad to read in DHH’s [recent post](https://signalvnoise.com/posts/3159-testing-like-the-tsa) that he is actually still using TDD***. I’m glad he has realized that TDD is not, in fact, [dead](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html).

我很高興在 DHH 最近的[文章](https://www.google.com/url?sa=E&q=https%3A%2F%2Fsignalvnoise.com%2Fposts%2F3159-testing-like-the-tsa)中讀到他實際上仍在使用 TDD***。我很高興他意識到 TDD 實際上並沒有[死亡](https://www.google.com/url?sa=E&q=http%3A%2F%2Fdavid.heinemeierhansson.com%2F2014%2Ftdd-is-dead-long-live-testing.html)。

This blog is a simple response; just to state a couple of disagreements. But I have to say, I agree more than I disagree.

這篇部落格只是一個簡單的回應；只是為了說明幾個不同意的地方。但我必須說，我同意的地方比不同意的地方還多。

DHH presented seven points. I have reproduced them below, along with my comments. And since DHH did not justify his opinions, I’ll not justify mine.

DHH 提出了七點。我將它們複製如下，以及我的評論。由於 DHH 沒有為他的觀點辯護，我也不會為我的辯護。

- (1. Don’t aim for 100% coverage. 不要以 100% 的覆蓋率為目標。

> _Disagree. Aim as high as you can. Treat 100% as an asymptotic goal. No other number makes a reasonable goal. There is never a reason to stop driving your coverage number higher._
> 不同意。盡可能地以高為目標。將 100% 視為一個漸近線目標。沒有其他數字可以成為合理的目標。永遠沒有理由停止提高你的覆蓋率數字。

- (2. Code-to-test ratios above 1:2 is a smell, above 1:3 is a stink. 程式碼與測試的比率高於 1:2 是一種氣味，高於 1:3 是一種惡臭。

> _Disagree. 1:1 LOC is about right. If you have 20K lines of code, you probably ought to have about 20K lines of tests. (BTW, I can’t make any sense of DHH’s ratios there. I think he meant the first one the other way around (2:1) and I think he meant the second one to be (1.5:1). Or maybe I’m just dense._
> 不同意。 1:1 LOC 差不多是對的。如果你有 2 萬行程式碼，你可能應該有大約 2 萬行測試。（順便說一下，我看不懂 DHH 的比率。我認為他指的是第一個反過來 (2:1)，我認為他指的是第二個 (1.5:1)。或者可能我只是太笨了。

- (3. You’re probably doing it wrong if testing is taking more than 1/3 of your time. You’re definitely doing it wrong if it’s taking up more than half. 如果測試佔用你超過 1/3 的時間，你可能做錯了。如果佔用你超過一半的時間，你肯定做錯了。

> _Agree. Testing should take none of your time. Writing tests, with TDD, requires negative time (i.e. it saves you a boatload) Every worthwhile test you don’t write costs you time._
> 同意。測試應該不佔用你任何時間。使用 TDD 編寫測試需要負時間（即它為你節省了很多）。每一個值得的測試，你不寫，都會花費你的時間。

- (4. Don’t test standard Active Record associations, validations, or scopes. 不要測試標準的 Active Record 關聯、驗證或範圍。

> _Agree. There’s generally no reason to test your framework; so long as you trust it. If you don’t trust it (and there are many frameworks out there that are not trustworthy) then writing some tests against the framework might be worthwhile. Consider it equivalent to “Incomming Inspection”._
> 同意。通常沒有理由測試你的框架；只要你信任它。如果你不信任它（並且有很多框架是不值得信任的），那麼針對框架編寫一些測試可能是有價值的。將其視為等同於“來料檢驗”。

- (5. Reserve integration testing for issues arising from the integration of separate elements (aka don’t integration test things that can be unit tested instead). 將整合測試保留給由獨立元素的整合引起的問題（也就是說，不要整合測試可以用單元測試代替的東西）。

> _Agree. Keep your tests focused. Don’t test through UIs. Don’t test through web servers. Test as close to the code as you can._
> 同意。保持你的測試重點。不要透過 UI 進行測試。不要透過 Web 伺服器進行測試。盡可能靠近程式碼進行測試。

- (6. Don’t use Cucumber unless you live in the magic kingdom of non-programmers-writing-tests (and send me a bottle of fairy dust if you’re there!) 除非你住在非程式設計師編寫測試的魔法王國（如果你在那裡，請寄給我一瓶仙塵！）否則不要使用 Cucumber。

> _Agree and Disagree. Cucumber (Gherkin) is worth it only if you have business people and/or QA who are willing to read your tests. If they will also write your acceptance tests then: ABSOLUTELY send that fairy dust far and wide; because it’s worth its weight in diamonds._
> 同意和不同意。只有當你有願意閱讀你的測試的業務人員和/或 QA 時，Cucumber (Gherkin) 才是值得的。如果他們也願意編寫你的驗收測試，那麼：絕對要廣泛傳播這種仙塵；因為它價值連城。

- (7. Don’t force yourself to test-first every controller, model, and view (my ratio is typically 20% test-first, 80% test-after). 不要強迫自己首先測試每個控制器、模型和視圖（我的比例通常是 20% 先測試，80% 後測試）。

> _Agree… Sort of. Some controllers, models, and views are too stupid to bother to test. If they are obviously correct, because they are one line of code, then testing them might be superfluous. But be careful. Sometimes one line of code has 20 lines of semantics._
> 同意……在某種程度上。有些控制器、模型和視圖太蠢了，不值得測試。如果它們顯然是正確的，因為它們只有一行程式碼，那麼測試它們可能是多餘的。但要小心。有時候一行程式碼有 20 行語義。

---

*** It has been pointed out to me that the “TSA” post actually predates the “TDD is Dead” post by several years. Somehow or another I got the two backwards. (sigh).

*** 有人向我指出，“TSA”文章實際上比“TDD 已死”文章早幾年。不知何故，我把這兩個文章搞反了。（嘆氣）。