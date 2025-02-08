---
date: '2025-02-08T21:36:49+08:00'
title: 'Cannot Measure Productivity'
tags: ["Martin Fowler"]
---

[Source](https://www.martinfowler.com/bliki/CannotMeasureProductivity.html)

We see so much emotional discussion about software process, design practices and the like. Many of these arguments are impossible to resolve because the software industry lacks the ability to measure some of the basic elements of the effectiveness of software development. In particular we have no way of reasonably measuring productivity.

我們經常看到許多關於軟體流程、設計實踐等方面的情緒化討論。其中許多爭論無法解決，因為軟體產業缺乏衡量軟體開發有效性基本要素的能力。特別是，我們無法合理地衡量生產力。

Productivity, of course, is something you determine by looking at the input of an activity and its output. So to measure software productivity you have to measure the output of software development - the reason we can't measure productivity is because we can't measure output.

生產力當然是透過觀察活動的投入與產出來確定的。因此，要衡量軟體的生產力，就必須衡量軟體開發的產出——然而，我們無法衡量生產力的原因是我們無法衡量產出。

This doesn't mean people don't try. One of my biggest irritations are studies of productivity based on lines of code. For a start there's all the stuff about differences between languages, different counting styles, and differences due to formatting conventions. But even if you use a consistent counting standard on programs in the same language, all auto-formatted to a single style - lines of code still doesn't measure output properly.

這並不意味著沒有人嘗試去做。最令我感到困擾的一件事，就是那些基於程式碼行數（LOC, Lines of Code）來研究生產力的嘗試。首先，不同語言之間的差異、不同的計算方式，以及因為格式化規範而導致的差異，都使得這種方法複雜化。但即使在相同的語言中，使用統一的計算標準，且程式碼都按照單一風格進行自動格式化，程式碼行數依然無法正確衡量產出。

Any good developer knows that they can code the same stuff with huge variations in lines of code, furthermore code that's well designed and factored will be shorter because it eliminates the duplication. Copy and paste programming leads to high LOC counts and poor design because it breeds duplication. You can prove this to yourself if you go at a program with a refactoring tool that supports [Inline Method](http://www.refactoring.com/catalog/inlineMethod.html). Just using that on common routines should allow you to easy double the LOC count.

任何優秀的開發者都知道，他們可以用巨大的程式碼行數變化來實現相同的功能。此外，設計良好且經過適當重構的程式碼會更簡短，因為它消除了重複。相反，複製和貼上程式碼會導致程式碼行數激增，並產生糟糕的設計，因為它助長了重複現象。如果你使用支持[內聯方法](http://www.refactoring.com/catalog/inlineMethod.html)的重構工具來處理程式碼，你可以輕鬆地將程式碼行數加倍，這一點可以自行驗證。只需對常見的例程使用該工具，即可輕鬆達到這樣的結果。

You would think that lines of code are dead, but it seems that every month I see productivity studies based on lines of code - even in such respected journals as IEEE Software that should know better.

你可能認為程式碼行數已經過時，但似乎每個月我都能看到基於程式碼行數進行的生產力研究——甚至在像 IEEE Software 這樣值得尊敬的期刊中也能看到，這些期刊應該更有見識才對。

Now this doesn't mean that LOC is a completely useless measure, it's pretty good at suggesting the size of a system. I can be pretty confident that a 100 KLOC system is bigger than a 10KLOC system. But if I've written the 100KLOC system in a year, and Joe writes the same system in 10KLOC during the same time, that doesn't make me more productive. Indeed I would conclude that our productivities are about the same but my system is much more poorly designed.

這並不意味著程式碼行數（LOC）是完全沒用的衡量指標，它對於估算系統的規模還是相當有用的。我可以很有把握地說，一個 100KLOC 的系統比一個 10KLOC 的系統要大得多。但是，如果我用一年寫了一個 100KLOC 的系統，而 Joe 在相同時間內寫了一個 10KLOC 的系統，這並不代表我的生產力更高。事實上，我會認為我們的生產力差不多，但我的系統設計得要差得多。

Another approach that's often talked about for measuring output is Function Points. I have a little more sympathy for them, but am still unconvinced. This hasn't been helped by stories I've heard of that talk about a single system getting counts that varied by a factor of three from different function point counters using the same system.

另一種經常被討論的衡量產出的方法是功能點（Function Points）。我對此稍微有一些認同，但仍然不完全信服。尤其是當我聽說，同一系統的功能點數量，在不同的功能點計算者之間，可能會相差三倍時，這更讓我對其存疑。

Even if we did find an accurate way for function points to determine functionality, I still think we are missing the point of productivity. I might say that measuring functionality is a way to look at the direct output of software development, but true output is something else. Assuming an accurate FP counting system, if I spend a year delivering a 100FP system and Joe spends the same year delivering a 50FP system can we assume that I'm more productive? I would say not. It may be that of my 100FP only 30 is actually functionality that's useful to my customer, but Joe's is all useful. I would thus argue that while my direct productivity is higher, Joe's true productivity is higher.

即使我們找到了準確的方法來用功能點衡量功能性，我仍然認為這忽略了生產力的真正意義。你可以說衡量功能性是檢視軟體開發直接產出的一種方法，但真正的產出是另一回事。假設有一個準確的功能點計算系統，如果我花了一年交付了一個 100FP 的系統，而 Joe 在同一年交付了一個 50FP 的系統，我們可以假設我的生產力更高嗎？我認為不能。或許我的 100FP 中只有 30FP 是對客戶有用的功能，而 Joe 的 50FP 則全部有用。因此，我會說，雖然我的直接生產力更高，但 Joe 的真正生產力更高。

Jeff Grigg pointed out to me that there's internal factors that affect delivering function points. “My 100 function points are remarkably similar functions, and it took me a year to do them because I failed to properly leverage reuse. Joe's 50 functions are (bad news for him) all remarkably different. Almost no reuse is possible. But in spite of having to implement 50 remarkably different function points, for which almost no reuse leverage is possible, Joe is an amazing guy, so he did it all in only a year.”

Jeff Grigg 向我指出，有一些內部因素會影響功能點的交付效率。“我的 100 個功能點都非常相似，花了一年完成是因為我沒能有效地利用重複使用。而 Joe 的 50 個功能點（對他來說是個壞消息）完全不同。幾乎沒有重複使用的可能性。但儘管要實現 50 個完全不同的功能點，而且幾乎無法利用重複，Joe 還是個了不起的人，他只用了一年就完成了。”

But all of this ignores the point that even useful functionality isn't the true measure. As I get better I produce 30 useful FP of functionality, and Joe only does 15. But someone figures out that Joe's 15 leads to $10 million extra profit for our customer and my work only leads to $5 million. I would again argue that Joe's true productivity is higher because he has delivered more business value - and I assert that any true measure of software development productivity must be based on delivered business value.

但是，以上討論忽略了一點：即使是有用的功能性，也不是最終的衡量標準。如果我越來越厲害，能交付 30 個有用的功能點，而 Joe 只能交付 15 個，但有人發現，Joe 的 15 個功能點為我們的客戶帶來了 1000 萬美元的額外利潤，而我的工作只帶來了 500 萬美元的利潤。我會再次認為，Joe 的真正生產力更高，因為他交付了更多的商業價值——而我主張，任何衡量軟體開發生產力的真正標準都必須基於交付的商業價值。

This thinking also feeds into success rates. Common statements about software success are bogus because people don't understand [WhatIsFailure](https://www.martinfowler.com/bliki/WhatIsFailure.html). I might argue that a successful project is one that delivers more business value than the cost of the project. So if Joe and I run five projects each, and I succeed on four and Joe on one - do I finally do a better job than Joe? Not necessarily. If my four successes yield $1 million profit each, but Joe's one success yields $10 million more than the cost of all his projects combined - then he's the one who should get the promotion.

這種思維也延伸到成功率的討論。關於軟體成功的常見說法是荒謬的，因為人們不了解[什麼是失敗](https://www.martinfowler.com/bliki/WhatIsFailure.html)。我可能會認為，一個成功的專案是指能帶來超過專案成本的商業價值的專案。所以，如果 Joe 和我各運行了五個專案，我成功了四個，而 Joe 成功了一個——我真的比 Joe 做得更好嗎？未必。如果我四個成功的專案各帶來 100 萬美元的利潤，而 Joe 的一個成功專案帶來的利潤超過了他所有專案的成本加總的 1000 萬美元——那麼應該被提拔的人應該是 Joe。

Some people say “if you can't measure it, you can't manage it”. That's a cop out. Businesses manage things they can't really measure the value of all the time. How do you measure the productivity of a company's lawyers, its marketing department, an educational institution? You can't - but you still need to manage them (see [Robert Austin](https://www.amazon.com/gp/product/0932633366/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0932633366&linkCode=as2&tag=martinfowlerc-20) for more).

有些人說：“如果你不能測量它，你就無法管理它。”這是推卸責任。企業經常需要管理那些無法真正衡量價值的事物。你如何衡量公司律師、生產部門或教育機構的生產力？你無法衡量——但你仍然需要管理它們（可參見[Robert Austin](https://www.amazon.com/gp/product/0932633366/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0932633366&linkCode=as2&tag=martinfowlerc-20) 的相關著作）。

If team productivity is hard to figure out, it's even harder to measure the contribution of individuals on that team. You can get a rough sense of a team's output by looking at how many features they deliver per iteration. It's a crude sense, but you can get a sense of whether a team's speeding up, or a rough sense if one team is more productive than another. But individual contributions are much harder to assess. While some people may be responsible for implementing features, others may play a supporting role - helping others to implement their features. Their contribution is that they are raising the whole team's productivity - but it's very hard to get a sense of their individual output unless you are a developer on that team.

如果團隊的生產力已經很難判斷，那麼衡量團隊中個別成員的貢獻就更加困難了。你可以透過查看每個迭代中交付了多少功能，對團隊的輸出有一個粗略的了解。這種方法很粗糙，但可以讓你感受到團隊是否在加速，或者大致判斷某個團隊是否比另一個團隊更有生產力。然而，個別成員的貢獻就難以評估了。雖然有些人可能負責直接實現功能，但其他人可能在扮演支援的角色——幫助其他人實現功能。他們的貢獻在於提高整個團隊的生產力，但除非你是該團隊的一員，否則很難了解他們的個人輸出。

If all this isn't complicated enough the Economist (sep 13-19, 2003) had an article on productivity trends. It seems that economists are now seeing productivity increases in business due to the computer investments in the nineties. The point is that the improvements lag the investments: “Investing in computers does not automatically boost productivity growth; firms need to reorganize their business practices as well”. The same lag occurred with the invention of electricity.

如果這些問題還不夠複雜，根據《經濟學人》（2003 年 9 月 13 日至 19 日）的一篇文章，經濟學家已經開始觀察到，九十年代的電腦投資正在帶動商業生產力的提升。文章指出，這些改善通常滯後於投資：“投資電腦並不會自動提升生產力增長；企業還需要重新組織其商業實踐。”同樣的滯後現象也曾發生在電力發明之後。

So not just is business value hard to measure, there's a time lag too. So maybe you can't measure the productivity of a team until a few years after a release of the software they were building.

因此，不僅商業價值難以衡量，還存在時間上的滯後現象。所以，可能要等到軟體發布幾年後，你才能真正衡量該團隊的生產力。

I can see why measuring productivity is so seductive. If we could do it we could assess software much more easily and objectively than we can now. But false measures only make things worse. This is somewhere I think we have to admit to our ignorance.

我可以理解為什麼衡量生產力如此具有吸引力。如果我們能做到，將比現在更容易、更客觀地評估軟體。然而，錯誤的衡量方法只會使情況變得更糟。我認為這是我們必須承認自身無知的領域之一。
