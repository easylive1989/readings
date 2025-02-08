---
date: '2025-02-08T21:39:18+08:00'
title: 'Is Design Dead'
tags: ["Martin Fowler"]
---

[Source](https://martinfowler.com/articles/designDead.html)

[Extreme Programming](http://www.extremeprogramming.org/) (XP) challenges many of the common assumptions about software development. Of these one of the most controversial is its rejection of significant effort in up-front design, in favor of a more evolutionary approach. To its detractors this is a return to “code and fix” development - usually derided as hacking. To its fans it is often seen as a rejection of design techniques (such as the UML), principles and patterns. Don't worry about design, if you listen to your code a good design will appear.

極限編程（Extreme Programming, XP）挑戰了許多關於軟體開發的常見假設。其中最具爭議的一點是，它拒絕在前期投入大量精力於設計，而是傾向於採用更具演進性的方式。批評者認為這是一種回到“編碼與修復”開發方式的退步——通常被視為隨意的駭客行為。而對支持者而言，這常被看作是對設計技術（例如 UML）、設計原則與模式的拒斥。不用擔心設計，只要傾聽你的程式碼，一個良好的設計自然會浮現。

I find myself at the center of this argument. Much of my career has involved graphical design languages - the Unified Modeling Language (UML) and its forerunners - and in patterns. Indeed I've written books on both the UML and patterns. Does my embrace of XP mean I recant all of what I've written on these subjects, cleansing my mind of all such counter-revolutionary notions?

我發現自己正處於這場爭論的中心。我的職業生涯大部分時間都涉及圖形化設計語言——統一建模語言（UML）及其前身——以及設計模式。事實上，我曾撰寫過關於 UML 和設計模式的書。那麼，我對 XP 的擁護是否意味著我否定了我在這些主題上所寫的一切，並將這些“反革命”觀念從我的思維中徹底清除呢？

Well I'm not going to expect that I can leave you dangling on the hook of dramatic tension. The short answer is no. The long answer is the rest of this paper.

嗯，我不會讓你一直懸在這種戲劇性的緊張感中。簡短的回答是：不。詳細的回答則是這篇文章的其餘部分。

## Planned and Evolutionary Design

For this paper I'm going to describe two styles how design is done in software development. Perhaps the most common is evolutionary design. Essentially evolutionary design means that the design of the system grows as the system is implemented. Design is part of the programming processes and as the program evolves the design changes.

在這篇文章中，我將描述兩種軟體開發中進行設計的方式。也許最常見的是演進式設計。基本上，演進式設計意味著系統的設計隨著系統的實現而逐步發展。設計是編程過程的一部分，隨著程式的演進，設計也會隨之改變。

In its common usage, evolutionary design is a disaster. The design ends up being the aggregation of a bunch of ad-hoc tactical decisions, each of which makes the code harder to alter. In many ways you might argue this is no design, certainly it usually leads to a poor design. As Kent puts it, design is there to enable you to keep changing the software easily in the long term. As design deteriorates, so does your ability to make changes effectively. You have the state of software entropy, over time the design gets worse and worse. Not only does this make the software harder to change, it also makes bugs both easier to breed and harder to find and safely kill. This is the “code and fix” nightmare, where the bugs become exponentially more expensive to fix as the project goes on.

在一般情況下，演進式設計常被視為一場災難。最終的設計往往只是一系列臨時策略決策的堆砌，而這些決策使程式碼變得越來越難以修改。從某種意義上來說，這甚至稱不上是設計，並且通常會導致糟糕的設計。正如 Kent 所說，設計的目的是讓軟體在長期內能夠輕鬆地持續改變。隨著設計的惡化，你有效進行修改的能力也隨之下降。這種情況被稱為軟體熵，隨著時間推移，設計會越來越差。這不僅讓軟體更難修改，還讓漏洞更容易滋生，且更難發現和安全地解決。這就是“編碼與修復”的噩夢，隨著專案的推進，修復漏洞的成本呈指數級增長。

Planned Design is a counter to this, and contains a notion born from other branches of engineering. If you want to build a doghouse, you can just get some wood together and get a rough shape. However if you want to build a skyscraper, you can't work that way - it'll just collapse before you even get half way up. So you begin with engineering drawings, done in an engineering office like the one my wife works at in downtown Boston. As she does the design she figures out all the issues, partly by mathematical analysis, but mostly by using building codes. Building codes are rules about how you design structures based on experience of what works (and some underlying math). Once the design is done, then her engineering company can hand the design off to another company that builds it.

計劃式設計是針對上述問題的一種對策，這種理念源自其他工程領域的經驗。如果你要建造一個狗屋，你可以隨便找些木材拼湊出一個大致的形狀。然而，如果你要建造一棟摩天大樓，這種方式就行不通了——它在建到一半之前就會倒塌。所以，你必須從工程圖開始，這些圖是在像我妻子在波士頓市中心工作的那類工程辦公室裡繪製的。在她設計的過程中，會通過數學分析部分地解決問題，但主要是依靠建築規範。建築規範是基於實踐經驗（以及一些基礎數學）制定的結構設計規則。一旦設計完成，她所在的工程公司就可以將設計交給另一家負責施工的公司來實現。

Planned design in software should work the same way. Designers think out the big issues in advance. They don't need to write code because they aren't building the software, they are designing it. So they can use a design technique like the UML that gets away from some of the details of programming and allows the designers to work at a more abstract level. Once the design is done they can hand it off to a separate group (or even a separate company) to build. Since the designers are thinking on a larger scale, they can avoid the series of tactical decisions that lead to software entropy. The programmers can follow the direction of the design and, providing they follow the design, have a well built system.

軟體中的計劃式設計應該以相同的方式運作。設計師會事先思考重大問題。他們不需要編寫程式碼，因為他們不是在構建軟體，而是在設計軟體。因此，他們可以使用像 UML 這樣的設計技術，避開程式細節，讓設計師能夠在更抽象的層次上工作。一旦設計完成，他們可以將設計交給另一個團隊（甚至是另一家公司）來實現。由於設計師專注於更大的規模，他們可以避免一系列導致軟體熵的策略性決策。程式員只需遵循設計的方向，並且只要按照設計進行，就能構建出一個結構良好的系統。

Now the planned design approach has been around since the 70s, and lots of people have used it. It is better in many ways than code and fix evolutionary design. But it has some faults. The first fault is that it's impossible to think through all the issues that you need to deal with when you are programming. So it's inevitable that when programming you will find things that question the design. However if the designers are done, moved onto another project, what happens? The programmers start coding around the design and entropy sets in. Even if the designer isn't gone, it takes time to sort out the design issues, change the drawings, and then alter the code. There's usually a quicker fix and time pressure. Hence entropy (again).

計劃式設計自 1970 年代起就已經存在，並被許多人使用。與“編碼與修復”式的演進設計相比，它在許多方面更優越。然而，它也存在一些缺陷。第一個缺陷是，在編程時，你無法事先完全預想到所有需要解決的問題。因此，在編程過程中，你必然會發現一些問題挑戰原有的設計。然而，如果設計師已經完成工作並投入到其他項目中，那該怎麼辦呢？程式員會開始繞過設計進行編碼，最終導致熵增。即使設計師還在，也需要花時間來解決設計問題、更改設計圖，然後再調整程式碼。然而通常會有更快的修補方法，加上時間壓力的影響，最終又導致了熵增（再次）。

Furthermore there's often a cultural problem. Designers are made designers due to skill and experience, but they are so busy working on designs they don't get much time to code any more. However the tools and materials of software development change at a rapid rate. When you no longer code not just can you miss out on changes that occur with this technological flux, you also lose the respect of those who do code.

此外，還經常存在一個文化問題。設計師之所以成為設計師，是因為他們的技術和經驗，但由於忙於設計工作，他們通常已經沒有太多時間進行編程。然而，軟體開發的工具和技術變化速度極快。當你不再編程時，不僅可能錯過這些技術變化帶來的改進，還可能失去仍在編程的人的尊重。

This tension between builders and designers happens in building too, but it's more intense in software. It's intense because there is a key difference. In building there is a clearer division in skills between those who design and those who build, but in software that's less the case. Any programmer working in high design environments needs to be very skilled. Skilled enough to question the designer's designs, especially when the designer is less knowledgeable about the day to day realities of the development platform.

這種建造者與設計師之間的緊張關係在建築領域也存在，但在軟體開發中更為強烈。其原因在於一個關鍵的差異：在建築中，設計者與建造者的技能劃分較為清晰，而在軟體領域，這種界限則不那麼明顯。在高設計環境中工作的程式員需要非常熟練，熟練到能夠質疑設計師的設計，尤其是在設計師對開發平台的日常現實不那麼熟悉的情況下。

Now these issues could be fixed. Maybe we can deal with the human tension. Maybe we can get designers skillful enough to deal with most issues and have a process disciplined enough to change the drawings. There's still another problem: changing requirements. Changing requirements are the number one big issue that causes headaches in software projects that I run into.

這些問題或許可以被解決。也許我們可以處理人員之間的緊張關係，也許我們可以讓設計師具備足夠的技能來解決大多數問題，並建立足夠嚴謹的流程來修改設計圖。然而，還有另一個問題：需求的變更。需求變更是我在軟體專案中遇到的最主要且令人頭痛的問題。

One way to deal with changing requirements is to build flexibility into the design so that you can easily change it as the requirements change. However this requires insight into what kind of changes you expect. A design can be planned to deal with areas of volatility, but while that will help for foreseen requirements changes, it won't help (and can hurt) for unforeseen changes. So you have to understand the requirements well enough to separate the volatile areas, and my observation is that this is very hard.

應對需求變更的一種方法是將靈活性內建於設計中，這樣當需求變更時，設計也能輕鬆調整。然而，這需要對可能發生的變更有深入的洞察。設計可以針對不穩定的區域進行規劃，但這只能應對已預見的需求變更，對於無法預見的變更則無能為力（甚至可能適得其反）。因此，你必須對需求有足夠的理解，以便識別並區分那些不穩定的區域，而我的觀察是，做到這一點非常困難。

Now some of these requirements problems are due to not understanding requirements clearly enough. So a lot of people focus on requirements engineering processes to get better requirements in the hope that this will prevent the need to change the design later on. But even this direction is one that may not lead to a cure. Many unforeseen requirements changes occur due to changes in the business. Those can't be prevented, however careful your requirements engineering process.

有些需求問題是因為對需求的理解不夠清晰。因此，許多人專注於需求工程流程，試圖獲得更清晰的需求，希望藉此避免日後修改設計的必要。然而，即使採取這種方法，也未必能徹底解決問題。許多無法預見的需求變更是由於業務本身的變化引起的。無論你的需求工程流程多麼謹慎，這些變化都是無法預防的。

So all this makes planned design sound impossible. Certainly they are big challenges. But I'm not inclined to claim that planned design is worse than evolutionary design as it is most commonly practiced in a “code and fix” manner. Indeed I prefer planned design to “code and fix”. However I'm aware of the problems of planned design and am seeking a new direction.

所有這些問題使計劃式設計聽起來似乎是不可能實現的。這些確實是巨大的挑戰。但我並不傾向於認為計劃式設計比以“編碼與修復”方式進行的演進式設計更糟。事實上，我更偏好計劃式設計，而非“編碼與修復”。然而，我也意識到計劃式設計的問題，並正在尋求一個新的方向。

## The Enabling Practices of XP

XP is controversial for many reasons, but one of the key red flags in XP is that it advocates evolutionary design rather than planned design. As we know, evolutionary design can't possibly work due to ad hoc design decisions and software entropy.

XP 因為許多原因而充滿爭議，其中一個關鍵的警訊是，它提倡演進式設計而非計劃式設計。正如我們所知，演進式設計由於臨時性設計決策和軟體熵的影響，根本不可能成功運作。

At the core of understanding this argument is the software change curve. The change curve says that as the project runs, it becomes exponentially more expensive to make changes. The change curve is usually expressed in terms of phases “a change made in analysis for $1 would cost thousands to fix in production”. This is ironic as most projects still work in an ad-hoc process that doesn't have an analysis phase, but the exponentiation is still there. The exponential change curve means that evolutionary design cannot possibly work. It also conveys why planned design must be done carefully because any mistakes in planned design face the same exponentiation.

要理解這個論點的核心，需要了解軟體的變更曲線。變更曲線表明，隨著專案的進行，進行變更的成本將以指數方式增加。這條曲線通常以階段的形式表達，例如：“在分析階段進行的每 1 美元變更，到了生產階段可能需要數千美元來修正。” 諷刺的是，大多數專案仍然採用沒有分析階段的臨時性流程，但指數增長的成本依然存在。這種指數性的變更曲線意味著演進式設計根本無法運作。它也說明了為何計劃式設計必須謹慎執行，因為計劃設計中的任何錯誤同樣會面臨指數增長的成本問題。

The fundamental assumption underlying XP is that it is possible to flatten the change curve enough to make evolutionary design work. This flattening is both enabled by XP and exploited by XP. This is part of the coupling of the XP practices: specifically you can't do those parts of XP that exploit the flattened curve without doing those things that enable the flattening. This is a common source of the controversy over XP. Many people criticize the exploitation without understanding the enabling. Often the criticisms stem from critics' own experience where they didn't do the enabling practices that allow the exploiting practices to work. As a result they got burned and when they see XP they remember the fire.

XP 的基本假設是，可以通過足夠地平坦化變更曲線，使演進式設計得以運作。這種平坦化既是由 XP 的實踐促成的，也是被 XP 所利用的。這體現了 XP 各種實踐之間的緊密耦合性：具體來說，如果不採取那些促成變更曲線平坦化的措施，就無法有效實施那些利用平坦化曲線的實踐。這正是 XP 爭議的一個常見來源。許多人批評 XP 的利用部分，卻沒有理解支撐這些利用的措施。通常，這些批評源於批評者自身的經驗——他們沒有採取那些使利用措施有效運作的支撐性實踐，結果導致失敗。而當他們看到 XP 時，就會想起那些失敗的教訓。

There are many parts to the enabling practices. At the core are the practices of Testing, and Continuous Integration. Without the safety provided by testing the rest of XP would be impossible. Continuous Integration is necessary to keep the team in sync, so that you can make a change and not be worried about integrating it with other people. Together these practices can have a big effect on the change curve. I was reminded of this again here at Thoughtworks. Introducing testing and continuous integration had a marked improvement on the development effort. Certainly enough to seriously question the XP assertion that you need all the practices to get a big improvement.

促成性實踐包含許多部分，其中核心是**測試**和**持續整合**的實踐。沒有測試所提供的安全保障，XP 的其餘部分就無法實現。**持續整合**則是保持團隊同步的必要手段，這樣你可以進行更改，而不用擔心與其他人的整合問題。這兩種實踐結合在一起，對變更曲線能產生顯著的影響。我在 Thoughtworks 再次見證了這一點。引入測試和持續整合後，開發效率明顯提高。這種改善的程度足以讓人認真質疑 XP 的一個主張：是否真的需要所有的實踐才能實現顯著的提升。

Refactoring has a similar effect. People who refactor their code in the disciplined manner suggested by XP find a significant difference in their effectiveness compared to doing looser, more ad-hoc restructuring. That was certainly my experience once Kent had taught me to refactor properly. After all, only such a strong change would have motivated me to write a whole book about it.

促成性實踐包含許多部分，其中核心是**測試**和**持續整合**的實踐。沒有測試所提供的安全保障，XP 的其餘部分就無法實現。**持續整合**則是保持團隊同步的必要手段，這樣你可以進行更改，而不用擔心與其他人的整合問題。這兩種實踐結合在一起，對變更曲線能產生顯著的影響。我在 Thoughtworks 再次見證了這一點。引入測試和持續整合後，開發效率明顯提高。這種改善的程度足以讓人認真質疑 XP 的一個主張：是否真的需要所有的實踐才能實現顯著的提升。

Jim Highsmith, in his excellent [summary of XP](http://www.cutter.com/ead/ead0002.html), uses the analogy of a set of scales. In one tray is planned design, the other is refactoring. In more traditional approaches planned design dominates because the assumption is that you can't change your mind later. As the cost of change lowers then you can do more of your design later as refactoring. Planned design does not go away completely, but there is now a balance of two design approaches to work with. For me it feels like that before refactoring I was doing all my design one-handed.

Jim Highsmith 在他那篇出色的 [XP 總結](http://www.cutter.com/ead/ead0002.html) 中，用天平來做比喻。一邊是計劃式設計，另一邊是重構。在更傳統的方法中，計劃式設計占據主導地位，因為假設後續無法輕易改變。然而，隨著變更成本的降低，你可以將更多的設計工作留到後期，通過重構來完成。計劃式設計並未完全消失，而是與==重構這兩種設計方式達成了一種平衡==。對我而言，感覺在有了重構之前，我的設計就像是單手操作一樣。

These enabling practices of continuous integration, testing, and refactoring, provide a new environment that makes evolutionary design plausible. However one thing we haven't yet figured out is where the balance point is. I'm sure that, despite the outside impression, XP isn't just test, code, and refactor. There is room for designing before coding. Some of this is before there is any coding, most of it occurs in the iterations before coding for a particular task. But there is a new balance between up-front design and refactoring.

持續整合、測試和重構這些促成性實踐，提供了一個新環境，使演進式設計變得可行。然而，我們尚未完全找出其中的平衡點。我確信，儘管外界可能有這樣的印象，XP 並不僅僅是測試、編碼和重構。在編碼之前，仍然有設計的空間。其中一些設計是在開始編碼之前完成的，而大部分則是在針對特定任務進行編碼前的迭代過程中進行的。不過，前期設計與重構之間已經達成了一種新的平衡。

## The Value of Simplicity

Two of the greatest rallying cries in XP are the slogans “Do the Simplest Thing that Could Possibly Work” and “You Aren't Going to Need It” (known as YAGNI). Both are manifestations of the XP practice of Simple Design.

XP 中最具號召力的兩句口號是：「做可能奏效的最簡單方案」（Do the Simplest Thing that Could Possibly Work）和「你不會需要它」（You Aren't Going to Need It，簡稱 YAGNI）。這兩者都是 XP 實踐中**簡單設計**（Simple Design）理念的體現。

The way YAGNI is usually described, it says that you shouldn't add any code today which will only be used by feature that is needed tomorrow. On the face of it this sounds simple. The issue comes with such things as frameworks, reusable components, and flexible design. Such things are complicated to build. You pay an extra up-front cost to build them, in the expectation that you will gain back that cost later. This idea of building flexibility up-front is seen as a key part of effective software design.

YAGNI 的通常描述方式是，今天不應添加任何僅為了明天才需要的功能而編寫的程式碼。乍看之下，這聽起來很簡單。然而，問題出在框架、可重用元件和靈活設計等事物上。這些東西構建起來非常複雜，且需要額外的前期成本，期望在未來能收回這些投入。提前構建靈活性這一理念被視為高效軟體設計的一個關鍵部分。

However XP's advice is that you not build flexible components and frameworks for the first case that needs that functionality. Let these structures grow as they are needed. If I want a Money class today that handles addition but not multiplication then I build only addition into the Money class. Even if I'm sure I'll need multiplication in the next iteration, and understand how to do it easily, and think it'll be really quick to do, I'll still leave it till that next iteration.

然而，XP 的建議是，當首次需要某功能時，不要立即構建靈活的元件和框架，而是隨需求的出現逐步發展這些結構。如果今天我需要一個只處理加法但不處理乘法的 Money 類，那麼我只會在 Money 類中實現加法功能。即使我確信下一次迭代中會需要乘法，並且知道如何輕鬆實現，甚至認為這會非常快，我仍然會將它留到下一次迭代再處理。

One reason for this is economic. If I have to do any work that's only used for a feature that's needed tomorrow, that means I lose effort from features that need to be done for this iteration. The release plan says what needs to be worked on now, working on other things in the future is contrary to the developers agreement with the customer. There is a risk that this iteration's stories might not get done. Even if this iteration's stories are not at risk it's up to the customer to decide what extra work should be done - and that might still not involve multiplication.

這樣做的一個原因是經濟層面的考量。如果我投入時間處理僅為明天才需要的功能，就意味著減少了本次迭代中需要完成的功能的投入。發布計畫已經明確了現在需要處理的內容，提前處理未來的事情違背了開發人員與客戶之間的協議。這可能導致本次迭代中的需求無法按時完成。即使本次迭代的需求沒有風險，是否要進行額外的工作仍應由客戶決定，而這可能仍然不包括乘法功能的實現。

This economic disincentive is compounded by the chance that we may not get it right. However certain we may be about how this function works, we can still get it wrong - especially since we don't have detailed requirements yet. Working on the wrong solution early is even more wasteful than working on the right solution early. And the XPerts generally believe that we are much more likely to be wrong than right (and I agree with that sentiment.)

這種經濟上的不利因素因另一個可能性而更加明顯，那就是我們可能無法正確實現它。無論我們多麼確信這個功能的實現方式，我們仍可能出錯——尤其是在我們尚未獲得詳細需求的情況下。==過早地投入錯誤的解決方案比過早投入正確的解決方案更加浪費。而且，XP 專家普遍認為，我們出錯的可能性遠高於正確的可能性==（對此我也表示贊同）。

The second reason for simple design is that a complex design is more difficult to understand than a simple design. Therefore any modification of the system is made harder by added complexity. This adds a cost during the period between when the more complicated design was added and when it was needed.

Now this advice strikes a lot of people as nonsense, and they are right to think that. Right providing that you imagine the usual development world where the enabling practices of XP aren't in place. However when the balance between planned and evolutionary design alters, then YAGNI becomes good practice (and only then).

簡單設計的第二個原因是，複雜的設計比簡單的設計更難以理解。因此，對系統進行任何修改時，增加的複雜性都會使修改變得更加困難。在添加更複雜的設計到實際需要它之間的這段時間內，這也會帶來額外的成本。

So to summarize. You don't want to spend effort adding new capability that won't be needed until a future iteration. And even if the cost is zero, you still don't want to add it because it increases the cost of modification even if it costs nothing to put in. However you can only sensibly behave this way when you are using XP, or a similar technique that lowers the cost of change.

總結一下：你不應該為未來的迭代才需要的功能投入精力添加新能力。即使添加這些功能的成本為零，你仍然不應該這麼做，因為即使沒有直接的添加成本，它也會增加修改的成本。然而，只有在使用 XP 或類似能降低變更成本的技術時，這種做法才是合理的。

## What on Earth is Simplicity Anyway

So we want our code to be as simple as possible. That doesn't sound like that's too hard to argue for, after all who wants to be complicated? But of course this begs the question “what is simple?”

因此，我們希望程式碼盡可能地簡單。這聽起來似乎不難辯護，畢竟誰會希望事情變得複雜呢？但這當然引出了另一個問題：「什麼是簡單？」

In [XPE](https://www.amazon.com/gp/product/0201616416/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0201616416&linkCode=as2&tag=martinfowlerc-20) Kent gives four criteria for a simple system. In order (most important first):

- Runs all the Tests
- No duplication
- Reveals all the intention
- Fewest number of classes or methods

在 [XPE](https://www.amazon.com/gp/product/0201616416/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0201616416&linkCode=as2&tag=martinfowlerc-20) 一書中，Kent 提出了判斷一個簡單系統的四個標準，按重要性排序如下（最重要的在前）：

- 能通過所有測試
- 沒有重複
- 清晰展現所有意圖
- 使用最少的類或方法

Running all the tests is a pretty simple criterion. No duplication is also pretty straightforward, although a lot of developers need guidance on how to achieve it. The tricky one has to do with revealing the intention. What exactly does that mean?

執行所有測試是一個相當簡單的標準。「沒有重複」也很直觀，儘管許多開發者在如何實現這一點上需要指導。比較棘手的是與「揭示意圖」有關。那究竟是什麼意思呢？

The basic value here is clarity of code. XP places a high value on code that is easily read. In XP “clever code” is a term of abuse. But some people's intention revealing code is another's cleverness.

這裡的基本價值是程式碼的清晰度。XP 非常重視易於閱讀的程式碼。在 XP 中，「巧妙的程式碼」是一個貶義詞。但有些人認為能揭示意圖的程式碼，對另一些人來說卻是巧妙的表現。

In his XP 2000 paper, Josh Kerievsky points out a good example of this. He looks at possibly the most public XP code of all - [JUnit](http://www.xprogramming.com/software.htm). JUnit uses decorators to add optional functionality to test cases, such things as concurrency synchronization and batch set up code. By separating out this code into decorators it allows the general code to be clearer than it otherwise would be.

在他 2000 年的 XP 論文中，Josh Kerievsky 提出了一個很好的例子。他檢視了可能是最具代表性的 XP 程式碼之一——[JUnit](http://www.xprogramming.com/software.htm)。JUnit 使用裝飾者模式來為測試案例添加可選功能，如並發同步和批次設置程式碼。通過將這些程式碼分離到裝飾者中，它使得一般程式碼變得比原本更為清晰。

But you have to ask yourself if the resulting code is really simple. For me it is, but then I'm familiar with the Decorator pattern. But for many that aren't it's quite complicated. Similarly JUnit uses pluggable methods which I've noticed most people initially find anything but clear. So might we conclude that JUnit's design is simpler for experienced designers but more complicated for less experienced people?

但你必須問問自己，結果的程式碼是否真的是簡單的。對我來說是簡單的，但我熟悉裝飾者模式。但對於許多不熟悉的人來說，這樣的程式碼其實相當複雜。類似地，JUnit 使用了可插拔的方法，我注意到大多數人一開始覺得它並不清晰。所以我們是否可以得出結論，JUnit 的設計對於有經驗的設計師來說更簡單，但對於經驗較少的人來說卻更複雜呢？

I think that the focus on eliminating duplication, both with XP's “Once and Only Once” and the [Pragmatic Programmer's](https://www.amazon.com/gp/product/020161622X/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=020161622X&linkCode=as2&tag=martinfowlerc-20) DRY (Don't Repeat Yourself) is one of those obvious and wonderfully powerful pieces of good advice. Just following that alone can take you a long way. But it isn't everything, and simplicity is still a complicated thing to find.

我認為，消除重複的焦點——無論是 XP 的「一次且只有一次」還是 [Pragmatic Programmer](https://www.amazon.com/gp/product/020161622X/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=020161622X&linkCode=as2&tag=martinfowlerc-20) 的 DRY（不要重複自己）——是一些顯而易見且極具威力的良好建議。僅僅遵循這一點就能讓你走得很遠。但這並不是全部，簡單性仍然是一個複雜的事物，難以找到。

Recently I was involved in doing something that may well be over-designed. It got refactored and some of the flexibility was removed. But as one of the developers said “it's easier to refactor over-design than it is to refactor no design.” It's best to be a little simpler than you need to be, but it isn't a disaster to be a little more complex.

最近我參與了一個可能過度設計的專案。它經過重構後，一些靈活性被移除了。但正如其中一位開發者所說的：「重構過度設計比重構沒有設計的情況更容易。」最好比需要的簡單一些，但過於複雜也不是災難。

The best advice I heard on all this came from Uncle Bob (Robert Martin). His advice was not to get too hung up about what the simplest design is. After all you can, should, and will refactor it later. In the end the willingness to refactor is much more important than knowing what the simplest thing is right away.

我聽過的最好的建議來自 Uncle Bob（Robert Martin）。他的建議是不要過於糾結於最簡單的設計是什麼。畢竟，你可以、應該並且將來會進行重構。最終，願意重構比一開始就知道最簡單的設計要重要得多。

## Does Refactoring Violate YAGNI?

This topic came up on the XP mailing list recently, and it's worth bringing out as we look at the role of design in XP.

這個話題最近出現在 XP 的郵件列表中，值得在我們討論 XP 中設計的角色時提及。

Basically the question starts with the point that refactoring takes time but does not add function. Since the point of YAGNI is that you are supposed to design for the present not for the future, is this a violation?

基本上，這個問題的出發點是重構需要時間，但不會增加功能。既然 YAGNI 的核心是你應該為當前設計，而不是為未來設計，那麼這樣做是否違背了這一原則呢？

The point of YAGNI is that you don't add complexity that isn't needed for the current stories. This is part of the practice of simple design. Refactoring is needed to keep the design as simple as you can, so you should refactor whenever you realize you can make things simpler.

YAGNI 的核心觀點是你不應該為當前的故事添加不必要的複雜性。這是簡單設計實踐的一部分。重構是保持設計簡單的必要手段，因此，當你意識到可以簡化某些部分時，應該進行重構。

Simple design both exploits XP practices and is also an enabling practice. Only if you have testing, continuous integration, and refactoring can you practice simple design effectively. But at the same time keeping the design simple is essential to keeping the change curve flat. Any unneeded complexity makes a system harder to change in all directions except the one you anticipate with the complex flexibility you put in. However people aren't good at anticipating, so it's best to strive for simplicity. However people won't get the simplest thing first time, so you need to refactor in order get closer to the goal.

簡單設計既利用了 XP 實踐，又是一種使能實踐。只有當你擁有測試、持續集成和重構時，才能有效地實踐簡單設計。與此同時，保持設計簡單對保持變更曲線平緩至關重要。任何不必要的複雜性都會使系統變得更難以改動，除了你預期的、為其設計的複雜靈活性。然而，人們並不擅長預測，因此最好追求簡單性。然而，人們不會第一次就做到最簡單的設計，所以你需要通過重構來逐步接近目標。

## Patterns and XP

The JUnit example leads me inevitably into bringing up patterns. The relationship between patterns and XP is interesting, and it's a common question. Joshua Kerievsky argues that patterns are under-emphasized in XP and he makes the argument eloquently, so I don't want to repeat that. But it's worth bearing in mind that for many people patterns seem in conflict to XP.

JUnit 的例子讓我不可避免地提到了設計模式。設計模式與 XP 之間的關係非常有趣，也是常見的問題。Joshua Kerievsky 認為在 XP 中設計模式被低估了，他表達這個觀點非常有說服力，所以我不打算重複他的論點。但值得注意的是，對於許多人來說，設計模式似乎與 XP 存在衝突。

The essence of this argument is that patterns are often over-used. The world is full of the legendary programmer, fresh off his first reading of [GOF](https://www.amazon.com/gp/product/0201633612/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0201633612&linkCode=as2&tag=martinfowlerc-20) who includes sixteen patterns in 32 lines of code. I remember one evening, fueled by a very nice single malt, running through with Kent a paper to be called “Not Design Patterns: 23 cheap tricks” We were thinking of such things as use an if statement rather than a strategy. The joke had a point, patterns are often overused, but that doesn't make them a bad idea. The question is how you use them.

這個論點的核心是設計模式經常被過度使用。世界上充滿了那些傳奇的程式設計師，他們剛讀完《GOF》一書後，便在 32 行代碼中加入了十六個設計模式。我記得有一個晚上，在喝了一杯非常好的單一麥芽威士忌後，我和 Kent 一起討論一篇名為《不是設計模式：23個便宜的技巧》的論文。我們當時想到的例子包括使用 if 語句而不是策略模式。這個笑話有其道理，設計模式確實經常被過度使用，但這並不意味著它們就是壞主意。關鍵在於你如何使用它們。

One theory of this is that the forces of simple design will lead you into the patterns. Many refactorings do this explicitly, but even without them by following the rules of simple design you will come up with the patterns even if you don't know them already. This may be true, but is it really the best way of doing it? Surely it's better if you know roughly where you're going and have a book that can help you through the issues instead of having to invent it all yourself. I certainly still reach for GOF whenever I feel a pattern coming on. For me effective design argues that we need to know the price of a pattern is worth paying - that's its own skill. Similarly, as Joshua suggests, we need to be more familiar about how to ease into a pattern gradually. In this regard XP treats the way we use patterns differently to the way some people use them, but certainly doesn't remove their value.

其中一種理論認為，簡單設計的力量會將你引導進入設計模式。許多重構過程明確地做到了這一點，但即使沒有這些過程，通過遵循簡單設計的規則，你也會自然產生設計模式，即使你事先不熟悉它們。這可能是對的，但這真的是最好的做法嗎？當然，如果你大致知道自己要去哪裡，並且有一本能幫助你解決問題的書，而不是必須自己發明所有的解決方案，這樣會更好。我自己確實會在感覺到需要使用設計模式時，翻開《GOF》一書。對我來說，有效的設計意味着我們需要知道一個模式的價值是否值得付出代價——這本身就是一項技能。同樣地，正如 Joshua 所建議的，我們需要更熟悉如何逐步進入一個模式。在這方面，XP 處理設計模式的方式與一些人使用它們的方式有所不同，但無疑不會剝奪它們的價值。

But reading some of the mailing lists I get the distinct sense that many people see XP as discouraging patterns, despite the irony that most of the proponents of XP were leaders of the patterns movement too. Is this because they have seen beyond patterns, or because patterns are so embedded in their thinking that they no longer realize it? I don't know the answers for others, but for me patterns are still vitally important. XP may be a process for development, but patterns are a backbone of design knowledge, knowledge that is valuable whatever your process may be. Different processes may use patterns in different ways. XP emphasizes both not using a pattern until it's needed and evolving your way into a pattern via a simple implementation. But patterns are still a key piece of knowledge to acquire.

但從我閱讀的一些郵件列表中，我明顯感覺到許多人認為 XP 會抑制設計模式，儘管諷刺的是，大多數 XP 的支持者也是設計模式運動的領導者之一。這是因為他們已經超越了設計模式，還是因為模式已經深深植入他們的思維中，以至於他們再也沒有意識到它？我不知道其他人的答案，但對我來說，設計模式仍然是至關重要的。XP 可能是一個開發過程，但設計模式是設計知識的基礎，無論你的開發過程是什麼，這些知識都是有價值的。不同的過程可能會以不同的方式使用設計模式。XP 強調的是，只有在需要時才使用模式，並通過簡單的實現逐步發展出模式。但設計模式仍然是必須獲得的重要知識。

My advice to XPers using patterns would be

- Invest time in learning about patterns
- Concentrate on when to apply the pattern (not too early)
- Concentrate on how to implement the pattern in its simplest form first, then add complexity later.
- If you put a pattern in, and later realize that it isn't pulling its weight - don't be afraid to take it out again.

我對使用設計模式的 XP 實踐者的建議是：

- 投入時間學習設計模式
- 集中精力在何時應用設計模式（不要過早使用）
- 集中精力首先以最簡單的形式實現模式，再在後來增加複雜度
- 如果你加入了一個設計模式，後來發現它沒有發揮作用，不要害怕將其移除

I think XP should emphasize learning about patterns more. I'm not sure how I would fit that into XP's practices, but I'm sure Kent can come up with a way.

我認為 XP 應該更強調學習設計模式。我不確定該如何將這一點融入 XP 的實踐中，但我相信 Kent 能夠想出一個方法。

## Growing an Architecture

What do we mean by a software architecture? To me the term architecture conveys a notion of the core elements of the system, the pieces that are difficult to change. A foundation on which the rest must be built.

我們所說的軟體架構是什麼意思？對我來說，架構這個術語傳達了一個概念，即系統的核心元素，那些難以變更的部分。它是其他部分必須建立的基礎。

What role does an architecture play when you are using evolutionary design? Again XPs critics state that XP ignores architecture, that XP's route is to go to code fast and trust that refactoring that will solve all design issues. Interestingly they are right, and that may well be weakness. Certainly the most aggressive XPers - Kent Beck, Ron Jeffries, and Bob Martin - are putting more and more energy into avoiding any up front architectural design. Don't put in a database until you really know you'll need it. Work with files first and refactor the database in during a later iteration.

在使用演化設計時，架構扮演什麼角色？再次，XP的批評者指出XP忽視架構，認為XP的路徑是快速編寫程式碼，並相信重構能解決所有設計問題。有趣的是，他們是對的，這可能是XP的一個弱點。當然，最積極的XP支持者——Kent Beck、Ron Jeffries 和 Bob Martin——越來越強調避免任何前期的架構設計。直到真正確定需要資料庫時才加入資料庫。先使用檔案，然後在以後的迭代中重構資料庫。

I'm known for being a cowardly XPer, and as such I have to disagree. I think there is a role for a broad starting point architecture. Such things as stating early on how to layer the application, how you'll interact with the database (if you need one), what approach to use to handle the web server.

我以是個膽小的XPer而聞名，因此我必須不同意。我認為，廣泛的起始架構仍然有其角色。比如早期就確定如何分層應用程式、如何與資料庫互動（如果需要資料庫的話）、以及如何處理網頁伺服器的方式等。

Essentially I think many of these areas are patterns that we've learned over the years. As your knowledge of patterns grows, you should have a reasonable first take at how to use them. However the key difference is that these early architectural decisions aren't expected to be set in stone, or rather the team knows that they may err in their early decisions, and should have the courage to fix them. Others have told the story of one project that, close to deployment, decided it didn't need EJB anymore and removed it from their system. It was a sizeable refactoring, it was done late, but the enabling practices made it not just possible, but worthwhile.

本質上，我認為這些領域中的許多是我們多年來所學到的模式。隨著你對模式的了解逐漸增長，你應該能夠對如何使用它們有一個合理的初步理解。然而，關鍵的區別在於，這些早期的架構決策並不被期望是一成不變的，或者更確切地說，團隊知道他們在早期的決策中可能會犯錯，並應該有勇氣去修正它們。其他人也曾講過一個故事，一個項目在接近部署時決定不再需要EJB，並將其從系統中移除。這是一個相當大的重構，雖然是晚了些，但啟用的實踐使它不僅可能，而且值得去做。

How would this have worked the other way round. If you decided not to use EJB, would it be harder to add it later? Should you thus never start with EJB until you have tried things without and found it lacking? That's a question that involves many factors. Certainly working without a complex component increases simplicity and makes things go faster. However sometimes it's easier to rip out something like that than it is to put it in.

如果情況相反，這樣做會怎麼樣呢？如果你決定不使用EJB，將來再加入它會不會更難？那麼你是否應該等到嘗試過不使用它，並發現它的不足，才考慮開始使用EJB？這是一個涉及多個因素的問題。當然，沒有使用複雜組件會增加簡單性，並使事情進展得更快。然而，有時候，將這樣的東西移除比加入它還要容易。

So my advice is to begin by assessing what the likely architecture is. If you see a large amount of data with multiple users, go ahead and use a database from day 1. If you see complex business logic, put in a domain model. However in deference to the gods of YAGNI, when in doubt err on the side of simplicity. Also be ready to simplify your architecture as soon as you see that part of the architecture isn't adding anything.

所以我的建議是，首先評估可能的架構。如果你預見到大量數據和多個使用者，從一開始就使用資料庫。如果你看到複雜的業務邏輯，就加入領域模型。然而，遵循YAGNI的原則，當你不確定時，應該傾向於簡單。並且一旦發現某部分架構沒有帶來任何價值，就準備隨時簡化你的架構。

## UML and XP

Of all the questions I get about my involvement with XP one of the biggest revolves around my association with the UML. Aren't the two incompatible?

在所有關於我參與 XP（極限編程）的問題中，其中一個最大的問題圍繞著我與 UML（統一建模語言）的關係。難道這兩者不相容嗎？

There are a number of points of incompatibility. Certainly XP de-emphasizes diagrams to a great extent. Although the official position is along the lines of “use them if they are useful”, there is a strong subtext of “real XPers don't do diagrams”. This is reinforced by the fact that people like Kent aren't at all comfortable with diagrams, indeed I've never seen Kent voluntarily draw a software diagram in any fixed notation.

確實，XP 和 UML 之間存在一些不相容的地方。XP 明顯地減少了對圖表的重視。雖然官方立場是類似於「如果有用就使用它們」，但隱含的強烈訊息卻是「真正的 XP 開發者不畫圖表」。這種觀念更因為像 Kent 這樣的人對圖表並不感到自在而加強。事實上，我從未見過 Kent 自願使用任何固定符號畫出軟體圖表。

I think the issue comes from two separate causes. One is the fact that some people find software diagrams helpful and some people don't. The danger is that those who do think that those who don't should do and vice-versa. Instead we should just accept that some people will use diagrams and some won't.

我認為這個問題源於兩個不同的原因。其一是，有些人認為軟體圖表有幫助，而有些人則不這麼認為。危險在於，認為圖表有用的人會覺得那些不使用的人應該使用，而不使用圖表的人則認為使用的人不應該這樣做。相反，我們應該接受這樣的事實：有些人會使用圖表，而有些人則不會。

The other issue is that software diagrams tend to get associated with a heavyweight process. Such processes spend a lot of time drawing diagrams that don't help and can actually cause harm. So I think that people should be advised how to use diagrams well and avoid the traps, rather than the “only if you must (wimp)” message that usually comes out of the XPerts.

另一個問題是，軟體圖表往往與繁重的流程聯繫在一起。這些流程花費大量時間繪製的圖表不但沒有幫助，甚至可能造成傷害。因此，我認為應該教導人們如何善用圖表並避免陷阱，而不是傳遞出XP專家們通常表達的「只有必要時才使用（懦弱）」這種訊息。

So here's my advice for using diagrams well.

以下是我對如何善用圖表的建議：

First keep in mind what you're drawing the diagrams for. The primary value is communication. Effective communication means selecting important things and neglecting the less important. This selectivity is the key to using the UML well. Don't draw every class - only the important ones. For each class, don't show every attribute and operation - only the important ones. Don't draw sequence diagrams for all use cases and scenarios - only... you get the picture. A common problem with the common use of diagrams is that people try to make them comprehensive. The code is the best source of comprehensive information, as the code is the easiest thing to keep in sync with the code. For diagrams comprehensiveness is the enemy of comprehensibility.

首先，請記住您繪製圖表的目的。圖表的主要價值在於**溝通**。有效的溝通意味著選擇重要的內容，忽略次要的部分。這種選擇性是善用 UML 的關鍵。不要繪製所有的類別——只繪製重要的類別。對於每個類別，也不要顯示所有的屬性和操作——只顯示重要的部分。不要為所有的用例和情境繪製序列圖——只需選擇重要的情境即可。您應該明白這個道理。常見的問題是，人們試圖讓圖表變得**全面而詳盡**。但實際上，代碼才是獲取全面資訊的最佳來源，因為代碼最容易與其自身保持同步。而對於圖表來說，全面性反而是**可理解性**的敵人。

A common use of diagrams is to explore a design before you start coding it. Often you get the impression that such activity is illegal in XP, but that's not true. Many people say that when you have a sticky task it's worth getting together to have a quick design session first. However when you do such sessions:

在開始編寫代碼之前，常常會使用圖表來探索設計。您可能會感覺這樣的活動在 XP 中是「非法」的，但事實並非如此。許多人認為，當遇到棘手的任務時，值得先召集大家進行一場快速的設計討論。然而，當您進行這類討論時，請注意以下幾點：

- keep them short
- don't try to address all the details (just the important ones)
- treat the resulting design as a sketch, not as a final design

- 保持簡短
- 不要試圖解決所有細節（只專注於重要部分）
- 將產生的設計視為草圖，而非最終設計

The last point is worth expanding. When you do some up-front design, you'll inevitably find that some aspects of the design are wrong, and you only discover this when coding. That's not a problem providing that you then change the design. The trouble comes when people think the design is done, and then don't take the knowledge they gained through the coding and run it back into the design.

最後一點值得深入說明。當你進行一些前期設計時，難免會發現設計的某些方面是錯誤的，而這些錯誤通常只有在編碼時才會發現。只要在發現後修改設計，這並不會構成問題。麻煩的是，有些人認為設計已經完成，卻沒有將透過編碼獲得的新知識回饋到設計中。

Changing the design doesn't necessarily mean changing the diagrams. It's perfectly reasonable to draw diagrams that help you understand the design and then throw the diagrams away. Drawing them helped, and that is enough to make them worthwhile. They don't have to become permanent artifacts. The best UML diagrams are not artifacts.

修改設計並不一定表示需要修改圖表。繪製圖表來幫助理解設計，然後將圖表丟棄是完全合理的。繪製圖表的過程本身就有助於理解，這已足以讓其變得有價值。它們不必成為永久性的產物。最好的 UML 圖表並非必須是長存的產物。

A lot of XPers use CRC cards. That's not in conflict with UML. I use a mix of CRC and UML all the time, using whichever technique is most useful for the job at hand.

許多 XP 的實踐者會使用 CRC 卡片，這與 UML 並不衝突。我經常將 CRC 和 UML 混合使用，根據當下的需求選擇最有用的技術。

Another use of UML diagrams is on-going documentation. In its usual form this is a model residing on a case tool. The idea is that keeping this documentation helps people work on the system. In practice it often doesn't help at all.

另一種 UML 圖的使用方式是作為持續的文件記錄。通常情況下，這是一個存放在 CASE 工具中的模型。理論上，維持這些文件可以幫助人們更好地處理系統，但實際上它往往毫無幫助。

- it takes too long to keep the diagrams up to date, so they fall out of sync with the code
- they are hidden in a CASE tool or a thick binder, so nobody looks at them

- 更新圖表需要花費太多時間，因此它們與程式碼脫節。
- 圖表被藏在 CASE 工具或厚重的文件夾中，導致沒有人去查看它們。

So the advice for on-going documentation runs from these observed problems:

因此，針對持續文件化的建議源自這些觀察到的問題：

- Only use diagrams that you can keep up to date without noticeable pain
- Put the diagrams where everyone can easily see them. I like to post them on a wall. Encourage people to edit the wall copy with a pen for simple changes.
- Pay attention to whether people are using them, if not throw them away.

- 只使用那些可以輕鬆保持更新的圖表，避免造成明顯的麻煩。
- 將圖表放在每個人都能輕鬆看到的地方。我喜歡把它們張貼在牆上，並鼓勵人們用筆直接在牆上的圖表上進行簡單修改。
- 注意是否有人實際在使用這些圖表，如果沒有人使用，就將它們丟掉。

The last aspect of using UML is for documentation in a handover situation, such as when one group hands over to another. Here the XP point is that producing documentation is a [user story](https://martinfowler.com/bliki/UserStory.html) like any other, and thus its business value is determined by the customer. Again the UML is useful here, providing the diagrams are selective to help communication. Remember that the code is the repository of detailed information, the diagrams act to summarize and highlight important issues.

使用 UML 的最後一個方面是用於交接情境，例如一個團隊將工作移交給另一個團隊。在這裡，XP 的觀點是，撰寫文檔是一種[使用者故事](https://martinfowler.com/bliki/UserStory.html)，其商業價值由客戶決定。同樣地，UML 在這種情況下是有用的，但前提是圖表具有選擇性，能夠幫助溝通。記住，程式碼是詳細資訊的存放庫，而圖表的作用則是摘要和突出重要的問題。
## On Metaphor ^martinIsDesignDeadMetaphor

Okay I might as well say it publicly - I still haven't got the hang of this metaphor thing. I saw it work, and work well on the C3 project, but it doesn't mean I have any idea how to do it, let alone how to explain how to do it.

好吧，我還是公開承認吧——我到現在仍然不太理解這個隱喻的概念。在 C3 項目中，我見過它的運作方式，而且效果很好，但這並不代表我知道如何實現它，更別提如何解釋如何做到這一點了。

The XP practice of Metaphor is built on Ward Cunninghams's approach of a system of names. The point is that you come up with a well known set of names that acts as a vocabulary to talk about the domain. This system of names plays into the way you name the classes and methods in the system.

XP 中的隱喻實踐是建立在 Ward Cunningham 的「命名系統」方法之上的。其核心在於創造一套熟知的名稱，作為討論領域的詞彙基礎。這套命名系統會影響系統中類別和方法的命名方式。

I've built a system of names by building a conceptual model of the domain. I've done this with the domain experts using UML or its predecessors. I've found you have to be careful doing this. You need to keep to a minimal simple set of notation, and you have to guard against letting any technical issues creeping into the model. But if you do this I've found that you can use this to build a vocabulary of the domain that the domain experts can understand and use to communicate with developers. The model doesn't match the class designs perfectly, but it's enough to give a common vocabulary to the whole domain.

我曾透過建立領域的概念模型來構建一個命名系統，並與領域專家一起使用 UML 或其前身完成這項工作。我發現，在進行這種建模時需要特別謹慎。必須保持簡單且最小化的符號集合，並且要防止任何技術性問題滲入模型。然而，如果能做到這些，我發現可以藉此構建出一套領域詞彙，讓領域專家能夠理解並用來與開發人員進行溝通。這個模型雖然不會完全與類別設計相匹配，但已足以為整個領域提供一個共同的詞彙基礎。

Now I don't see any reason why this vocabulary can't be a metaphorical one, such as the C3 metaphor that turned payroll into a factory assembly line. But I also don't see why basing your system of names on the vocabulary of the domain is such a bad idea either. Nor am I inclined to abandon a technique that works well for me in getting the system of names.

我不認為這套詞彙無法是比喻性的，例如 C3 比喻將薪資系統轉化為工廠流水線。但我也不覺得將命名系統基於領域的詞彙是個壞主意。此外，我也不傾向於放棄一個對我來說在構建命名系統方面運作良好的方法。

Often people criticize XP on the basis that you do need at least some outline design of a system. XPers often respond with the answer “that's the metaphor”. But I still don't think I've seen metaphor explained in a convincing manner. This is a real gap in XP, and one that the XPers need to sort out.

常常有人批評極限編程（XP）的理念，認為在開發系統時至少需要一些初步的架構設計。XP支持者通常以「那就是隱喻（metaphor）」來回應。但我仍然認為，這個隱喻至今還未被令人信服地解釋清楚。這是極限編程中的一個真實缺口，是XP支持者需要解決的問題。

## Do you wanna be an Architect when you grow up?

For much of the last decade, the term “software architect” has become popular. It's a term that is difficult personally for me to use. My wife is a structural engineer. The relationship between engineers and architects is ... interesting. My favorite was “architects are good for the three B's: bulbs, bushes, birds”. The notion is that architects come up with all these pretty drawings, but it's the engineers who have to ensure that they actually can stand up. As a result I've avoided the term software architect, after all if my own wife can't treat me with professional respect what chance do I stand with anyone else?

在過去十年的大部分時間裡，「軟體架構師」這個詞變得很流行。這是一個我個人很難用的詞。我的妻子是結構工程師。工程師和建築師之間的關係……很有趣。我最喜歡的一句話是：「建築師最擅長三個B：燈泡、灌木、鳥類」。這個說法意味著建築師會畫出漂亮的圖紙，但真正要確保這些設計能夠實際站立起來的是工程師。因此，我一直避免使用「軟體架構師」這個詞，畢竟如果連我自己的妻子都無法以職業尊重的態度看待我，我又怎麼指望其他人會尊重我呢？

In software, the term architect means many things. (In software any term means many things.) In general, however it conveys a certain gravitas, as in “I'm not just a mere programmer - I'm an architect”. This may translate into “I'm an architect now - I'm too important to do any programming”. The question then becomes one of whether separating yourself from the mundane programming effort is something you should do when you want to exercise technical leadership.

在軟體領域，「架構師」這個詞有許多不同的含義。（在軟體世界裡，任何詞彙都可能有多種含義。）然而，總的來說，它傳達出一種特定的莊重感，就像是在說：「我不只是一個普通的程式設計師 — 我是一個架構師」。這可能進一步轉化為：「我現在是架構師了 — 我已經太重要，不需要再做任何程式設計工作」。於是，問題就變成了：當你想要發揮技術領導力時，將自己與日常的程式設計工作隔絕是否明智。

This question generates an enormous amount of emotion. I've seen people get very angry at the thought that they don't have a role any more as architects. “There is no place in XP for experienced architects” is often the cry I hear.

這個問題引發了大量的情緒波動。我曾看到很多人一聽到可能失去架構師角色就極度憤怒。「在極限編程（XP）中，資深架構師根本沒有立足之地」是我經常聽到的抱怨。

Much as in the role of design itself, I don't think it's the case that XP does not value experience or good design skills. Indeed many of the proponents of XP - Kent Beck, Bob Martin, and of course Ward Cunningham - are those from whom I have learned much about what design is about. However it does mean that their role changes from what a lot of people see as a role of technical leadership.

正如在設計角色本身一樣，我不認為極限編程（XP）不重視經驗或優秀的設計技能。事實上，許多XP的支持者 — 包括Kent Beck、Bob Martin，當然還有Ward Cunningham — 正是那些我從中學到許多設計本質的人。然而，這確實意味著他們的角色已經改變，不再是許多人眼中傳統的技術領導角色。

As an example, I'll cite one of our technical leaders at Thoughtworks: Dave Rice. Dave has been through a few life-cycles and has assumed the unofficial mantle of technical lead on a fifty person project. His role as leader means spending a lot of time with all the programmers. He'll work with a programmer when they need help, he looks around to see who needs help. A significant sign is where he sits. As a long term ThoughtWorker, he could pretty well have any office he liked. He shared one for a while with Cara, the release manager. However in the last few months he moved out into the open bays where the programmers work (using the open “war room” style that XP favors.) This is important to him because this way he sees what's going on, and is available to lend a hand wherever it's needed.

舉個例子，我要談談在Thoughtworks的一位技術領導：Dave Rice。Dave經歷過幾個專案生命週期，並在一個五十人的專案中非正式地擔任技術負責人。作為領導者，他的角色意味著要花很多時間與所有程式設計師互動。他會在程式設計師需要幫助時給予協助，並四處察看誰需要幫忙。他的座位位置是一個很重要的標誌。作為一位資深的ThoughtWorks員工，他本可以選擇任何他喜歡的辦公室。他曾與發佈經理Cara共用一個辦公室。然而，在最近幾個月，他搬到了程式設計師工作的開放辦公區（使用XP偏好的開放式「戰情室」風格）。對他來說，這很重要，因為這樣他可以看到正在發生的事情，並且可以在需要的地方隨時提供幫助。

Those who know XP will realize that I'm describing the explicit XP role of Coach. Indeed one of the several games with words that XP makes is that it calls the leading technical figure the “Coach”. The meaning is clear: in XP technical leadership is shown by teaching the programmers and helping them make decisions. It's one that requires good people skills as well as good technical skills. Jack Bolles at XP 2000 commented that there is little room now for the lone master. Collaboration and teaching are keys to success.

了解極限編程（XP）的人會意識到，我描述的是XP明確定義的「教練」角色。事實上，XP在術語上的一個小把戲就是稱呼技術領導為「教練」。其意義很清楚：在XP中，技術領導體現在教導程式設計師並幫助他們做決策。這個角色不僅需要出色的技術技能，還需要優秀的人際交往能力。在XP 2000大會上，Jack Bolles評論說，現在已經很少有lone master（獨狼式專家）的立足之地。協作和教學是成功的關鍵。

At a conference dinner, Dave and I talked with a vocal opponent of XP. As we discussed what we did, the similarities in our approach were quite marked. We all liked adaptive, iterative development. Testing was important. So we were puzzled at the vehemence of his opposition. Then came his statement, along the lines of “the last thing I want is my programmers refactoring and monkeying around with the design”. Now all was clear. The conceptual gulf was further explicated by Dave saying to me afterwards “if he doesn't trust his programmers why does he hire them?”. In XP the most important thing the experienced developer can do is pass on as many skills as he can to the more junior developers. Instead of an architect who makes all the important decisions, you have a coach that teaches developers to make important decisions. As Ward Cunningham pointed out, by that he amplifies his skills, and adds more to a project than any lone hero can.

在一場會議晚宴上，Dave和我與一位對極限編程（XP）持強烈反對意見的人交談。當我們討論我們的工作方法時，我們的方法之間有相當明顯的相似之處。我們都喜歡適應性的、迭代式的開發。測試很重要。所以我們對他激烈的反對感到困惑。然後他說了這樣的話：「我最不希望的就是我的程式設計師進行重構並亂改設計」。現在一切都清楚了。Dave隨後對我說：「如果他不信任他的程式設計師，幹嘛要雇用他們？」這個概念上的鴻溝進一步得到了闡明。在XP中，經驗豐富的開發者可以做的最重要的事情，就是盡可能多地將技能傳授給資歷較淺的開發者。與其由架構師做出所有重要決策，不如有一位教練教導開發者如何做出重要決策。正如Ward Cunningham指出的，通過這種方式，他放大了自己的技能，為專案增添的價值遠超過任何獨立英雄所能做到的。

## Reversibility

At XP 2002 Enrico Zaninotto gave a fascinating talk that discussed the tie-ins between agile methods and lean manufacturing. His view was that one of the key aspects of both approaches was that they tackled complexity by reducing the irreversibility in the process.

在XP 2002大會上，Enrico Zaninotto發表了一場極具啟發性的演講，討論了敏捷方法與精益製造之間的聯繫。他的觀點是，這兩種方法的關鍵方面之一，在於通過降低過程中的不可逆性來應對複雜性。

In this view one of the main source of complexity is the irreversibility of decisions. If you can easily change your decisions, this means it's less important to get them right - which makes your life much simpler. The consequence for evolutionary design is that designers need to think about how they can avoid irreversibility in their decisions. Rather than trying to get the right decision now, look for a way to either put off the decision until later (when you'll have more information) or make the decision in such a way that you'll be able to reverse it later on without too much difficulty.

從這個角度看，複雜性的主要來源之一是決策的不可逆性。如果你可以輕鬆改變決策，這意味著一開始做出正確決策的重要性降低了 — 這讓你的生活變得簡單得多。對於演進式設計來說，其後果是設計師需要思考如何避免決策中的不可逆性。與其現在就試圖做出正確的決策，不如尋找方法要麼將決策推遲到稍後（屆時你將擁有更多資訊），要麼以一種可以在日後不太困難地撤銷的方式做出決策。

This determination to support reversibility is one of the reasons that agile methods put a lot of emphases on source code control systems, and of putting everything into such a system. While this does not guarantee reversibility, particularly for longed-lived decisions, it does provide a foundation that gives confidence to a team, even if it's rarely used.

這種支持可逆性的決心，是敏捷方法非常強調原始碼控制系統，並將所有內容都放入此類系統的原因之一。雖然這並不能完全保證可逆性，尤其是對於長期性的決策，但它確實提供了一個基礎，給團隊帶來信心，即便這個系統很少被實際使用。

Designing for reversibility also implies a process that makes errors show up quickly. One of the values of iterative development is that the rapid iterations allow customers to see a system as it grows, and if a mistake is made in requirements it can be spotted and fixed before the cost of fixing becomes prohibitive. This same rapid spotting is also important for design. This means that you have to set things up so that potential problem areas are rapidly tested to see what issues arrive. It also means it's worth doing experiments to see how hard future changes can be, even if you don't actually make the real change now - effectively doing a throw-away prototype on a branch of the system. Several teams have reporting trying out a future change early in prototype mode to see how hard it would be.

為可逆性設計還意味著建立一個能快速顯現錯誤的流程。迭代開發的價值之一，在於快速的疊代能讓客戶看到系統的成長過程，如果需求中出現錯誤，可以在修正成本變得prohibitive之前及時發現並修正。這種快速發現同樣對設計也很重要。這意味著你必須安排好系統，以便快速測試潛在的問題區域，看看會出現什麼問題。這還意味著值得進行實驗，以了解未來的變更會有多困難，即使你現在並不真正做出實際的更改 — 本質上是在系統的分支上做一個拋棄式原型。已經有多個團隊報告過在早期以原型模式嘗試未來可能的變更，以了解其難度。

## The Will to Design

While I've concentrated a lot of technical practices in this article, one thing that's too easy to leave out is the human aspect.

雖然在這篇文章中我著重討論了許多技術實踐，但很容易忽略的是人性這個層面。

In order to work, evolutionary design needs a force that drives it to converge. This force can only come from people - somebody on the team has to have the determination to ensure that the design quality stays high.

為了能夠運作，演進式設計需要一種驅使其收斂的力量。這種力量只能來自人：團隊中必須有人有決心確保設計品質始終保持高水準。

This will does not have to come from everyone (although it's nice if it does), usually just one or two people on the team take on the responsibility of keeping the design whole. This is one of the tasks that usually falls under the term 'architect'.

這種意願不必來自每個人（儘管若能如此當然很好），通常只需團隊中的一兩個人承擔起維持設計完整性的責任。這通常是「架構師」這個角色所需執行的任務之一。

This responsibility means keeping a constant eye on the code base, looking to see if any areas of it are getting messy, and then taking rapid action to correct the problem before it gets out of control. The keeper of the design doesn't have to be the one who fixes it - but they do have to ensure that it gets fixed by somebody.

這項責任意味著必須時刻關注代碼庫，檢查是否有任何部分變得混亂，並迅速採取行動在問題失控之前加以解決。設計的維護者不一定要親自修復問題，但他們必須確保有人負責修復問題。

A lack of will to design seems to be a major reason why evolutionary design can fail. Even if people are familiar with the things I've talked about in this article, without that will design won't take place.

缺乏設計意志似乎是導致演化式設計失敗的一個主要原因。即使人們熟悉我在本文中提到的內容，但如果沒有這種意志，設計就無法實現。

## Things that are difficult to refactor in

Can we use refactoring to deal with all design decisions, or are there some issues that are so pervasive that they are difficult to add in later? At the moment, the XP orthodoxy is that all things are easy to add when you need them, so YAGNI always applies. I wonder if there are exceptions. A good example of something that is controversial to add later is internationalization. Is this something which is such a pain to add later that you should start with it right away?

我們是否可以透過重構來處理所有的設計決策，還是有些問題過於廣泛，以至於後續很難加入？目前，XP（極限編程）的正統觀點認為，所有東西在需要時都很容易加入，因此「YAGNI」（You Aren't Gonna Need It，即你不會需要它）始終適用。然而，我不禁想知道是否存在例外。一個有爭議的例子是**國際化**。這是否是一個如此麻煩以至於後續加入會很痛苦的問題，因此應該一開始就考慮？

I could readily imagine that there are some things that would fall into this category. However the reality is that we still have very little data. If you have to add something, like internationalization, in later you're very conscious of the effort it takes to do so. You're less conscious of the effort it would actually have taken, week after week, to put it in and maintain it before it was actually needed. Also you 're less conscious of the fact that you may well have got it wrong, and thus needed to do some refactoring anyway.

我可以輕易想像確實有一些事情可能屬於這一類別。然而，現實是我們仍然缺乏足夠的數據。如果你需要在後期加入某些功能，例如國際化，你會非常清楚這需要付出的努力。然而，你卻不太清楚如果一開始就加入並在實際需要之前每週持續維護它，究竟會花費多少精力。同時，你也不太會意識到自己可能早早就做錯了，從而仍然需要進行一些重構。

Part of the justification of YAGNI is that many of these potential needs end up not being needed, or at least not in the way you'd expect. By not doing them, you'll save a good deal of effort. Although there will be effort required to refactor the simple solution into what you actually need, this refactoring is likely to be less work than building all the questionable features.

YAGNI 的其中一部分理由在於，許多潛在的需求最終可能根本不需要，或者至少不會以你預期的方式需要。透過不預先實現這些功能，你可以節省大量的精力。雖然將簡單的解決方案重構為實際所需的功能確實需要一些努力，但這種重構工作通常比預先構建所有那些可疑的功能要少得多。

Another issue to bear in mind in this is whether you really know how to do it. If you've done internationalization several times, then you'll know the patterns you need to employ. As such you're more likely to get it right. Adding anticipatory structures is probably better if you're in that position, than if you're new to the problem. So my advice would be that if you do know how to do it, you're in a position to judge the costs of doing it now to doing it later. However if you've not done it before, not just are you not able to assess the costs well enough, you're also less likely to do it well. In which case you should add it later. If you do add it then, and find it painful, you'll probably be better off than you would have been had you added it early. Your team is more experienced, you know the domain better, and you understand the requirements better. Often in this position you look back at how easy it would have been with 20/20 hindsight. It may have been much harder to add it earlier than you think.

另一個需要記住的問題是，你是否真的知道該怎麼做。如果你已經多次處理過國際化問題，那麼你會知道需要採用哪些模式。因此，你更有可能做對。如果你處於這種情況下，添加預期的結構可能會更好，而不是當你對這個問題完全陌生時。因此，我的建議是，如果你確實知道該怎麼做，那麼你可以評估現在實現與將來再實現的成本差異。然而，如果你之前沒有做過這件事，不僅無法準確評估成本，你還可能無法做好。在這種情況下，應該等到後來再添加。如果你後來再添加，並且覺得很痛苦，那可能仍然比一開始就添加要好。因為你的團隊更有經驗，你對領域和需求的理解更深入。通常，在這種情況下，你可能會事後回想覺得早點添加會更容易，但實際上它可能比你想像的要困難得多。

This also ties into the question about the ordering of stories. In [Planning XP](https://www.amazon.com/gp/product/0201710919/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0201710919&linkCode=as2&tag=martinfowlerc-20), Kent and I openly indicated our disagreement. Kent is in favor of letting business value be the only factor in driving the ordering of the stories. After initial disagreement Ron Jeffries now agrees with this. I'm still unsure. I believe it is a balance between business value and technical risk. This would drive me to provide at least some internationalization early to mitigate this risk. However this is only true if internationalization was needed for the first release. Getting to a release as fast as possible is vitally important. Any additional complexity is worth doing after that first release if it isn't needed for the first release. The power of shipped, running code is enormous. It focuses customer attention, grows credibility, and is a massive source of learning. Do everything you can to bring that date closer. Even if it is more effort to add something after the first release, it is better to release sooner.

這也與故事排序的問題有關。在《[Planning XP](https://www.amazon.com/gp/product/0201710919/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0201710919&linkCode=as2&tag=martinfowlerc-20)》一書中，Kent 和我公開表達了我們的分歧。Kent 主張僅以業務價值作為驅動故事排序的唯一因素。經過最初的分歧後，Ron Jeffries 現在也同意這一點。而我仍不確定。我認為應在業務價值與技術風險之間取得平衡。這使我傾向於在早期就處理一些國際化工作，以降低這方面的風險。然而，這只有在國際化是首次發布所需時才成立。儘快達到可發布的狀態至關重要。如果首次發布不需要某項功能，那麼在首次發布後再添加任何額外的複雜性都是值得的。已經上線運行的代碼力量是巨大的。它能集中客戶注意力，提升可信度，並成為學習的巨大來源。竭盡所能縮短這個日期的到來。即使在首次發布後添加某項功能需要更多的努力，能夠更早發布仍然是更好的選擇。

With any new technique it's natural that its advocates are unsure of its boundary conditions. Most XPers have been told that evolutionary design is impossible for a certain problem, only to discover that it is indeed possible. That conquering of 'impossible' situations leads to a confidence that all such situations can be overcome. Of course you can't make such a generalization, but until the XP community hits the boundaries and fails, we can never be sure where these boundaries lie, and it's right to try and push beyond the potential boundaries that others may see.

對於任何新技術而言，其倡導者對其邊界條件感到不確定是很自然的。大多數 XP 實踐者曾被告知，針對某些問題，演進式設計是不可能的，然而最終他們發現這確實是可行的。對那些「不可能」情境的克服，帶來了一種信心，認為所有這類情境都可以被解決。當然，不能因此進行這樣的泛化，但在 XP 社群碰到邊界並失敗之前，我們無法確定這些邊界究竟在哪裡。嘗試突破其他人可能認為的潛在邊界是正確的選擇。

(A recent [article](https://martinfowler.com/ieeeSoftware/continuousDesign.pdf) by Jim Shore discusses some situations, including internationalization, where potential boundaries turned out not to be barriers after all.)

（最近由 Jim Shore 撰寫的一篇[文章](https://martinfowler.com/ieeeSoftware/continuousDesign.pdf)討論了一些情況，包括國際化問題，其中潛在的邊界最終並未成為真正的障礙。）

## Is Design Happening?

One of the difficulties of evolutionary design is that it's very hard to tell if design is actually happening. The danger of intermingling design with programming is that programming can happen without design - this is the situation where Evolutionary Design diverges and fails.

演進式設計的困難之一在於很難判斷設計是否真的在發生。設計與程式設計交織在一起的危險在於，程式設計可能在沒有設計的情況下進行 — 這正是演進式設計可能偏離並失敗的情況。

If you're in the development team, then you sense whether design is happening by the quality of the code base. If the code base is getting more complex and difficult to work with, there isn't enough design getting done. But sadly this is a subjective viewpoint. We don't have reliable metrics that can give us an objective view on design quality.

如果你在開發團隊中，你可以通過程式碼庫的質量來感知設計是否在進行。如果程式碼庫變得越來越複雜且難以使用，那麼進行的設計就不足夠。但遺憾的是，這是一個主觀的觀點。我們目前沒有可靠的指標能夠客觀地衡量設計品質。

If this lack of visibility is hard for technical people, it's far more alarming for non-technical members of a team. If you're a manager or customer how can you tell if the software is well designed? It matters to you because poorly designed software will be more expensive to modify in the future. There's no easy answer to this, but here are a few hints.

如果這種缺乏可見性對技術人員來說已經很困難，對於團隊中的非技術成員來說則更加令人擔憂。作為經理或客戶，你如何判斷軟體是否設計得好？這很重要，因為設計不佳的軟體在未來修改時將會更加昂貴。對此沒有簡單的答案，但以下是一些提示。

- Listen to the technical people. If they are complaining about the difficulty of making changes, then take such complaints seriously and give them time to fix things.
- Keep an eye on how much code is being thrown away. A project that does healthy refactoring will be steadily deleting bad code. If nothing's getting deleted then it's almost certainly a sign that there isn't enough refactoring going on - which will lead to design degradation. However like any metric this can be abused, the opinion of good technical people trumps any metric, despite its subjectivity.

- 聽取技術人員的意見。如果他們抱怨修改代碼的困難，就要認真對待這些抱怨，並給他們時間來修正問題。
- 關注被拋棄的程式碼數量。一個進行健康重構的專案將穩定地刪除劣質程式碼。如果沒有任何程式碼被刪除，這幾乎可以肯定是缺乏重構的跡象 — 這將導致設計退化。然而，與任何指標一樣，這也可能被濫用，優秀技術人員的意見比任何指標都更重要，儘管其主觀性很明顯。

## So is Design Dead?

Not by any means, but the nature of design has changed. XP design looks for the following skills

雖然並非完全如此，但設計的本質已經改變。XP設計尋求以下技能：

- A constant desire to keep code as clear and simple as possible
- Refactoring skills so you can confidently make improvements whenever you see the need.
- A good knowledge of patterns: not just the solutions but also appreciating when to use them and how to evolve into them.
- Designing with an eye to future changes, knowing that decisions taken now will have to be changed in the future.
- Knowing how to communicate the design to the people who need to understand it, using code, diagrams and above all: conversation.

- 不斷追求讓程式碼盡可能清晰簡單
- 重構技能，使你能在看到需要時自信地進行改進
- 對模式有良好的知識：不僅了解解決方案，還要懂得何時使用以及如何演進
- 以未來變更的眼光進行設計，知道現在做出的決策將來必須被更改
- 知道如何使用程式碼、圖表，並且最重要的是通過交談，向需要理解的人溝通設計

That's a fearsome selection of skills, but then being a good designer has always been tough. XP doesn't really make it any easier, at least not for me. But I think XP does give us a new way to think about effective design because it has made evolutionary design a plausible strategy again. And I'm a big fan of evolution - otherwise who knows what I might be?

這是一套令人生畏的技能，但一直以來成為一個優秀的設計師都很困難。XP並沒有真正讓它變得更容易，至少對我來說不是。但我認為XP確實為我們提供了一種思考有效設計的新方式，因為它使演進式設計再次成為一個可行的策略。而我是進化論的忠實擁護者 — 不然誰知道我可能會是什麼？