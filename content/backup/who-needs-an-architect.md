---
date: '2025-02-28T02:14:11+08:00'
title: 'Who Needs an Architect?'
tags: ["Martin Fowler"]
---
[Source](https://martinfowler.com/ieeeSoftware/whoNeedsArchitect.pdf)

Wandering down our corridor a while ago, I saw my colleague Dave Rice in a particularly grumpy mood. My brief question caused a violent statement, “We shouldn’t interview anyone who has ‘architect’ on his resume.” At first blush, this was an odd turn of phrase, because we usually introduce Dave as one of our leading architects. The reason for his title schizophrenia is the fact that, even by our industry’s standards, “architect” and “architecture” are terribly overloaded words. For many, the term “software architect” fits perfectly with the smug controlling image at the end of Matrix Reloaded. Yet even in firms that have the greatest contempt for that image, there’s a vital role for the technical leadership that an architect such as Dave plays. 

漫步在走廊上，我看到我的同事 Dave Rice 心情特別不好。我隨口問了一句，就引來他激烈的回應：「我們不應該面試任何履歷上有『架構師』的人。」乍聽之下，這句話有點奇怪，因為我們通常都把 Dave 當作我們頂尖的架構師之一來介紹。他對頭銜的矛盾情緒來自於，即使以我們這個產業的標準來說，「架構師」和「架構」都是意義過於氾濫的詞彙。對許多人來說，「軟體架構師」這個詞完全符合《駭客任務：重裝上陣》結尾那個自鳴得意、控制一切的形象。然而，即使在那些最鄙視這種形象的公司裡，像 Dave 這樣的架構師所扮演的技術領導角色仍然至關重要。

### What is architecture? 

When I was fretting over the title for Patterns of Enterprise Application Architecture (Addison-Wesley, 2002), everyone who reviewed it agreed that “architecture” belonged in the title. Yet we all felt uncomfortable defining the word. Because it was my book, I felt compelled to take a stab at defining it. 

當我為《企業應用程式架構模式》(Patterns of Enterprise Application Architecture，Addison-Wesley, 2002) 的書名煩惱時，所有審閱者都同意書名應該包含「架構」這個詞。然而，我們都對定義這個詞感到不安。因為這是我的書，我覺得不得不嘗試給它一個定義。

My first move was to avoid fuzziness by just letting my cynicism hang right out. In a sense, I define architecture as a word we use when we want to talk about design but want to puff it up to make it sound important. (Yes, you can imagine a similar phenomenon for architect.) However, as so often occurs, inside the blighted cynicism is a pinch of truth. Understanding came to me after reading a posting from Ralph Johnson on the Extreme Programming mailing list. It’s so good I’ll quote it all. 

我的第一步是避免含糊不清，直接讓我的玩世不恭暴露出來。在某種程度上，我將架構定義為一個詞，當我們想談論設計，但又想誇大它，使其聽起來很重要時，我們就會使用它。（是的，你可以想像架構師也存在類似的現象。）然而，就像經常發生的那樣，在充滿貶低意味的玩世不恭中，卻也隱藏著一絲真相。在閱讀了 Ralph Johnson 在 Extreme Programming 郵件列表上的一篇文章後，我才真正理解了它的含義。這篇文章寫得太好了，我想全部引用。

A previous posting said 

之前的一篇文章說：

The RUP, working off the IEEE definition, defines architecture as “the highest level concept of a system in its environment. The architecture of a software system (at a given point in time) is its organization or structure of significant components interacting through interfaces, those components being composed of successively smaller components and interfaces.” 

RUP 基於 IEEE 的定義，將架構定義為「系統在其環境中的最高層次概念。軟體系統（在給定的時間點）的架構是其透過介面互動的重要組件的組織或結構，這些組件由連續更小的組件和介面組成。」

Johnson responded: 

Johnson 回應道：

I was a reviewer on the IEEE standard that used that, and I argued uselessly that this was clearly a completely bogus definition. There is no highest level concept of a system. Customers have a different concept than developers. Customers do not care at all about the structure of significant components. So, perhaps an architecture is the highest level concept that developers have of a system in its environment. Let’s forget the developers who just understand their little piece. Architecture is the highest level concept of the expert developers. What makes a component significant? It is significant because the expert developers say so. 

我曾是使用該定義的 IEEE 標準的審閱者，我無用地爭辯說這顯然是一個完全錯誤的定義。系統沒有最高的層次概念。客戶擁有與開發人員不同的概念。客戶根本不關心重要組件的結構。所以，也許架構是開發人員對系統在其環境中的最高層次概念。讓我們忘記那些只了解自己小部分的開發人員。架構是專家開發人員的最高層次概念。什麼使組件變得重要？它之所以重要，是因為專家開發人員這麼說。

So, a better definition would be “In most successful software projects, the expert developers working on that project have a shared understanding of the system design. This shared understanding is called ‘architecture.’ This understanding includes how the system is divided into components and how the components interact through interfaces. These components are usually composed of smaller components, but the architecture only includes the components and interfaces that are understood by all the developers.”

因此，一個更好的定義是「在大多數成功的軟體專案中，參與該專案的專家開發人員對系統設計有共同的理解。這種共同的理解稱為『架構』。這種理解包括系統如何劃分為組件，以及組件如何透過介面進行互動。這些組件通常由更小的組件組成，但架構僅包括所有開發人員都理解的組件和介面。」

This would be a better definition because it makes clear that architecture is a social construct (well, software is too, but architecture is even more so) because it doesn’t just depend on the software, but on what part of the software is considered important by group consensus.

這會是一個更好的定義，因為它明確地指出架構是一種社會建構（嗯，軟體也是，但架構更是如此），因為它不僅取決於軟體，還取決於群體共識認為軟體的哪個部分是重要的。

There is another style of definition of architecture which is something like “architecture is the set of design decisions that must be made early in a project.” I complain about that one, too, saying that architecture is the decisions that you wish you could get right early in a project, but that you are not necessarily more likely to get them right than any other.

還有另一種架構定義風格，類似於「架構是在專案早期必須做出的設計決策集合」。我也抱怨這種定義，我說架構是那些你希望能在專案早期做對的決策，但你不一定比其他決策更有可能做對它們。

Anyway, by this second definition, programming language would be part of the architecture for most projects. By the first, it wouldn’t be.

無論如何，根據第二種定義，程式語言將是大多數專案的架構的一部分。根據第一種定義，它不會。

Whether something is part of the architecture is entirely based on whether the developers think it is important. People who build “enterprise applications” tend to think that persistence is crucial. When they start to draw their architecture, they start with three layers. They will mention “and we use Oracle for our database and have our own persistence layer to map objects onto it.” But a medical imaging application might include Oracle without it being considered part of the architecture. That is because most of the complication is in analyzing the images, not in storing them. Fetching and storing images is done by one little part of the application and most of the developers ignore it.

某件事物是否屬於架構的一部分，完全取決於開發人員是否認為它很重要。建構「企業應用程式」的人往往認為持久化至關重要。當他們開始繪製他們的架構時，他們會從三個層開始。他們會提到「我們使用 Oracle 作為資料庫，並擁有我們自己的持久化層，以將物件映射到它。」但醫療影像應用程式可能包含 Oracle，但不會被視為架構的一部分。這是因為大部分的複雜性在於分析影像，而不是儲存影像。提取和儲存影像是由應用程式的一小部分完成的，並且大多數開發人員會忽略它。

So, this makes it hard to tell people how to describe their architecture. “Tell us what is important.” Architecture is about the important stuff. Whatever that is.

所以，這使得很難告訴人們如何描述他們的架構。「告訴我們什麼是重要的。」架構是關於重要的東西。無論那是什麼。


> [!NOTE] 
> What makes a component significant? It is significant because the expert developers say so.
> 
> 什麼使組件變得重要？它之所以重要，是因為專家開發人員這麼說。

### The architect’s role 架構師的角色

So if architecture is the important stuff, then the architect is the person (or people) who worries about the important stuff. And here we get to the essence of the difference between the Matrix Reloaded species of architect and the style that Dave Rice exemplifies. 

所以如果架構是重要的東西，那麼架構師就是關心重要東西的人（或人們）。在這裡，我們開始接觸到《駭客任務：重裝上陣》類型的架構師和 Dave Rice 所代表的風格之間的本質區別。

Architectus Reloadus is the person who makes all the important decisions. The architect does this because a single mind is needed to ensure a system’s conceptual integrity, and perhaps because the architect doesn’t think that the team members are sufficiently skilled to make those decisions. Often, such decisions must be made early on so that everyone else has a plan to follow.

Architectus Reloadus (重裝上陣架構師) 是做出所有重要決策的人。架構師這樣做，因為需要單一的思維來確保系統的概念完整性，或許是因為架構師認為團隊成員沒有足夠的技能來做出這些決策。通常，這些決策必須在早期做出，以便其他所有人都有一個計劃可以遵循。

Architectus Oryzus is a different kind of animal (if you can’t guess, try www.nd.edu/~archives/latgramm.htm. This kind of architect must be very aware of what’s going on in the project, looking out for important issues and tackling them before they become a serious problem. When I see an architect like this, the most noticeable part of the work is the intense collaboration. In the morning, the architect programs with a developer, trying to harvest some common locking code. In the afternoon, the architect participates in a requirements session, helping explain to the requirements people the technical consequences of some of their ideas in nontechnical terms—such as development costs.

Architectus Oryzus (水稻架構師) 是一種不同的動物（如果你猜不出來，請試試 https://www.nd.edu/~archives/latgramm.htm。這種架構師必須非常清楚專案中發生的事情，注意重要的問題，並在它們變成嚴重問題之前解決它們。當我看到這樣的架構師時，最引人注目的工作是緊密的協作。早上，架構師與開發人員一起編寫程式，試圖收集一些通用的鎖定程式碼。下午，架構師參與需求會議，以非技術術語向需求人員解釋他們的一些想法的技術後果，例如開發成本。

In many ways, the most important activity of Architectus Oryzus is to mentor the development team, to raise their level so that they can take on more complex issues. Improving the development team’s ability gives an architect much greater leverage than being the sole decision maker and thus running the risk of being an architectural bottleneck. This leads to the satisfying rule of thumb that an architect’s value is inversely proportional to the number of decisions he or she makes.

在許多方面，Architectus Oryzus 最重要的活動是指導開發團隊，提高他們的水平，以便他們可以處理更複雜的問題。與成為唯一的決策者並因此面臨成為架構瓶頸的風險相比，提高開發團隊的能力為架構師提供了更大的槓桿作用。這導致了一個令人滿意的經驗法則，即架構師的價值與他或她做出的決策數量成反比。

At a recent ThoughtWorks retreat, some colleagues and I were talking about the issue of architects. I found it interesting that we quickly agreed on the nature of the job, following Architectus Oryzus, but we could not easily find a name. Architectus Reloadus is too common for us to be comfortable with “architect,” and it’s based on a flawed metaphor (see http://martinfowler.com/bliki/BuildingArchitect.html). Mike Two came up with the best name I’ve heard so far: guide, as in mountaineering. A guide is a more experienced and skillful team member who teaches other team members to better fend for themselves yet is always there for the really tricky stuff.

在最近的 ThoughtWorks 退修會上，我和一些同事正在談論架構師的問題。我發現很有趣的是，我們很快就達成了對工作性質的共識，遵循 Architectus Oryzus，但我們不容易找到一個名稱。Architectus Reloadus 太常見了，我們無法接受「架構師」這個詞，而且它基於一個有缺陷的隱喻（參見 [http://martinfowler.com/bliki/BuildingArchitect.html](http://martinfowler.com/ bliki/BuildingArchitect.html)。Mike Two 提出了我目前聽過的最好的名字：嚮導，就像登山運動中的嚮導一樣。嚮導是一位更有經驗和技能的團隊成員，他教導其他團隊成員更好地自力更生，但在真正棘手的事情上總是在那裡。

### Getting rid of software architecture 擺脫軟體架構

I love writing a startling heading, and the best, like this one, have an important meaning that’s not immediately obvious. Remember Johnson’s secondary definition: “Architecture is the decisions that you wish you could get right early in a project.” Why do people feel the need to get some things right early in the project? The answer, of course, is because they perceive those things as hard to change. So you might end up defining architecture as “things that people perceive as hard to change.”

我喜歡寫一個令人震驚的標題，而最好的標題，就像這個標題一樣，都有一個重要的含義，但不是立即顯而易見的。記住 Johnson 的第二個定義：「架構是那些你希望能在專案早期做對的決策。」為什麼人們覺得需要在專案早期就把某些事情做對？當然，答案是因為他們認為這些事情很難改變。所以你最終可能會將架構定義為「人們認為難以改變的事情」。

It’s commonly believed that if you are building an enterprise application, you must get the database schema right early on because it’s hard to change the database schema—particularly once you have gone live. On one of our projects, the database administrator, Pramod Sadalage, devised a system that let us change the database schema (and migrate the data) easily (see https://martinfowler.com/articles/evodb.html). By doing this, he made it so that the database schema was no longer architectural. I see this as an entirely good thing because it let us better handle change.

人們普遍認為，如果你正在建構一個企業應用程式，你必須在早期就把資料庫綱要做好，因為很難改變資料庫綱要——特別是當你已經上線後。在我們的一個專案中，資料庫管理員 Pramod Sadalage 設計了一個系統，讓我們可以輕鬆地更改資料庫綱要（並遷移資料）（參見 [https://martinfowler.com/articles/evodb.html](https://martinfowler.com/articles/evodb.html)。透過這樣做，他使資料庫綱要不再是架構性的。我認為這完全是一件好事，因為它讓我們更好地處理變更。

At a fascinating talk at the XP 2002 conference (https://martinfowler.com/articles/xp2002.html), Enrico Zaninotto, an economist, analyzed the underlying thinking behind agile ideas in manufacturing and software development. One aspect I found particularly interesting was his comment that irreversibility was one of the prime drivers of complexity. He saw agile methods, in manufacturing and software development, as a shift that seeks to contain complexity by reducing irreversibility— as opposed to tackling other complexity drivers. I think that one of an architect’s most important tasks is to remove architecture by finding ways to eliminate irreversibility in software designs.

在 XP 2002 會議（[https://martinfowler.com/articles/xp2002.html](https://martinfowler.com/articles/xp2002.html)）上的一個引人入勝的演講中，經濟學家 Enrico Zaninotto 分析了製造業和軟體開發中敏捷思想背後的基礎思維。我發現特別有趣的一個方面是他評論說，不可逆性是複雜性的主要驅動因素之一。他認為製造業和軟體開發中的敏捷方法是一種變革，旨在透過減少不可逆性來控制複雜性——而不是解決其他複雜性驅動因素。我認為架構師最重要的任務之一是透過尋找消除軟體設計中不可逆性的方法來移除架構。

Here’s Johnson again, this time in response to an email message I sent him:

這是 Johnson 再次發言，這次是回應我發給他的一封電子郵件：

One of the differences between building architecture and software architecture is that a lot of decisions about a building are hard to change. It is hard to go back and change your basement, though it is possible.

建築架構和軟體架構之間的一個區別是，關於建築物的許多決策都很難改變。很難回頭改變你的地下室，儘管這是可能的。

There is no theoretical reason that anything is hard to change about software. If you pick any one aspect of software then you can make it easy to change, but we don’t know how to make everything easy to change. Making something easy to change makes the overall system a little more complex, and making everything easy to change makes the entire system very complex. Complexity is what makes software hard to change. That, and duplication.

沒有理論上的理由說明軟體的任何方面都很難改變。如果你選擇軟體的任何一個方面，那麼你可以使它易於更改，但我們不知道如何使所有事物都易於更改。使某件事易於更改會使整個系統稍微複雜一些，而使所有事物都易於更改會使整個系統變得非常複雜。複雜性是使軟體難以更改的原因。還有，重複。

My reservation of Aspect-Oriented Programming is that we already have fairly good techniques for separating aspects of programs, and we don’t use them. I don’t think the real problem will be solved by making better techniques for separating aspects. We don’t know what should be the aspects that need separating, and we don’t know when it is worth separating them and when it is not.

我對面向切面程式設計的保留意見是，我們已經擁有相當好的技術來分離程式的切面，但我們沒有使用它們。我不認為真正的問題會透過開發更好的分離切面技術來解決。我們不知道哪些應該是需要分離的切面，我們也不知道何時值得分離它們，何時不值得。

Software is not limited by physics, like buildings are. It is limited by imagination, by design, by organization. In short, it is limited by properties of people, not by properties of the world. “We have met the enemy, and he is us.”

軟體不受物理定律的限制，就像建築物一樣。它受到想像力、設計、組織的限制。簡而言之，它受到人的特性的限制，而不是受到世界特性的限制。「我們遇到了敵人，他就是我們自己。」