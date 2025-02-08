---
date: '2025-02-08T09:24:00+08:00'
title: 'Unit Test'
tags: ["Martin Fowler"]
---

[Source](https://martinfowler.com/bliki/UnitTest.html)

Unit testing is often talked about in software development, and is a term that I've been familiar with during my whole time writing programs. Like most software development terminology, however, it's very ill-defined, and I see confusion can often occur when people think that it's more tightly defined than it actually is.

單元測試經常在軟體開發中被提及,這是我寫程式時一直熟悉的術語。然而,像大多數軟體開發術語一樣,它的定義非常模糊,我發現當人們認為它的定義比實際更為嚴謹時,往往會引起混淆。

Although I'd done plenty of unit testing before, my definitive exposure was when I started working with Kent Beck and used the Xunit family of unit testing tools. (Indeed I sometimes think a good term for this style of testing might be "xunit testing.") Unit testing also became a signature activity of ExtremeProgramming (XP), and led quickly to TestDrivenDevelopment.

雖然我之前做過很多單元測試,但我真正深入接觸是在與Kent Beck合作並使用Xunit系列單元測試工具時。(事實上,有時我認為這種測試風格的好術語可能是“xunit測試”。)單元測試也成為了極限編程(XP)的標誌性活動,並迅速引導了測試驅動開發。

There were definitional concerns about XP's use of unit testing right from the early days. I have a distinct memory of a discussion on a usenet discussion group where us XPers were berated by a testing expert for misusing the term "unit test." We asked him for his definition and he replied with something like "in the first morning of my training course I cover 24 different definitions of unit test."

從早期開始,XP對單元測試的使用就存在定義上的擔憂。我清楚地記得在一個usenet討論組上的討論,我們這些XP支持者被一位測試專家指責濫用“單元測試”這個術語。我們請他給出他的定義,他回應說:“在我的培訓課程的第一個上午,我涵蓋了24種不同的單元測試定義。”

Despite the variations, there are some common elements. Firstly there is a notion that unit tests are low-level, focusing on a small part of the software system. Secondly unit tests are usually written these days by the programmers themselves using their regular tools - the only difference being the use of some sort of unit testing framework 2. Thirdly unit tests are expected to be significantly faster than other kinds of tests.

儘管存在差異,但還是有一些共同元素。首先,單元測試是低層次的,集中於軟體系統的一小部分。其次,現在的單元測試通常是由程式員自己使用常規工具編寫的——唯一的區別是使用某種單元測試框架。第三,單元測試預期比其他類型的測試快得多。

So there's some common elements, but there are also differences. One difference is what people consider to be a unit. Object-oriented design tends to treat a class as the unit, procedural or functional approaches might consider a single function as a unit. But really it's a situational thing - the team decides what makes sense to be a unit for the purposes of their understanding of the system and its testing. Although I start with the notion of the unit being a class, I often take a bunch of closely related classes and treat them as a single unit. Rarely I might take a subset of methods in a class as a unit. However you define it doesn't really matter.

雖然有一些共同元素,但也存在差異。一個差異是人們認為什麼是單元。面向對象設計傾向於將類作為單元,程序式或函數式方法可能將單個函數視為單元。但實際上這是情境性的——團隊決定什麼對他們理解系統及其測試有意義。雖然我以將單元視為一個類開始,但我經常將一組緊密相關的類視為一個單元。很少我會將一個類中的一部分方法視為一個單元。不論你如何定義其實並不重要。

A more important distinction is whether the unit you're testing should be sociable or solitary. Imagine you're testing an order class's price method. The price method needs to invoke some functions on the product and customer classes. If you like your unit tests to be solitary, you don't want to use the real product or customer classes here, because a fault in the customer class would cause the order class's tests to fail. Instead you use TestDoubles for the collaborators.

更重要的區別在於你測試的單元應該是社交型還是孤立型。想像一下,你在測試訂單類的價格方法。價格方法需要調用產品和客戶類的一些函數。如果你喜歡讓單元測試是孤立的,你不會在這裡使用真實的產品或客戶類,因為客戶類中的錯誤會導致訂單類的測試失敗。相反,你會使用測試替身來替代協作者。

But not all unit testers use solitary unit tests. Indeed when xunit testing began in the 90's we made no attempt to go solitary unless communicating with the collaborators was awkward (such as a remote credit card verification system). We didn't find it difficult to track down the actual fault, even if it caused neighboring tests to fail. So we felt allowing our tests to be sociable didn't lead to problems in practice.

但並非所有的單元測試者都使用孤立的單元測試。事實上,當xunit測試在90年代開始時,除非與協作者的通信非常麻煩(例如遠程信用卡驗證系統),我們並不會嘗試進行孤立測試。我們發現,即使錯誤導致相鄰測試失敗,追踪實際錯誤也不困難。所以我們認為允許測試是社交型的在實踐中不會帶來問題。

Indeed using sociable unit tests was one of the reasons we were criticized for our use of the term "unit testing". I think that the term "unit testing" is appropriate because these tests are tests of the behavior of a single unit. We write the tests assuming everything other than that unit is working correctly.

事實上,使用社交型單元測試是我們被批評使用“單元測試”這個術語的原因之一。我認為“單元測試”這個術語是合適的,因為這些測試是對單個單元行為的測試。我們在編寫測試時,假設除了該單元之外的所有部分都正常工作。

As xunit testing became more popular in the 2000's the notion of solitary tests came back, at least for some people. We saw the rise of Mock Objects and frameworks to support mocking. Two schools of xunit testing developed, which I call the classic and mockist styles. One of the differences between the two styles is that mockists insist upon solitary unit tests, while classicists prefer sociable tests. Today I know and respect xunit testers of both styles (personally I've stayed with classic style).

隨著xunit測試在2000年代越來越流行,孤立測試的概念又回來了,至少對某些人來說是這樣。我們看到了Mock Objects和支持mocking的框架的興起。兩種xunit測試風格發展了起來,我稱之為經典風格和mockist風格。這兩種風格的區別之一是mockist堅持孤立單元測試,而經典風格更喜歡社交型測試。今天我認識並尊重這兩種風格的xunit測試者(我個人一直堅持經典風格)。

Even a classic tester like myself uses test doubles when there's an awkward collaboration. They are invaluable to remove non-determinism when talking to remote services. Indeed some classicist xunit testers also argue that any collaboration with external resources, such as a database or filesystem, should use doubles. Partly this is due to non-determinism risk, partly due to speed. While I think this is a useful guideline, I don't treat using doubles for external resources as an absolute rule. If talking to the resource is stable and fast enough for you then there's no reason not to do it in your unit tests.

即使是像我這樣的經典測試者,在遇到麻煩的協作時也會使用測試替身。它們在消除與遠程服務通信時的非確定性方面非常寶貴。事實上,一些經典的xunit測試者也認為任何與外部資源(如數據庫或文件系統)的協作都應該使用替身。這部分是由於非確定性風險,部分是由於速度問題。雖然我認為這是一個有用的指導方針,但我不會將使用替身作為絕對規則。**如果與資源的通信是穩定且足夠快的,那麼在單元測試中使用它們沒有理由不行。**