---
date: '2025-02-08T21:43:00+08:00'
title: 'Story Points Revisited'
tags: ["Ron Jeffries"]
---

[Source](https://ronjeffries.com/articles/019-01ff/story-points/Index.html)

> I like to say that I may have invented story points, and if I did, I’m sorry now. Let’s explore my current thinking on story points. At least one of us is interested in what I think.
> 
> 我可能發明了故事點，如果真是如此，我現在感到抱歉。讓我們探討一下我對故事點的最新想法。至少我們中有一個人對我的想法感興趣。

Stories, of course, are an XP idea, not a Scrum idea. Somehow, Scrum practitioners have adopted the idea. Even though the official Scrum Guide refers to backlog items, having backlog items be User Stories is a common Scrum practice.

當然，故事是極限編程（XP）的概念，不是 Scrum 的概念。不知怎的，Scrum的實踐者採用了這個概念。即使官方的 Scrum 指南提到的是待辦事項，將待辦事項視為用戶故事也是 Scrum 中的常見做法。

At least to the limited extent that they get it right. I’ve written elsewhere about the general use of stories as she should be done. Here we’ll talk about “Story Points”.

至少在某種程度上，他們是正確的。我在其他地方寫過關於如何正確使用故事的內容。在這裡，我們將談談“故事點”。

In XP, stories were originally estimated in time: the time it would take to implement the story. We quickly went to what we called “Ideal Days”, which was informally described as how long it would take a pair to do it if the bastards would just leave you alone. We multiplied Ideal Days by a “load factor” to convert to actual implementation time. Load factor tended to be about three: three real days to get an Ideal Day’s work done.

在XP中，故事最初是以時間估算的：完成這個故事需要的時間。我們很快轉向了所謂的“理想天數”，這個概念的非正式描述是：如果沒有人打擾你，完成這個故事需要的時間。然後我們將理想天數乘以一個“負載係數”來轉換為實際的實施時間。負載係數通常是三倍：完成一天的理想工作需要三天的真實時間。

We spoke of our estimates in days, usually leaving “ideal” out. The result was that our stakeholders were often confused by how it could keep taking three days to get a day’s work done, or, looking at the other side of the coin, why we couldn’t do 50 “days” of work in three weeks.

我們在談論估算時用的是天數，通常不加“理想”這個詞。結果是，我們的利益相關者經常困惑於為什麼完成一天的工作需要三天，或者從另一個角度來看，為什麼我們不能在三週內完成50天的工作。

So, as I recall it, we started calling our “ideal days” just “points”. So a story would be estimated at three points, which meant it would take about nine days to complete. And we really only used the points to decide how much work to take into an iteration anyway, so if we said it was about 20 points, no one really objected.

所以，據我回憶，我們開始把“理想天數”稱為“點數”。這樣一來，一個故事估算為三點，意味著大約需要九天來完成。我們基本上只是用這些點數來決定在一個迭代中可以接受多少工作量，所以當我們說大約是20點時，沒有人真正反對。

I may have made the name-changing suggestion. If I did, I’m sorry now. Here are some of my current thoughts on the topic, from an email to my colleague Simon, who asked:

我可能提出了改名字的建議。如果是我，我現在感到抱歉。以下是我對這個話題的一些最新想法，來自我寫給同事Simon的一封電子郵件，他問：

> Do you really regret they were invented, or do you simply deplore their misuse when relative sizing is not properly understood?
> 
> 你真的後悔它們被發明出來，還是只是對相對大小的誤解感到遺憾？

I replied that

- I certainly deplore their misuse;
- I think using them to predict “when we’ll be done” is at best a weak idea;
- I think tracking how actuals compare with estimates is at best wasteful;
- I think comparing teams on quality of estimates or velocity is harmful.

- 我當然對它們的誤用感到遺憾；
- 我認為用它們來預測“我們什麼時候完成”這種想法充其量是薄弱的；
- 我認為追踪實際情況與估算的比較充其量是浪費的；
- 我認為比較團隊的估算質量或速度是有害的。

Let’s look a bit more deeply.

讓我們更深入地看一下。

Some approaches to “Agile” actually recommend normalizing story points across teams, in the name of easier planning. While this seems sensible enough on the surface, it’s too easy to fall into the trap of comparing teams, and, too often, organizations do that.

一些“敏捷”方法實際上建議在團隊之間統一故事點，以便於計劃。雖然這在表面上看起來是合理的，但很容易陷入比較團隊的陷阱，並且這種情況在組織中經常發生。

## Comparing

First of all, even if they “look the same”, each team has its own skills and works in its own environment. So if they look at two stories that seem the same, and one team says it’s a two and the other one says it’s a six, that’s just not very interesting, and it’s certainly not a useful way to compare teams.

首先，即使它們“看起來一樣”，每個團隊都有自己的技能，並在自己的環境中工作。因此，如果他們看兩個看起來相似的故事，一個團隊說是兩點，另一個團隊說是六點，那就不太有趣了，當然也不是比較團隊的有用方法。

Now, you and I, seeing that situation, would approach it with curiosity, first whether the situations were similar enough to compare, and then to explore whether the team with the higher estimate needed some kind of help that we could provide. That would be good. Implicitly or explicitly concluding that the “slower” team was inferior or messing up in some way – that would be very bad, but it is unfortunately common.

你和我看到這種情況時，會先好奇這些情況是否足夠相似可以比較，然後探究估算較高的團隊是否需要我們可以提供的幫助。這會很好。隱含或明確地得出“較慢”的團隊低人一等或以某種方式出錯的結論——這會非常糟糕，但這種情況不幸地很常見。

I think given two teams producing things, it’s an irresistible temptation, for many managers, to compare them. I think it’s irresistible enough that I’d drop the notion of story points, and even the notion of estimating stories at all, where possible. We’ll come back to the question of how to work with fewer estimates, and there are other [articles here addressing that as well.](https://ronjeffries.com/categories/estimation)

我認為，對於許多管理者來說，看到兩個團隊產出時，很難抗拒比較它們。我認為這種誘惑足夠強烈，以至於我會放棄故事點的概念，甚至在可能的情況下完全放棄對故事的估算。我們將回到如何使用更少估算的問題，還有其他[文章在這裡討論這個問題。](https://ronjeffries.com/categories/estimation)

## Tracking

To many managers, the existence of an estimate implies the existence of an “actual”, and means that you should compare estimates to actuals, and make sure that estimates and actuals match up. When they don’t, that means people should learn to estimate better.

對於許多管理者來說，估算的存在意味著“實際”的存在，並且意味着應該將估算與實際進行比較，確保估算與實際相符。當它們不匹配時，這意味着人們應該學習如何更好地進行估算。

To me, the important thing in Real Agile is to pick the next few things to do, and do them promptly. The key question is to find the most valuable things to do, and to do them quickly. Doing them quickly comes down to doing small slices of high value, and iterating rapidly. Story cost estimation doesn’t help much with that, if at all.

對我來說，在真正的敏捷中，重要的是選擇接下來要做的幾件事情，並迅速完成它們。關鍵問題是找出最有價值的事情去做，並快速完成。快速完成的關鍵在於做出高價值的小切片，並迅速迭代。故事成本估算在這方面幫助不大，甚至根本沒有幫助。

So if the existence of an estimate causes management to take their eye off the ball of value and instead focus on improving estimates, it takes attention from the central purpose, which is to deliver real value quickly.

因此，如果估算的存在使管理層將注意力從價值上轉移到改進估算上，那麼它就偏離了核心目標，即迅速交付真正的價值。

This makes me think that estimation, be it in points or time, is to be avoided.

這讓我認為，無論是用點數還是時間進行估算，都應該避免。

## Pressure

Related to the estimate / actuals concern is the natural pressure of management to want “more”. However much the team is getting done, it’s not enough. More, more, more.

與估算/實際相關的是管理層想要“更多”的自然壓力。無論團隊完成了多少工作，都不夠。更多，更多，更多。

The best way to deliver value isn’t more, more, more, it’s to do small valuable things frequently. If instead of estimating stories, we slice them down to “small enough”, we can come to a smooth flow of value, delivering all the time.

交付價值的最佳方式不是更多，更多，更多，而是頻繁地完成小而有價值的事情。如果我們不是估算故事，而是將它們切成“足夠小”的部分，我們可以實現價值的平穩流動，隨時交付。

The focus on more gets in the way of increasing value. Increasing pressure to do more almost inevitably has a bad result: the team tries to go faster, and wind up skimping on code quality and on tests. They soon begin shipping more defects, slowing down because of the increased rework to fix the defects, and slowing down even more because the code quality rapidly declines. Things get worse and worse, pressure increases, and it becomes a race to disaster.

對“更多”的專注會妨礙價值的增加。增加完成更多的壓力幾乎不可避免地會產生不良後果：團隊試圖加快速度，結果在代碼質量和測試上偷工減料。他們很快就會發現更多的缺陷，因為需要增加返工來修復這些缺陷而變慢，並且隨著代碼質量迅速下降，速度進一步放慢。情況越來越糟，壓力增加，最終成為災難的競賽。

Because estimates are at least implicated in the application of undue pressure, I’d prefer to avoid them.

因為估算至少在施加過度壓力中起了一定作用，我更願意避免它們。

I’ll go further: I’d prefer to avoid iteration or Sprint planning entirely. We wouldn’t work to fill up a budget for the next few weeks: we’d work to have a list of the few most important next things to do.

我會更進一步：我更願意完全避免迭代或Sprint計劃。我們不會為未來幾週的預算填滿工作量，而是列出幾件最重要的事情去做。

## Predicting Done

It is common practice to make a list of essential features, think about them for a while, and then decide that they define the next release of our product. The next question, of course, is “when will all this be done?”

通常的做法是列出基本功能，考慮一段時間，然後決定它們定義了我們產品的下一個版本。接下來的問題當然是“什麼時候能完成這一切？”

The answer is that no one knows. We could do a lot of work to improve our not knowing, and in some areas and at some times some of that is worth doing, such as when there’s a large contract waiting to be bid. But when we’re in the business of developing solutions for internal or external customers, we do best to provide small amounts of value frequently, not wait for Big Bang releases that seem often to recede indefinitely into the future.

答案是沒有人知道。我們可以做很多工作來改善我們的無知，在某些領域和某些時候，這些工作是值得的，比如當有一個大型合同等待投標時。但是，當我們在為內部或外部客戶開發解決方案時，最好是經常提供少量的價值，而不是等待那些似乎無限推遲的大規模發布。

It’s far better to pick a close-in date for the next release to customers, and pick as much good stuff into that release as possible. Estimating, be it in story points or gummi bears or even time, gets in the way of this. Where possible, in my opinion, it’s best avoided.

更好的做法是選擇一個接近的日期作為下次發布的目標，並儘可能多地將好東西納入該版本中。估算，無論是故事點、糖果還是時間，都會妨礙這一過程。在可能的情況下，我認為最好避免。

## Slicing

So the question comes up, if you don’t like story estimation, what do you like? Well, I like story slicing, which is the practice of taking larger stories and slicing them down into smaller ones, each of as high value as possible, but requiring very little time to get done, ideally less than a day, maybe just a couple of hours.

所以問題來了，如果你不喜歡故事估算，那你喜歡什麼？我喜歡故事切分，這是一種將較大的故事切分為較小的故事的做法，每個故事都儘可能高價值，但需要非常少的時間完成，理想情況下不到一天，也許只有幾個小時。

Now, I don’t care to quibble with you about whether there must be some kind of estimation going on in slicing. If you or your team estimate in your heads and never tell anyone, then the problems with estimates, whether they are story points or time, aren’t likely to arise. And certainly, knowing the difference between “probably small enough” and “probably not small enough” isn’t the same as knowing the difference between “three days” and “one day”.

現在，我不想跟你爭論是否必須進行某種估算。如果你或你的團隊在頭腦中進行估算而從不告訴任何人，那麼估算的問題，無論是故事點還是時間，都不太可能出現。而且，確定“可能足夠小”和“可能不夠小”之間的區別並不等同於知道“三天”和“一天”之間的區別。

Plus, there’s a trick. I mentioned it in [Getting Small Stories](https://ronjeffries.com/xprog/articles/getting-small-stories/) and [Slicing, Estimating, Trimming](https://ronjeffries.com/articles/015-jul/slicing/). I learned it from Neil Killick: slice stories down until they just need a single acceptance test. With a little practice that gets things right down to a good size.

此外，還有一個技巧。我在[獲得小故事](https://ronjeffries.com/xprog/articles/getting-small-stories/)和[切分、估算、修剪](https://ronjeffries.com/articles/015-jul/slicing/)中提到過。我從Neil Killick那裡學到的：將故事切分到只需要一個驗收測試。稍加練習就能把事情控制在合適的規模。

And of course there are other articles on the subject of estimation, just click the link at the top of the page for more than you ever wanted to know.

當然，還有其他關於估算的文章，只需點擊頁面頂部的鏈接，就能看到比你想知道的更多的內容。

## Predicting the future

But … isn’t there some legitimate need to know how long a product release will take, and aren’t estimates necessary for that? Well, perhaps, but perhaps not story estimates. You probably won’t even _have_ your requirements down to the story level, and if you do, they are likely too bulky and largely waste.

但是……難道真的不需要知道一個產品發布需要多長時間嗎？難道估算不是必須的嗎？也許吧，但也許不是故事估算。你甚至可能沒有將需求細化到故事層級，如果你有，它們可能太龐大而且大部分是浪費。

Of course, if you have to do it, go ahead and do it. Whatever I’d do or my theories about what you should do, are just ideas. In the end you have to do whatever it takes to succeed in your situation. But there is something that I think is better.

當然，如果你必須這樣做，那就去做。無論我會做什麼，或者我關於你應該做什麼的理論，都只是一些想法。最終，你必須做任何能在你的情況下成功的事情。但我認為有些方法會更好。

First, think about one or a few important capabilities for the next release. Talk about what the problems are that they solve, and what software might help solve them. Talk about the simplest capabilities that might help a bit. We don’t have to solve everything: often if we can give a bit of help, that’s enough to get things rolling.

首先，考慮下一次發布的一項或幾項重要功能。討論它們解決了哪些問題，以及哪種軟件可能有助於解決這些問題。討論可能有一點幫助的最簡單的功能。我們不必解決所有問題：通常，如果我們能提供一點幫助，就足夠讓事情順利進行了。

Second, think about a close-in deadline such that you feel you could get some good capabilities built by then. Set the deadline and get to work.

其次，考慮一個近在咫尺的截止日期，讓你覺得可以在此之前建成一些好的功能。設置截止日期並開始工作。

Third, slice off thin slices of the important capabilities and do them. You should be able to get them down to a day or less pretty easily. Work only on the most important next bits: don’t try to fill out the first capability all the way to the tiniest frill. You’re trying to get into a frame of mind where you think “If we just did this one little thing, Customer Jack could actually use this”. Then, do that little thing and let Customer Jack try it. We want to move as quickly as we can to continuous delivery of value.

第三，將重要功能分割成薄薄的切片並完成它們。你應該能夠輕鬆地將它們縮短到一天或更少的時間。只專注於最重要的下一步工作：不要嘗試將第一項功能從頭到尾填充到最細小的裝飾上。你要進入一種心態，即“如果我們只做這一點點事情，客戶Jack就可以實際使用這個功能”。然後，完成這一點點事情，讓客戶Jack試試看。我們希望盡可能快地進入價值的持續交付。

We want to make the value of what we’re doing so visible that our Product Owner and other stakeholders can’t wait to get it out there. Then … we’ll be doing the right thing, with, or without, story estimates.

我們希望讓我們所做事情的價值變得如此明顯，以至於我們的產品負責人和其他利益相關者迫不及待地想要將其推出市場。這樣……我們就會做對的事情，無論是否有故事估算。

## Summing up

Well, if I did invent story points, I’m probably a little sorry now, but not very sorry. I do think that they are frequently misused and that we can avoid many pitfalls by not using story estimates at all. If they’re not providing great value to your team or company, I’d advise dropping them on the grounds that they are waste. If, on the other hand, you just love them, well, carry on!

如果我確實發明了故事點，我現在可能會有一點點後悔，但不會非常後悔。我確實認為它們經常被濫用，我們可以通過完全不使用故事估算來避免許多陷阱。如果它們沒有為你的團隊或公司帶來很大的價值，我建議將它們視為浪費並放棄它們。另一方面，如果你就是喜歡它們，那就繼續使用吧！kkk
