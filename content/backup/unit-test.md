---
date: '2025-02-08T09:24:00+08:00'
title: 'Unit Test'
tags: ["Martin Fowler"]
---

[Source](https://martinfowler.com/bliki/UnitTest.html)

Unit testing is often talked about in software development, and is a term that I've been familiar with during my whole time writing programs. Like most software development terminology, however, it's very ill-defined, and I see confusion can often occur when people think that it's more tightly defined than it actually is. 1

單元測試經常在軟體開發中被提及,這是我寫程式時一直熟悉的術語。然而,像大多數軟體開發術語一樣,它的定義非常模糊,我發現當人們認為它的定義比實際更為嚴謹時,往往會引起混淆。

![](https://martinfowler.com/bliki/images/unitTest/sketch.png)

Although I'd done plenty of unit testing before, my definitive exposure was when I started working with Kent Beck and used the Xunit family of unit testing tools. (Indeed I sometimes think a good term for this style of testing might be "xunit testing.") Unit testing also became a signature activity of ExtremeProgramming (XP), and led quickly to TestDrivenDevelopment.

雖然我之前做過很多單元測試,但我真正深入接觸是在與Kent Beck合作並使用Xunit系列單元測試工具時。(事實上,有時我認為這種測試風格的好術語可能是“xunit測試”。)單元測試也成為了極限編程(XP)的標誌性活動,並迅速引導了測試驅動開發。

There were definitional concerns about XP's use of unit testing right from the early days. I have a distinct memory of a discussion on a usenet discussion group where us XPers were berated by a testing expert for misusing the term "unit test." We asked him for his definition and he replied with something like "in the first morning of my training course I cover 24 different definitions of unit test."

從早期開始,XP對單元測試的使用就存在定義上的擔憂。我清楚地記得在一個usenet討論組上的討論,我們這些XP支持者被一位測試專家指責濫用“單元測試”這個術語。我們請他給出他的定義,他回應說:“在我的培訓課程的第一個上午,我涵蓋了24種不同的單元測試定義。”

Despite the variations, there are some common elements. Firstly there is a notion that unit tests are low-level, focusing on a small part of the software system. Secondly unit tests are usually written these days by the programmers themselves using their regular tools - the only difference being the use of some sort of unit testing framework 2. Thirdly unit tests are expected to be significantly faster than other kinds of tests.

儘管存在差異,但還是有一些共同元素。首先,單元測試是低層次的,集中於軟體系統的一小部分。其次,現在的單元測試通常是由程式員自己使用常規工具編寫的——唯一的區別是使用某種單元測試框架。第三,單元測試預期比其他類型的測試快得多。

So there's some common elements, but there are also differences. One difference is what people consider to be a unit. Object-oriented design tends to treat a class as the unit, procedural or functional approaches might consider a single function as a unit. But really it's a situational thing - the team decides what makes sense to be a unit for the purposes of their understanding of the system and its testing. Although I start with the notion of the unit being a class, I often take a bunch of closely related classes and treat them as a single unit. Rarely I might take a subset of methods in a class as a unit. However you define it doesn't really matter.

雖然有一些共同元素,但也存在差異。一個差異是人們認為什麼是單元。面向對象設計傾向於將類作為單元,程序式或函數式方法可能將單個函數視為單元。但實際上這是情境性的——團隊決定什麼對他們理解系統及其測試有意義。雖然我以將單元視為一個類開始,但我經常將一組緊密相關的類視為一個單元。很少我會將一個類中的一部分方法視為一個單元。不論你如何定義其實並不重要。

## Solitary or Socialble?

A more important distinction is whether the unit you're testing should be sociable or solitary ^3. Imagine you're testing an order class's price method. The price method needs to invoke some functions on the product and customer classes. If you like your unit tests to be solitary, you don't want to use the real product or customer classes here, because a fault in the customer class would cause the order class's tests to fail. Instead you use TestDoubles for the collaborators.

更重要的區別在於你測試的單元應該是社交型還是孤立型。想像一下,你在測試訂單類的價格方法。價格方法需要調用產品和客戶類的一些函數。如果你喜歡讓單元測試是孤立的,你不會在這裡使用真實的產品或客戶類,因為客戶類中的錯誤會導致訂單類的測試失敗。相反,你會使用測試替身來替代協作者。

![](https://martinfowler.com/bliki/images/unitTest/isolate.png)

But not all unit testers use solitary unit tests. Indeed when xunit testing began in the 90's we made no attempt to go solitary unless communicating with the collaborators was awkward (such as a remote credit card verification system). We didn't find it difficult to track down the actual fault, even if it caused neighboring tests to fail. So we felt allowing our tests to be sociable didn't lead to problems in practice.

但並非所有的單元測試者都使用孤立的單元測試。事實上,當xunit測試在90年代開始時,除非與協作者的通信非常麻煩(例如遠程信用卡驗證系統),我們並不會嘗試進行孤立測試。我們發現,即使錯誤導致相鄰測試失敗,追踪實際錯誤也不困難。所以我們認為允許測試是社交型的在實踐中不會帶來問題。

Indeed using sociable unit tests was one of the reasons we were criticized for our use of the term "unit testing". I think that the term "unit testing" is appropriate because these tests are tests of the behavior of a single unit. We write the tests assuming everything other than that unit is working correctly.

事實上,使用社交型單元測試是我們被批評使用“單元測試”這個術語的原因之一。我認為“單元測試”這個術語是合適的,因為這些測試是對單個單元行為的測試。我們在編寫測試時,假設除了該單元之外的所有部分都正常工作。

As xunit testing became more popular in the 2000's the notion of solitary tests came back, at least for some people. We saw the rise of Mock Objects and frameworks to support mocking. Two schools of xunit testing developed, which I call the classic and mockist styles. One of the differences between the two styles is that mockists insist upon solitary unit tests, while classicists prefer sociable tests. Today I know and respect xunit testers of both styles (personally I've stayed with classic style).

隨著xunit測試在2000年代越來越流行,孤立測試的概念又回來了,至少對某些人來說是這樣。我們看到了Mock Objects和支持mocking的框架的興起。兩種xunit測試風格發展了起來,我稱之為經典風格和mockist風格。這兩種風格的區別之一是mockist堅持孤立單元測試,而經典風格更喜歡社交型測試。今天我認識並尊重這兩種風格的xunit測試者(我個人一直堅持經典風格)。

Even a classic tester like myself uses test doubles when there's an awkward collaboration. They are invaluable to remove non-determinism when talking to remote services. Indeed some classicist xunit testers also argue that any collaboration with external resources, such as a database or filesystem, should use doubles. Partly this is due to non-determinism risk, partly due to speed. While I think this is a useful guideline, I don't treat using doubles for external resources as an absolute rule. If talking to the resource is stable and fast enough for you then there's no reason not to do it in your unit tests.

即使是像我這樣的經典測試者,在遇到麻煩的協作時也會使用測試替身。它們在消除與遠程服務通信時的非確定性方面非常寶貴。事實上,一些經典的xunit測試者也認為任何與外部資源(如數據庫或文件系統)的協作都應該使用替身。這部分是由於非確定性風險,部分是由於速度問題。雖然我認為這是一個有用的指導方針,但我不會將使用替身作為絕對規則。**如果與資源的通信是穩定且足夠快的,那麼在單元測試中使用它們沒有理由不行。**

## Speed

The common properties of unit tests — small scope, done by the programmer herself, and fast — mean that they can be run very frequently when programming. Indeed this is one of the key characteristics of [SelfTestingCode](https://martinfowler.com/bliki/SelfTestingCode.html). In this situation programmers run unit tests after any change to the code. I may run unit tests several times a minute, any time I have code that's worth compiling. I do this because should I accidentally break something, I want to know right away. If I've introduced the defect with my last change it's much easier for me to spot the bug because I don't have far to look.

單元測試的共同屬性——範圍小、由程式設計師自己完成且速度快——意味著它們可以在程式設計時非常頻繁地運行。 實際上，這是 [自我測試代碼](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Fbliki%2FSelfTestingCode.html) 的關鍵特徵之一。 在這種情況下，程式設計師在對代碼進行任何更改後都會運行單元測試。 每當我有值得編譯的代碼時，我可能會每分鐘運行幾次單元測試。 我這樣做是因為如果我不小心破壞了某些東西，我想立即知道。 如果我使用最新的變更引入了缺陷，我更容易發現錯誤，因為我不必費力尋找。

When you run unit tests so frequently, you may not run all the unit tests. Usually you only need to run those tests that are operating over the part of the code you're currently working on. As usual, you trade off the depth of testing with how long it takes to run the test suite. I'll call this suite the **compile suite**, since it's what I run whenever I think of compiling - even in an interpreted language like Ruby.

當您如此頻繁地運行單元測試時，您可能不會運行所有單元測試。 通常，您只需要運行那些在您目前正在處理的代碼部分上運行的測試。 與往常一樣，您可以在測試的深度與運行測試套件所需的時間之間進行權衡。 我將此套件稱為 **編譯套件**，因為每當我想到編譯時，我都會運行它 - 即使是在像 Ruby 這樣的解釋型語言中。

If you are using Continuous Integration you should run a test suite as part of it. It's common for this suite, which I call the **commit suite**, to include all the unit tests. It may also include a few [BroadStackTests](https://martinfowler.com/bliki/BroadStackTest.html). As a programmer you should run this commit suite several times a day, certainly before any shared commit to version control, but also at any other time you have the opportunity - when you take a break, or have to go to a meeting. The faster the commit suite is, the more often you can run it. 4

如果您正在使用持續整合，您應該將測試套件作為其一部分運行。 我稱之為 **提交套件** 的此套件通常包含所有單元測試。 它也可能包含一些 [廣堆疊測試](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Fbliki%2FBroadStackTest.html)。 作為一名程式設計師，您應該每天運行此提交套件幾次，當然是在任何共享提交到版本控制之前，但也應該在任何其他您有機會的時間運行——當您休息一下或必須去參加會議時。 提交套件越快，您就可以越頻繁地運行它。 4

Different people have different standards for the speed of unit tests and of their test suites. [David Heinemeier Hansson](http://david.heinemeierhansson.com/2014/slow-database-test-fallacy.html) is happy with a compile suite that takes a few seconds and a commit suite that takes a few minutes. [Gary Bernhardt](https://www.destroyallsoftware.com/blog/2014/tdd-straw-men-and-rhetoric) finds that unbearably slow, insisting on a compile suite of around 300ms and [Dan Bodart](http://dan.bodar.com/2012/02/28/crazy-fast-build-times-or-when-10-seconds-starts-to-make-you-nervous/) doesn't want his commit suite to be more than ten seconds

不同的人對單元測試及其測試套件的速度有不同的標準。 [David Heinemeier Hansson](https://www.google.com/url?sa=E&q=http%3A%2F%2Fdavid.heinemeierhansson.com%2F2014%2Fslow-database-test-fallacy.html) 對於一個需要幾秒鐘的編譯套件和一個需要幾分鐘的提交套件感到滿意。 [Gary Bernhardt](https://www.google.com/url?sa=E&q=https%3A%2F%2Fwww.destroyallsoftware.com%2Fblog%2F2014%2Ftdd-straw-men-and-rhetoric) 發現這令人難以忍受的緩慢，堅持要求編譯套件大約 300 毫秒，而 [Dan Bodart](https://www.google.com/url?sa=E&q=http%3A%2F%2Fdan.bodar.com%2F2012%2F02%2F28%2Fcrazy-fast-build-times-or-when-10-seconds-starts-to-make-you-nervous%2F) 不希望他的提交套件超過 10 秒

I don't think there's an absolute answer here. Personally I don't notice a difference between a compile suite that's sub-second or a few seconds. I like Kent Beck's rule of thumb that the commit suite should run in no more than ten minutes. But the real point is that your test suites should run fast enough that you're not discouraged from running them frequently enough. And frequently enough is so that when they detect a bug there's a sufficiently small amount of work to look through that you can find it quickly.

我不認為這裡有一個絕對的答案。 就我個人而言，我沒有注意到次秒的編譯套件或幾秒鐘的編譯套件之間的差異。 我喜歡 Kent Beck 的經驗法則，即提交套件的運行時間不應超過 10 分鐘。 但真正的重點是您的測試套件應該運行得足夠快，以至於不會阻止您足夠頻繁地運行它們。 並且足夠頻繁是指當它們檢測到一個錯誤時，有足夠少量的工作需要查看，以便您可以快速找到它。

## Notes

1: I wrote a little about the historical roots of the name in the entry for [IntegrationTest](https://martinfowler.com/bliki/IntegrationTest.html) 我在 [整合測試](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Fbliki%2FIntegrationTest.html) 的條目中簡要地介紹了名稱的歷史根源。

2: I say “these days” because this is certainly something that has changed due to XP. In the turn-of-the-century debates, XPers were strongly criticized for this as the common view was that programmers should never test their own code. Some shops had specialized unit testers whose entire job would be to write unit tests for code written earlier by developers. The reasons for this included: people having a conceptual blindness to testing their own code, programmers not being good testers, and it was good to have a adversarial relationship between developers and testers. The XPer view was that programmers could learn to be effective testers, at least at the unit level, and that if you involved a separate group the feedback loop that tests gave you would be hopelessly slow. Xunit played an essential role here, it was designed specifically to minimize the friction for programmers writing tests. 我說「現在」，因為這肯定是因 XP 而改變的事情。 在世紀之交的辯論中，XPers 因這一點受到強烈批評，因為普遍的觀點是程式設計師永遠不應該測試自己的代碼。 有些商店有專門的單元測試人員，他們的全部工作是為開發人員先前編寫的代碼編寫單元測試。 這樣做的原因包括：人們對測試自己的代碼存在概念上的盲點，程式設計師不是好的測試人員，並且開發人員和測試人員之間存在對抗關係是件好事。 XPer 的觀點是，程式設計師可以學習成為有效的測試人員，至少在單元層級是這樣，並且如果您涉及一個單獨的團隊，那麼測試給您的反饋迴圈將會非常緩慢。 Xunit 在這裡發揮了至關重要的作用，它專門設計用於最大限度地減少程式設計師編寫測試的摩擦。

3: Jay Fields [came up with the terms](https://leanpub.com/wewut) “solitary” and “sociable” Jay Fields [提出了術語](https://www.google.com/url?sa=E&q=https%3A%2F%2Fleanpub.com%2Fwewut)「孤立型」和「社交型」

4: If you have tests that are useful, but take longer than you want the commit suite to run, then you should build a [DeploymentPipeline](https://martinfowler.com/bliki/DeploymentPipeline.html) and put the slower tests in a later stage of the pipeline. 如果您有有用的測試，但運行時間長於您希望提交套件運行的時間，那麼您應該建構一個 [部署管道](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Fbliki%2FDeploymentPipeline.html)，並將速度較慢的測試放在管道的後期階段。