---
date: '2025-02-08T16:06:09+08:00'
title: 'Forest and Desert'
tags: ["Kent Beck"]
---

[Source](https://tidyfirst.substack.com/p/forest-and-desert)

> This guest post is written by [Beth Andres-Beck](https://open.substack.com/users/14841068-beth-andres-beck?utm_source=mentions), following discussions we had preparing for our recent Øredev pair keynote (link to come).

“This architecture you’ve described sounds like a lush forest, but we are living in the desert. I don’t see how this will work here.”

這個架構聽起來像是一片茂密的森林，但我們身處沙漠之中。我看不出來這套做法如何能在這裡運作。

I am often confronted with skepticism, but seldom is it so constructively put.

我經常面對質疑，但很少有人能如此具有建設性地表達出來。

## Desert

In a software desert, we travel by force of will under an oppressive heat, with saddlebags stuffed to the brim. There is no room for error in the desert, no opportunity for good news, no time to revisit a place we have already been. It is very important that we know how long the journey will take, even when we aren’t sure that where we end up will be any better than where we started.

在軟體開發的沙漠裡，我們憑著意志力在酷熱之下前行，馱袋裡塞滿了負擔。在沙漠中沒有犯錯的空間，沒有好消息的機會，也沒有時間回頭檢視過去的路。我們必須確切知道這趟旅程會花多久，儘管我們不確定目的地是否真的比起點更好。

## Forest

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3276be6b-e6d3-4a2d-b7bf-c35155b2c4a0_2048x2048.png)

Some of us have experienced a software forest, once upon a time. There was a team that just worked. A company where things were built in small, safe steps and we architected as we went along. A time a whole group of people shared a clear goal and we were all going after it as best we knew how. A place where on call was no big deal because we didn’t have bugs to be paged about.

有些人曾經歷過軟體開發的森林。在那裡，團隊運作順暢，公司採取小步快跑的方式來建設系統，架構在發展中逐步演進。那是個目標清晰、整個團隊齊心協力向前邁進的時代。在那裡，輪班待命並不麻煩，因為根本沒有太多 Bug 需要修復。

When we talk about “building software”, we aren’t all talking about the same job. Building in the desert? Building in the forest?

當我們談論「開發軟體」時，彼此的工作環境可能大相逕庭——是在沙漠中開發？還是在森林裡建造？

## Which?

There is a lot of advice targeted at the desert. Write detailed design documents. Have senior engineers do more project management. Mandate multiple levels of review. Build microservices that align with autonomous team boundaries. Spend more time estimating. Optimize for your performance review. Rewrite what isn’t working. Create a sense of urgency.

有許多建議是針對沙漠環境的，例如撰寫詳盡的設計文件、讓資深工程師負責更多專案管理、強制多層級的審查機制，或是建立與獨立團隊邊界對應的微服務。此外，花更多時間進行預估、優化績效考核、重寫無法運作的部分，甚至營造緊迫感，都是在沙漠中常見的做法。

That advice doesn’t make sense in the forest.

然而，這些方法在森林裡並不適用。

There is also advice targeted at the forest. Have developers write tests. Practice collective code ownership. Build incrementally. Refactor. Host guilds. Pair. Release continuously. Work on smaller slices. Run experiments. Stop setting deadlines. Host weekly demos.

相對地，森林環境的建議則包括讓開發人員撰寫測試、實踐集體代碼所有權、逐步建構系統、重構、組織技術分享會，並透過配對編程和持續發布來提高開發效率。團隊會將工作拆分成更小的單位，執行試驗，停止設定死線，並透過每週 Demo 來同步進展。

Those techniques don’t have the intended effect in a desert.

然而，這些方法在沙漠中往往無法發揮預期的效果。

## Here To There

There is a path from the desert to the forest, a series of small, safe steps a community can take to terraform the landscape. No developer can do it alone, and no amount of technology will get us there, but it is possible.

從沙漠走向森林，是一條可以透過一系列小而安全的步驟來改變環境的道路。沒有哪位開發者能單獨完成這項轉變，也沒有任何技術能夠單靠自身促成改變，但這是有可能達成的。

Before we start that journey, it is worth understanding why a company has built a desert. Sometimes it is just an accident. Other times the fruits of the forest aren’t valuable to the executive team, whereas the fruits of the desert are.

然而，在踏上這段旅程之前，我們需要理解為何一家公司會建構出這樣的沙漠環境。有時這只是意外造成的，但有時候，森林的果實對於管理層而言並不具吸引力，而沙漠的產出才符合他們的需求。

If executives don’t care about user trust, consistent design isn’t going to be useful. If bugs don’t have any impact on the bottom line, there is no reason to avoid them. When the challenge is legibility and control of processes rather than how to deliver useful software to the users, delivering useful software more efficiently doesn’t earn us anything. Trying to write clean code in a company getting anything at all done with a minimum of resources just makes developers sound self-righteous. Worse, it convinces programmers that forest techniques don’t work, can’t work, and are never worth doing.

如果管理層不在乎使用者的信任，那麼一致性的設計就沒有任何價值。如果 Bug 並不影響公司的營收，那麼就沒有理由去避免它們。當企業的核心挑戰是讓流程清晰可控，而不是如何交付有價值的軟體時，更高效地開發產品並不會帶來任何實質收益。在一個只求用最少資源完成工作的公司裡，試圖撰寫乾淨的代碼只會讓開發者顯得自以為是。更糟的是，這會讓程式設計師相信森林的方法行不通、永遠行不通，也不值得嘗試。

## Staff+

Executive teams shouldn’t have to understand how software developers do their jobs to get the outcomes they want. That is where Staff+ Engineers come in. Our job is to partner with business leaders to understand the goals, and then cultivate the landscape that can deliver on them.

管理層不需要理解軟體開發的細節，就能獲得他們想要的結果。而這正是資深工程師（Staff+ Engineer）的價值所在。我們的工作是與業務領導層合作，理解公司的目標，並打造能夠實現這些目標的環境。