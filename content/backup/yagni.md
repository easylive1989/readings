---
date: '2025-02-17T13:21:44+08:00'
title: 'YAGNI'
tags: ["Martin Fowler"]
---

[Source](https://martinfowler.com/bliki/Yagni.html)

Yagni originally is an acronym that stands for “You Aren't Gonna Need It”. It is a mantra from [ExtremeProgramming](https://martinfowler.com/bliki/ExtremeProgramming.html) that's often used generally in agile software teams. It's a statement that some capability we presume our software needs in the future should not be built now because “you aren't gonna need it”.

Yagni 最初是「You Aren't Gonna Need It」（你不會需要它）的縮寫。這是一個來自[極限編程（Extreme Programming, XP）](https://martinfowler.com/bliki/ExtremeProgramming.html)的口號，在敏捷開發團隊中經常被廣泛使用。它的核心理念是，我們假設軟體未來可能需要某種功能，但現在不應該提前開發，因為「你不會需要它」。

Yagni is a way to refer to the XP practice of Simple Design (from the first edition of [The White Book](https://www.amazon.com/gp/product/0321278658/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0321278658&linkCode=as2&tag=martinfowlerc-20), the second edition refers to the related notion of “incremental design”). 1 Like many elements of XP, it's a sharp contrast to elements of the widely held principles of software engineering in the late 90s. At that time there was a big push for careful up-front planning of software development.

Yagni 是 XP 中簡單設計（Simple Design）實踐的一部分（來自[《白皮書》（The White Book）](https://www.amazon.com/gp/product/0321278658/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0321278658&linkCode=as2&tag=martinfowlerc-20)第一版，第二版則提到了相關的增量設計（Incremental Design）概念）。與 90 年代末期流行的軟體工程原則相比，XP 的許多理念形成了鮮明的對比。當時，軟體開發界普遍推崇在開發前進行詳細的規劃。

Let's imagine I'm working with a startup in Minas Tirith selling insurance for the shipping business. Their software system is broken into two main components: one for pricing, and one for sales. The dependencies are such that they can't usefully build sales software until the relevant pricing software is completed.

讓我們舉個例子：假設我在米那斯提力斯（Minas Tirith）的一家初創公司工作，這家公司專門為航運業提供保險。它的軟體系統由兩個主要組件組成：一個用於定價，另一個用於銷售。而由於其依賴關係，在相關的定價軟體完成之前，銷售軟體是無法有效構建的。

At the moment, the team is working on updating the pricing component to add support for risks from storms. They know that in six months time, they will need to also support pricing for piracy risks. Since they are currently working on the pricing engine they consider building the presumptive feature 2 for piracy pricing now, since that way the pricing service will be complete before they start working on the sales software.

目前，開發團隊正在更新定價組件，以增加對風暴風險的支持。他們知道在六個月後，他們還需要支援海盜風險的定價。由於現在正在開發定價引擎，團隊考慮是否要提前構建這個預期功能（presumptive feature），以確保在開始開發銷售軟體之前，定價服務已經完整。

Yagni argues against this, it says that since you won't need piracy pricing for six months you shouldn't build it until it's necessary. So if you think it will take two months to build this software, then you shouldn't start for another four months (neglecting any buffer time for schedule risk and updating the sales component).

Yagni 反對這種做法。它的觀點是，既然在六個月後才會需要海盜風險定價功能，那麼現在就不應該開發。如果這個功能需要兩個月的開發時間，那麼最早也應該等到四個月後才開始開發（暫不考慮排程風險或銷售組件的更新時間）。

The first argument for yagni is that while we may now think we need this presumptive feature, it's likely that we will be wrong. After all the context of agile methods is an acceptance that we welcome changing requirements. A plan-driven requirements guru might counter argue that this is because we didn't do a good-enough job of our requirements analysis, we should have put more time and effort into it. I counter that by pointing out how difficult and costly it is to figure out your needs in advance, but even if you can, you can still be blind-sided when the Gondor Navy wipes out the pirates, thus undermining the entire business model.

Yagni 的第一個論點：儘管我們現在認為需要這個預期功能，但很可能我們的判斷是錯的。敏捷方法論的核心理念之一就是接受需求變更。一些推崇傳統規劃驅動方法的專家可能會反駁，認為這是因為我們的需求分析做得不夠充分，應該投入更多時間來完善。然而，我的觀點是，提前準確預測需求是極其困難且昂貴的，甚至即使我們真的準確預測了，也仍然可能被突如其來的變化打亂計畫。例如，如果剛鐸（Gondor）海軍剷除了海盜，那麼這個保險產品的整個商業模式將被徹底顛覆，使該功能變得毫無價值。

In this case, there's an obvious cost of the presumptive feature - the **cost of build**: all the effort spent on analyzing, programming, and testing this now useless feature.

在這種情況下，開發預期功能會產生明顯的開發成本，即花費在分析、編程和測試這個最終無用功能的所有時間與精力。

But let's consider that we were completely correct with our understanding of our needs, and the Gondor Navy didn't wipe out the pirates. Even in this happy case, building the presumptive feature incurs two serious costs. The first cost is the cost of delayed value. By expending our effort on the piracy pricing software we didn't build some other feature. If we'd instead put our energy into building the sales software for weather risks, we could have put a full storm risks feature into production and be generating revenue two months earlier. This **cost of delay** due to the presumptive feature is two months revenue from storm insurance.

但讓我們假設我們對需求的理解是完全正確的，並且剛鐸（Gondor）海軍並沒有剷除海盜。即便在這種樂觀的情況下，開發這個預期功能（presumptive feature）仍然會帶來兩個嚴重的成本問題。第一個成本是延遲價值的成本（cost of delayed value）。由於我們將精力投入到開發海盜風險的定價軟體，因此無法開發其他功能。如果我們選擇將這些資源用於開發天氣風險的銷售系統，那麼完整的暴風風險功能可以提前兩個月投入生產，從而更早開始產生收益。這種由於開發預期功能而導致的**延遲成本（cost of delay）**，相當於暴風風險保險的兩個月收入損失。

The common reason why people build presumptive features is because they think it will be cheaper to build it now rather than build it later. But that cost comparison has to be made at least against the cost of delay, preferably factoring in the probability that you're building an unnecessary feature, for which your odds are at least ⅔. 3

為何開發者會選擇提前開發？人們之所以選擇開發預期功能，通常是因為他們認為「現在開發比未來開發更便宜」。但這種成本比較至少應該考慮到**延遲成本**，更理想的做法是還應該考慮該功能最終可能根本不需要的機率，這個機率通常至少有 **⅔**。

Often people don't think through the comparative cost of building now to building later. One approach I use when mentoring developers in this situation is to ask them to **imagine the refactoring** they would have to do later to introduce the capability when it's needed. Often that thought experiment is enough to convince them that it won't be significantly more expensive to add it later. Another result from such an imagining is to add something that's easy to do now, adds minimal complexity, yet significantly reduces the later cost. Using lookup tables for error messages rather than inline literals are an example that are simple yet make later translations easier to support.

許多人沒有仔細思考「現在開發 vs. 以後開發」的比較成本。在這種情況下，我在指導開發者時，經常使用的一種方法是請他們想像未來進行重構（refactoring）時需要做的工作量。這個思考實驗通常足以讓他們意識到，將來再添加這個功能其實並不會貴很多。此外，這種思考也可能會讓他們發現一些現在可以做的小改動，能夠顯著降低未來的開發成本。例如，現在使用查詢表（lookup table）來管理錯誤訊息，而不是直接寫在程式碼中，這樣將來要支援多語系時，改動就會變得更容易。

The cost of delay is one cost that a successful presumptive feature imposes, but another is the **cost of carry**. The code for the presumptive feature adds some complexity to the software, this complexity makes it harder to modify and debug that software, thus increasing the cost of other features. The extra complexity from having the piracy-pricing feature in the software might add a couple of weeks to how long it takes to build the storm insurance sales component. That two weeks hits two ways: the additional cost to build the feature, plus the additional cost of delay since it look longer to put it into production. We'll incur a cost of carry on every feature built between now and the time the piracy insurance software starts being useful. Should we never need the piracy-pricing software, we'll incur a cost of carry on every feature built until we remove the piracy-pricing feature (assuming we do), together with the cost of removing it.

預設性功能（Presumptive feature）成功時會產生延遲成本（Cost of Delay），但還有另一種成本，稱為**持有成本（Cost of Carry）**。 預設性功能的程式碼會增加軟體的複雜性，這種複雜性會使得軟體更難修改和除錯，進而增加其他功能的成本。 例如，軟體中存在盜版定價功能，可能會使構建風暴保險銷售組件的時間額外增加幾個星期。 這兩個星期的影響有兩個方面：構建功能的額外成本，以及由於生產時間延長而造成的額外延遲成本。 從現在到盜版保險軟體開始派上用場的這段時間內，我們構建的每個功能都會產生持有成本。 如果我們永遠不需要盜版定價軟體，那麼直到我們移除盜版定價功能為止（假設我們這麼做），我們構建的每個功能都會產生持有成本，以及移除它的成本。

So far I've divided presumptive features in two categories: successful and unsuccessful. Naturally there's really a spectrum there, and with one point on that spectrum that's worth highlighting: the right feature built wrong. Development teams are always learning, both about their users and about their code base. They learn about the tools they're using and these tools go through regular upgrades. They also learn about how their code works together. All this means that you often realize that a feature coded six months ago wasn't done the way you now realize it should be done. In that case you have accumulated [TechnicalDebt](https://martinfowler.com/bliki/TechnicalDebt.html) and have to consider the **cost of repair** for that feature or the on-going costs of working around its difficulties.

到目前為止，我將預設性功能分為兩類：成功和不成功。 當然，實際上這是一個連續譜，其中有一個值得強調的點：構建了錯誤的正確功能。 開發團隊總是在學習，包括關於他們的用戶和他們的程式碼庫。 他們了解他們正在使用的工具，而這些工具也會定期升級。 他們也了解他們的程式碼如何協同工作。 所有這些都意味著，您經常會意識到六個月前編碼的功能並不是按照您現在意識到的方式完成的。 在這種情況下，您已經累積了 [技術債（TechnicalDebt）](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Fbliki%2FTechnicalDebt.html)，並且必須考慮該功能的**修復成本（Cost of Repair）**，或者規避其困難所產生的持續成本。

So we end up with three classes of presumptive features, and four kinds of costs that occur when you neglect yagni for them.

因此，我們最終得到了三種類型的預設性功能，以及當您忽略 YAGNI 原則時，針對它們會發生的四種成本。


![](https://martinfowler.com/bliki/images/yagni/sketch.png)

My insurance example talks about relatively user-visible functionality, but the same argument applies for abstractions to support future flexibility. When building the storm risk calculator, you may consider putting in abstractions and parameterizations now to support piracy and other risks later. Yagni says not to do this, because you may not need the other pricing functions, or if you do your current ideas of what abstractions you'll need will not match what you learn when you do actually need them. This doesn't mean to forego all abstractions, but it does mean any abstraction that makes it harder to understand the code for current requirements is presumed guilty.

我的保險例子談論的是相對用戶可見的功能，但同樣的論點也適用於支持未來彈性的抽象概念。 在構建風暴風險計算器時，您可能會考慮現在加入抽象概念和參數化，以支持以後的盜版和其他風險。 YAGNI 說不要這樣做，因為您可能不需要其他定價功能，或者即使需要，您現在對所需抽象概念的想法也可能與您實際需要它們時所學到的不符。 這並不意味著放棄所有抽象概念，但它確實意味著任何使理解當前需求程式碼變得更加困難的抽象概念，都應被預設為有罪。

Yagni is at its most visible with larger features, but you see it more frequently with small things. Recently I wrote some code that allows me to highlight part of a line of code. For this, I allow the highlighted code to be specified using a regular expression. One problem I see with this is that since the whole regular expression is highlighted, I'm unable to deal with the case where I need the regex to match a larger section than what I'd like to highlight. I expect I can solve that by using a group within the regex and letting my code only highlight the group if a group is present. But I haven't needed to use a regex that matches more than what I'm highlighting yet, so I haven't extended my highlighting code to handle this case - and won't until I actually need it. For similar reasons I don't add fields or methods until I'm actually ready to use them.

YAGNI 在較大的功能中最為明顯，但在小事情上您會更頻繁地看到它。 最近我寫了一些程式碼，允許我突出顯示一行程式碼的一部分。 為此，我允許使用正則表達式指定要突出顯示的程式碼。 我發現的一個問題是，由於整個正則表達式都被突出顯示，因此我無法處理需要正則表達式匹配比我想要突出顯示的更大的部分的情況。 我預計我可以通過在正則表達式中使用群組，並讓我的程式碼僅在存在群組時才突出顯示該群組來解決這個問題。 但我還沒有需要匹配比我突出顯示的更多的正則表達式，所以我還沒有擴展我的突出顯示程式碼來處理這種情況 - 並且在實際需要它之前不會這樣做。 出於類似的原因，我不會添加字段或方法，直到我真正準備好使用它們。

Small yagni decisions like this fly under the radar of project planning. As a developer it's easy to spend an hour adding an abstraction that we're sure will soon be needed. Yet all the arguments above still apply, and a lot of small yagni decisions add up to significant reductions in complexity to a code base, while speeding up delivery of features that are needed more urgently.

像這樣的小型 YAGNI 決策在項目計劃中並不引人注目。 作為開發人員，很容易花一個小時添加一個我們確信很快就需要的抽象概念。 然而，上述所有論點仍然適用，並且許多小型 YAGNI 決策加起來可以顯著降低程式碼庫的複雜性，同時加快更緊急需要的功能的交付速度。

Now we understand why yagni is important we can dig into a common confusion about yagni. **Yagni only applies to capabilities built into the software to support a presumptive feature, it does not apply to effort to make the software easier to modify.** Yagni is only a viable strategy if the code is easy to change, so expending effort on refactoring isn't a violation of yagni because refactoring makes the code more malleable. Similar reasoning applies for practices like [SelfTestingCode](https://martinfowler.com/bliki/SelfTestingCode.html) and [ContinuousDelivery](https://martinfowler.com/bliki/ContinuousDelivery.html). These are [enabling practices for evolutionary design](https://martinfowler.com/articles/designDead.html), without them yagni turns from a beneficial practice into a curse. But if you do have a malleable code base, then yagni reinforces that flexibility. Yagni has the curious property that it is both enabled by and enables evolutionary design.

現在我們了解了為什麼 YAGNI 很重要，我們可以深入研究關於 YAGNI 的常見混淆。 **YAGNI 僅適用於內建於軟體中以支持預設性功能的功能，不適用於使軟體更易於修改的工作。** YAGNI 只有在程式碼易於更改時才是一種可行的策略，因此在重構上花費精力並不違反 YAGNI，因為重構使程式碼更具可塑性。 類似的推理適用於像 [自測試程式碼（SelfTestingCode）](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Fbliki%2FSelfTestingCode.html) 和 [持續交付（ContinuousDelivery）](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Fbliki%2FContinuousDelivery.html) 這樣的實踐。 這些是 [演化式設計的賦能實踐](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Farticles%2FdesignDead.html)，沒有它們，YAGNI 就會從有益的實踐變成詛咒。 但如果您確實有一個可塑的程式碼庫，那麼 YAGNI 會增強這種靈活性。 YAGNI 具有奇特的特性，它既受演化式設計的啟用，又可以啟用演化式設計。

I also argue that yagni only applies when you introduce extra complexity now that you won't take advantage of until later. If you do something for a future need that doesn't actually increase the complexity of the software, then there's no reason to invoke yagni.

我也認為，YAGNI 僅在您現在引入額外的複雜性，而您稍後才會利用它時才適用。 如果您為將來的需求做了一些事情，但實際上並沒有增加軟體的複雜性，那麼就沒有理由調用 YAGNI。

Having said all this, there are times when applying yagni does cause a problem, and you are faced with an expensive change when an earlier change would have been much cheaper. The tricky thing here is that these cases are hard to spot in advance, and much easier to remember than the cases where yagni saved effort 4. My sense is that yagni-failures are relatively rare and their costs are easily outweighed by when yagni succeeds.

說了這麼多，有時應用 YAGNI 確實會導致問題，並且當早期更改會便宜得多時，您會面臨昂貴的更改。 這裡棘手的是，這些情況很難提前發現，而且比 YAGNI 節省了精力的情況更容易記住<sup>4</sup>。 我認為 YAGNI 失敗的情況相對較少，而且 YAGNI 成功時所帶來的收益很容易超過其成本。

## Further Reading

My essay [Is Design Dead](https://martinfowler.com/articles/designDead.html) talks in more detail about the role of design and architecture in agile projects, and thus role yagni plays as an enabling practice.

我的文章 [設計已死？（Is Design Dead）](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Farticles%2FdesignDead.html) 更詳細地探討了設計和架構在敏捷專案中的作用，以及 YAGNI 作為一種賦能實踐所扮演的角色。

This principle was first discussed and fleshed out on [Ward's Wiki](http://c2.com/cgi/wiki?YouArentGonnaNeedIt).

這個原則最早在 [Ward's Wiki](https://www.google.com/url?sa=E&q=http%3A%2F%2Fc2.com%2Fcgi%2Fwiki%3FYouArentGonnaNeedIt) 上被討論和充實。

## Notes

1: The origin of the phrase is an early conversation between Kent Beck and Chet Hendrickson on the [C3](https://martinfowler.com/bliki/C3.html) project. Chet came up to Kent with a series of capabilities that the system would soon need, to each one Kent replied “you aren't going to need it”. Chet's a fast learner, and quickly became renowned for his ability to spot opportunities to apply yagni. Although “yagni” began life as an acronym, I feel it's now entered our lexicon as a regular word, and thus forego the capital letters.

1： 這個詞的起源是 Kent Beck 和 Chet Hendrickson 在 [C3](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Fbliki%2FC3.html) 專案中早期的一次對話。 Chet 拿著一系列系統很快會需要的功能來找 Kent，Kent 對每個功能都回答說：“你不需要它（You aren't going to need it）”。 Chet 是一位快速學習者，並迅速以其發現應用 YAGNI 機會的能力而聞名。 雖然“yagni”最初是一個首字母縮略詞，但我認為它現在已經作為一個普通詞進入了我們的詞彙，因此放棄了大寫字母。

2: In this post I use “presumptive feature” to refer to any code that supports a feature that isn't yet being made available for use.

2： 在這篇文章中，我使用“預設性功能（presumptive feature）”來指代任何支持尚未提供的功能使用的程式碼。

3: The ⅔ number is suggested by [Kohavi et al](http://ai.stanford.edu/~ronnyk/ExPThinkWeek2009Public.pdf), who analyzed the value of features built and deployed on products at microsoft and found that, even with careful up-front analysis, only ⅓ of them improved the metrics they were designed to improve.

3： 這個 ⅔ 的數字是由 [Kohavi et al](https://www.google.com/url?sa=E&q=http%3A%2F%2Fai.stanford.edu%2F~ronnyk%2FExPThinkWeek2009Public.pdf) 提出的，他們分析了在微軟產品上構建和部署的功能的價值，發現即使經過仔細的事前分析，只有 ⅓ 的功能改善了它們旨在改善的指標。

4: This is a consequence of availability bias

4: 這是可得性偏誤（availability bias）的結果。