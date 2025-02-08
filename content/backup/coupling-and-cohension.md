---
date: '2025-02-08T16:05:34+08:00'
title: 'Coupling and Cohension'
tags: ["Kent Beck"]
---

[Source](https://tidyfirst.substack.com/p/coupling-and-cohesion)

> This is from 2009, a mere 4 years into my study of software design. Note that the description of cohesion is nowhere near as crisp as in _Tidy First?_. Being able to discuss cohesion clearly was what took me the next 10 years. The only way to be able to describe something well is to describe it badly 100 times.
> 
> 這是2009年的一篇文章，那時我才學習軟體設計四年。請注意，對於內聚性的描述遠不如《Tidy First?》一書中的清晰。能夠清楚地討論內聚性花了我接下來的十年時間。能夠很好地描述某件事情的唯一方法就是糟糕地描述它一百次。

I just finished a week of training at SKB Kontur in Ekaterinburg, Russia. We covered a lot of ground during the week–TDD, social principles of development, habits for agility. We ended the week talking about software design. During design day we tried to identify the best-designed software there (I recommend this exercise). Because the day was so experiential, we didn’t get to talk about all the design concepts I wanted to discuss. In particular, coupling and cohesion play a central role in the value of software design. As a kind of parting gift to the great group of programmers in the workshop (and because it will bug me if I don’t write it down), here is an introduction to coupling and cohesion.

我剛結束在俄羅斯葉卡捷琳堡的SKB Kontur進行的一週培訓。我們在這週涵蓋了很多內容——測試驅動開發（TDD）、開發的社會原則、敏捷習慣。我們以討論軟體設計結束了這週的課程。在設計日，我們試圖找出那裡設計最好的軟體（我推薦這個練習）。由於這一天的體驗性質，我們沒有談到我想要討論的所有設計概念。特別是耦合和內聚在軟體設計的價值中起著核心作用。作為對參加研討會的這群出色程序員的一種告別禮物（以及如果我不寫下來就會困擾我的原因），這裡介紹了耦合和內聚。

Yourdon and Constantine in their classic [Structured Design](https://web.archive.org/web/20140415191114/http://www.amazon.com/Structured-Design-Fundamentals-Discipline-Computer/dp/0138544719) identify cost minimization as the goal of software design. The cost of software is dominated by the cost of maintenance, the cost of maintenance is dominated by the cost of changes the ripple through the system, and effective software design minimizes the chance of changes propagating. Changes that touch a single element cost less and are more predictable than changes to one element that require changes to two more, and then three… The expected cost of change can be reduced by paying careful attention to two factors: coupling between elements and cohesion within elements.

Yourdon 和 Constantine 在他們經典的[《結構化設計》](https://web.archive.org/web/20140415191114/http://www.amazon.com/Structured-Design-Fundamentals-Discipline-Computer/dp/0138544719)中指出，軟體設計的目標是成本最小化。軟體的成本主要來自於維護成本，而維護成本主要來自於系統變更所帶來的成本。有效的軟體設計可以最大限度地降低變更傳播的機會。影響單一元素的變更成本較低，並且比影響一個元素需要改變兩個甚至更多元素的變更更加可預測。可以通過仔細關注兩個因素來降低變更的預期成本：元素之間的耦合和元素內的內聚。

(Software design also has a role in increasing or accelerating revenue, but revenue isn’t directly connected to coupling and cohesion so I will deal with this role later.)

（軟體設計在增加或加速收入方面也有作用，但收入與耦合和內聚無直接關係，因此我將稍後處理這個角色。）

## **Coupling**

Two elements are coupled to the degree that changes to one tend to require changes in another. For example, parallel class hierarchies are coupled if a class is added to one, because the other hierarchy will also require another class. Two systems communicating via a wire protocol are coupled with respect to protocol changes–if one system requires a change to the protocol the other will need to change as well. Coupling between elements is a conductor of change.

兩個元素之間的耦合程度取決於對其中一個的更改是否傾向於要求另一個也發生更改。例如，平行類層次結構是耦合的，如果在一個層次結構中添加一個類，另一個層次結構也將需要另一個類。通過線協議進行通信的兩個系統在協議更改方面是耦合的——如果一個系統需要更改協議，另一個系統也需要進行更改。元素之間的耦合是變更的傳導體。

![Coupling propagates change](https://substackcdn.com/image/fetch/w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd6131c95-ebe3-4497-9dda-1facf0fd8836_223x110.jpeg)

I talk about coupling (and cohesion) in terms of particular changes. This is not the standard definition. Coupling is generally described as a static property: these two elements are temporally coupled, for example, if the calling sequence between them is constrained. These static properties are only potential cost. If nothing ever triggers the coupling, the cost is never realized.

我以特定變更的術語來討論耦合（和內聚）。這不是標準的定義。耦合通常被描述為一種靜態屬性：如果兩個元素之間的調用順序受到約束，則它們是時間上的耦合。這些靜態屬性只是潛在的成本。如果沒有任何東西觸發耦合，則不會產生成本。

A system could be coupled to a particular vendor’s database by using vendor-specific features. A change to the database would require changes to the system. If the database never changes, though, then the coupling remains potential. Evaluating the cost of coupling precisely requires evaluating the set of changes that are actually required of the system. This can only be done a posteriori. Evaluating the cost prospectively requires estimating the probabilities of the kinds of change that would propagate across a relationship.

一個系統可能因使用特定供應商的數據庫特性而耦合於該供應商的數據庫。更改數據庫將需要更改系統。然而，如果數據庫從未更改，那麼耦合仍然是潛在的。準確評估耦合成本需要評估實際系統所需的變更集。這只能事後完成。前瞻性地評估成本需要估計各種可能在關係中傳播的變更類型的概率。

The relationship between coupling and change cost goes both ways. Changes that are likely to be expensive are less likely to be chosen. Breaking a coupling can open up the possibility for new kinds of change.

耦合和變更成本之間的關係是雙向的。可能代價高昂的變更不太可能被選擇。打破耦合可以開啟新型變更的可能性。

There is much more to say about the various kinds of coupling and the kinds of change that propagate across them, but that detail will have to await another post. The fundamental concept is that elements in a design should not be coupled with respect to the changes that actually take place. This keeps the cost of a change contained.

有更多話題要討論各種類型的耦合以及跨越它們的變更類型，但這些細節將留待另一篇文章中討論。基本概念是設計中的元素不應該就實際發生的變更而言是耦合的。這樣可以將變更的成本控制在範圍內。

Coupling measures the spread of a change across elements. Cohesion measures the cost of a change within an element. An element is cohesive to the degree that the entire element changes when the system needs to change.

耦合衡量變更在元素之間的傳播範圍。內聚衡量元素內變更的成本。一個元素的內聚程度取決於系統需要變更時該元素整體的變更程度。

An element can lack cohesion either by being too large or too small. An element that is too small, solving only part of a problem, will have to be coupled to elements solving the other parts of the problem. Changing the solution will require changing all the elements. An element that solves several problems will only be partly changed. This is riskier and more expensive than changing a whole element because first you need to figure out what part of the element should be changed and then you need to prove that the unchanged part of the element is truly unchanged. Cohesive elements, replaced in total, don’t incur these costs.

一個元素可以因過大或過小而缺乏內聚。一個過小的元素，只解決部分問題，將不得不與解決其他部分問題的元素耦合。更改解決方案將需要更改所有元素。一個解決多個問題的元素將只部分更改。這比整個元素的變更風險更高且成本更高，因為首先你需要弄清楚應更改元素的哪個部分，然後需要證明元素的未更改部分確實沒有變更。內聚元素被整體替換，不會產生這些成本。

![Cohesion](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F018b3f6c-f600-44b3-b896-a95c657f716e_398x149.jpeg)

(The strategy of isolating change is a way of inducing cohesion before making a change, for example extracting the part of a method that needs to change into its own method before making the change.)

（在進行更改之前，透過隔離變更來誘導內聚是一種策略，例如在更改之前將需要變更的方法部分提取到自己的方法中。）

## **Balance**

One of the things about design that makes it such a joy is that it requires balance. If elements are too large, each change will be more expensive than it needs to be. If elements are too small, changes will ripple across elements. And optimizing the design takes place against the backdrop of an unpredictable stream of changes.

設計的樂趣之一在於它需要平衡。如果元素過大，每次變更的成本將高於必要的成本。如果元素過小，變更將在元素之間傳播。優化設計是在不可預測的變更流背景下進行的。

Elements that are too large tend to multiply the cost of change: N * C. Change that ripples across elements can potentially be much more expensive: C ^ N. (This math is simplistic and not intended to be taken literally.) This suggests that the most care should be spent on reducing coupling. This is a bit puzzling as my practice is generally to make more smaller pieces. Perhaps I’m just confident of my ability reduce coupling.

過大的元素往往會成倍增加變更成本：N * C。而變更在元素之間傳播的成本可能要高得多：C ^ N。（這種數學是簡化的，不應被字面解讀。）這表明最應該關注的是降低耦合。這有點令人困惑，因為我的實踐通常是將更多小的部分組合在一起。也許我只是對自己降低耦合的能力充滿信心。

One challenge in design is to cheaply reduce coupling. If one element can be insulated from a likely change in another at reasonable cost, then it’s worth doing sooner rather than later. Breaking other forms of coupling will be more expensive and might be better defered until just before a triggering change. Again, it’s the imprecision of this analysis that makes design fun.

設計中的一個挑戰是以低成本減少耦合。如果一個元素可以以合理的成本與另一個元素的可能變更隔離，那麼值得儘早進行。打破其他形式的耦合將更加昂貴，可能最好推遲到觸發變更之前。再次強調，正是這種分析的不精確性讓設計變得有趣。

The unpredictability of changes renders it impossible to statically determine the “best” design for a system. There will always be some changes that ripple through the system. I speculate that the number of elements changed per change to the system follows a power law distribution. Careful attention to coupling can reduce the slope of the line describing the number of changes but cannot eliminate the distribution. This hypothesis needs experimental backing.

變更的不可預測性使得無法靜態地確定系統的“最佳”設計。總會有一些變更在系統中傳播。我推測每次對系統的變更所更改的元素數量遵循一種冪律分佈。仔細關注耦合可以降低描述變更數量的線條斜率，但無法消除這種分佈。這一假設需要實驗支持。

## **Further topics**

When I write about design, I notice that I tend to assume prerequisites that I haven’t yet written about and point to corollaries that I likewise haven’t covered yet. Here is a list of topics I should write about implied by this post:

當我寫到設計時，我注意到我傾向於假設一些前提條件，而這些前提條件我尚未寫過，同時指出一些我同樣尚未涵蓋的推論。以下是這篇文章暗示我應該撰寫的主題列表：

- Isolate change
- Design is beneficially relating elements
- Forms of coupling
- Cost and benefit of design (in particular the points of diminishing and reversing returns)
- Design fitness and its evolution
- Design to maximize revenue
- Design timing

- 隔離變更
- 設計是有益地關聯元素
- 耦合的形式
- 設計的成本和收益（特別是收益遞減和反轉的點）
- 設計適應性及其演變
- 最大化收入的設計
- 設計時機