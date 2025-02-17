---
date: '2025-02-17T13:23:57+08:00'
title: 'How to Develop Software Like Commanding a Tank'
tags: ["Gojko Adzic"]
---

[Source](https://gojko.net/2006/10/11/how-to-develop-software-like-commanding-a-tank/)

In [The Sources of Power](https://gojko.net/2006/10/07/sources-of-power/), Gary Klein describes his research of variations in understanding orders among commanders and tank platoon leaders, reaching conclusions that giving answers to ‘_what_’ and ‘_how_’ does not prepare individual teams for reacting to unforeseen problems. As I was reading this, it struck me that this mismatch is also present in software development. Most of the time we get or give answers to ‘_how_’ and ‘_what_’, and then spend enormous effort on coordination when problems arise, as people start pulling in different directions.

在[《力量的來源》](https://gojko.net/2006/10/07/sources-of-power/)中，加里·克萊恩描述了他對指揮官和坦克排長在理解命令上的差異進行的研究，並得出結論，僅僅回答「_什麼_」和「_如何_」並不能為團隊應對未預見問題做好準備。在閱讀這段內容時，我突然意識到這種不匹配在軟體開發中也存在。我們大多數時候得到或給予的答案是「_如何_」和「_什麼_」，然後當問題出現時，大家開始朝不同方向拉扯，投入大量精力進行協調。

Real-world projects are far from ideal - most of the time we have to compromise between cost, time, features and quality. Project stakeholders decide globally on trade-offs, but even junior programmers make those choices every day, on a smaller scale. When someone decides to spend a few more days testing the transit algorithm and covering every imaginable edge-case, he is following an idea that the best possible transit in some strange exceptional case is more important then earlier delivery. That might, or equally possible, might not be the case. You can hardly blame anyone for choosing the wrong option without actually having a framework on which to base an educated guess.

現實中的項目遠非理想狀態——大多數時候，我們不得不在成本、時間、功能和質量之間妥協。項目利益相關者在全球範圍內做出權衡決策，但即使是初級程式員每天也會在小範圍內做出這些選擇。當有人決定花幾天時間測試傳輸算法並涵蓋每一個可以想像的邊界情況時，他遵循的是一種想法，即某些特殊情況下最佳的傳輸比提前交付更重要。這可能是，也可能不是這樣。如果沒有一個可以基於教育猜測的框架，你很難責怪任何人選擇了錯誤的選項。

Dr. Klein and his researches reviewed various _Commander’s intent_ documents, reaching the conclusion that they should be more focused on goals and contain ‘_why_’ and ‘_watch out for_’, in order to give individual platoon leaders enough information to improvise and cut corners when needed.

克萊恩博士及其研究團隊審查了各種「_指揮官意圖_」文件，得出結論，這些文件應該更多地關注目標，並包含「_為什麼_」和「_小心_」，以便給個別排長足夠的信息來即興發揮和必要時縮短流程。

## Applying to software development

Focusing on goals is a well known technique for setting up a discussion framework with clients or stakeholders. However, goals are rarely passed on – instead they transform into specifications and requirements. Goals surface only when they become a code-red priority (‘_must be live before Expo opens on Monday_’) or after the initial delivery failed to meet client’s expectations. Even then, they are again passed on wrapped into a list of tasks or requests.

關注目標是一種眾所周知的技術，用於與客戶或利益相關者設置討論框架。然而，目標很少被傳遞下去——相反，它們會轉化為規格和需求。目標只有在它們成為代碼紅色優先級（「_必須在週一展覽會開幕前上線_」）或初步交付未能滿足客戶期望後才會浮現出來。即使這樣，它們也會再次以任務或請求列表的形式傳遞下去。

If communicating goals and anti-goals helped to coordinate tank platoon leaders in low-visibility desert conditions, it should also help programmers whose biggest optical obstacle is an occasional lack of caffeine in the veins. I decided to test that theory. For the past twelve months, project goals have found their way into everything from wiki sections, to power-point presentations, requirements and specifications, and I have made sure that everybody, down to the youngest junior involved, knows why we are doing something.

如果溝通目標和反目標能夠幫助在低能見度沙漠條件下協調坦克排長，那麼它也應該能幫助那些最大視覺障礙是偶爾缺乏咖啡因的程式員。我決定測試這一理論。在過去的十二個月裡，項目目標被包含在從維基部分到演示文稿、需求和規格的各個方面，我確保每個人，甚至是最年輕的初級員工，都知道我們為什麼要做某件事。

The other part of Dr. Klein’s suggestion, anti-goals, was a bit more tricky. Certain problems like changing requirements or resource issues can hardly be called unexpected - we just don’t know specifics of those issues, but they will undoubtedly happen. So, we build in some slack into the plans in advance - most software development methods minimise or mitigate the risk of such issues in one way or another. Since it’s not very likely that Luftwaffe will carry out a surprise attack our database team, this ‘_watch out for_’ advice on a high level typically consisted of a small list of pitfalls - either identified during prototyping or design brainstorming. It was much easier to identify ‘_watch out for_’ on a lower level, related to individual tasks - so in every hand-off I would try to devote at least ten minutes to discuss possible technical problems.

克萊恩博士建議的另一部分——反目標——有點棘手。一些問題，如需求變更或資源問題，幾乎不能稱為意外——我們只是不知道這些問題的具體細節，但它們無疑會發生。因此，我們提前在計劃中內置了一些餘地——大多數軟體開發方法以某種方式最小化或減輕了這類問題的風險。由於不太可能有意外的空襲我們的數據庫團隊，這種高層次的「_小心_」建議通常包括一個小列表，這些坑是在原型或設計頭腦風暴期間識別出來的。在較低層次，與個別任務相關的「_小心_」更容易識別——所以在每次交接中，我都會嘗試至少花十分鐘討論可能的技術問題。

## Improved communication

It is very hard to quantify the effect of this technique, since software projects are not developed in lab conditions. Even if we could set up two exactly same teams, with same skill sets and mindsets, put them to work on identical projects, one communicating goals and other communicating only tasks, I do not think that the results would be relevant in the real world. Change, at least in my software projects, is mostly driven by external factors, so projects developed in isolation would not face the same challenges. Though I cannot give you some nice round figure followed by a percentage sign and preceded by a fancy three letter acronym like ROI, my subjective feeling is that this experiment was a great success.

很難量化這種技術的效果，因為軟體項目不是在實驗室條件下開發的。即使我們能夠設置兩個完全相同的團隊，擁有相同的技能組合和心態，讓他們在相同的項目上工作，一個團隊溝通目標，另一個只溝通任務，我也不認為結果在現實世界中具有相關性。在我的軟體項目中，變化主要由外部因素驅動，因此在隔離中開發的項目不會面臨相同的挑戰。雖然我不能給你一個好看的數字，後面加上一個百分比符號，前面加上一個花哨的三字母縮寫如ROI，但我的主觀感覺是這次實驗非常成功。

The biggest difference with junior programmers is less wandering into the wilderness. After hitting a programming equivalent of the brick wall, it was typical for them to just slide from the main road and start working on utterly complex workarounds, until somebody discovers what they are doing or until they just give up and ask for help. As we have started talking about some of those brick walls before the journey, there was a lot less of searching through the woods.

與初級程式員相比，最大的不同是他們較少迷失在未知領域。在遇到程式設計等效的磚牆後，他們通常會偏離主路，開始處理極其複雜的變通方案，直到有人發現他們在做什麼，或者他們放棄並尋求幫助。由於我們在旅程之前已經開始討論其中一些磚牆，搜尋困難解決方案的情況大大減少。

With senior colleagues, there was not much difference. They would know the overall goals anyway, and would understand the pitfalls - having a list of goals readily available all the time just made it a bit easier to communicate, as it set a clear platform for adding or removing features (_Should we do a new management console? Does it help any of the goals? If not, scrape it!_).

對於資深同事來說，差別不大。他們無論如何都會知道總體目標，並且會理解陷阱——隨時備有目標清單只是讓溝通稍微容易了一點，因為它設置了一個清晰的平台來添加或刪除功能（「_我們應該做一個新的管理控制台嗎？它有助於實現任何目標嗎？如果沒有，刪掉它！_」）。

The effect was most noticeable with mid-level developers. First, people started reporting problems early - which is a very, very good thing. Sometimes they found a genuine problem, sometimes they just misunderstood the original idea, but in any case there was a lot less unpleasant surprises later. I guess that everybody could compare their understanding of ideas and specifications with the framework set by overall goals, and quickly notice any conflicts. Knowing goals probably helped them to see the big picture (or at least see a bigger part of it), and make sure ‘_what_’ and ‘_how_’ are understood consistently throughout the teams.

效果在中階開發人員中最為顯著。首先，人們開始早早報告問題——這是一件非常非常好的事情。有時候他們確實發現了真正的問題，有時候他們只是誤解了原來的想法，但無論如何，後來的不愉快驚喜少了很多。我猜每個人都可以將自己對想法和規格的理解與整體目標設定的框架進行比較，並迅速注意到任何衝突。了解目標可能幫助他們看到整體大局（或至少看到更大的一部分），並確保「_什麼_」和「_如何_」在整個團隊中得到一致的理解。

Even more important was the fact that people, who were previously just ‘_doing their job_’, started to get much more involved. I guess that knowing goals was received as a sign of personal recognition or respect and an invitation to give their opinion on things. There was much more feedback then usual - including some very good ideas.

更重要的是，那些之前只是「_做他們的工作_」的人，開始變得更為投入。我猜，了解目標被視為個人認可或尊重的標誌，並是一個對事情發表意見的邀請。反饋比平時多得多——包括一些非常好的想法。

More communication is consistent with Klein’s final addition to _Commander’s intent_: ‘_Now, talk to me_’.

更多的溝通與克萊恩的「_指揮官意圖_」中的最後補充一致：「_現在，和我談談_」。

> The Modified Commander’s intent template from Sources of Power:
> 
> - Here’s what I think we face
> - Here’s what I think we should do
> - Here’s why
> - Here’s what we should keep our eye on
> - Now, talk to me
>   
> 來自《力量的來源》的修改版指揮官意圖模板：
>
> - 這是我認為我們面對的
> - 這是我認為我們應該做的
> - 這是為什麼
> - 這是我們應該注意的
> - 現在，和我談談

## Important challenges

Though the technique sounds very simple, it’s not without challenges. The greatest one was identifying and expressing true goals, not just repeating specifications or requirements. ‘_Our aim is to develop a transaction server_’ is not goal, but ‘_our aim is to enable the system to serve ten times more customers_’ is. However, it is hard to distinguish between those two in the beginning. If goals just repeat the specifications or requirements, then the effort of communicating them throughout the organisation is pointless. This problem was also observed by Dr. Klein in his analysis of tank commanders’ orders. My rule of thumb is that a goal must be an answer to a real-world problem or customer benefit, and not suggest the solution. I consider replacing anything that might provoke a question like ‘_but why?_’ with an answer to that question. On the other hand, taking that to the extreme of including ‘_making more money_’ or ‘_beating the competition_’ into project goals is useless. True goals lie somewhere between customer’s mission statement and their project vision.

儘管這種技術聽起來很簡單，但它並非沒有挑戰。最大的挑戰是確定和表達真正的目標，而不是僅僅重複規格或需求。「_我們的目標是開發一個交易伺服器_」不是目標，但「_我們的目標是使系統能夠服務於十倍的客戶_」是。然而，一開始很難區分這兩者。如果目標只是重複規格或需求，那麼在整個組織中傳達它們的努力是無意義的。這個問題也在克萊恩博士的坦克指揮官命令分析中得到了觀察。我的經驗法則是，目標必須是對現實世界問題或客戶利益的答案，而不是建議解決方案。我認為將可能引發「_但為什麼？_」問題的任何內容替換為該問題的答案。另一方面，將「_賺更多錢_」或「_擊敗競爭對手_」等極端的內容納入項目目標是無用的。真正的目標位於客戶的使命聲明和他們的項目願景之間。

The second big challenge was explained by Scott Berkun in his book [The Art of Project Management](http://www.amazon.com/gp/product/0596007868?ie=UTF8&tag=swingwiki-20&linkCode=as2&camp=1789&creative=9325&creativeASIN=0596007868) (another very strong case for of focusing on goals but from another perspective - I will publish the review of that book along with the sequel to this article next week). True goals are consolidated and prioritised and a single project should not have too many goals. It’s very hard to get everyone to agree on less than 10 goals and even harder to identify 2-3 ‘_must haves_’ and consider the rest as ‘_nice to have_’. Clients will tend to define too much goals and all of them will be ‘_must have_’ - but identifying non critical ‘_nice to have_’ objectives is necessary in order to provide a framework for correct compromises. However, putting the goal list on paper actually gives you some leverage in negotiating, since it becomes obvious when the project is too broad or if some goals are unrealistic or conflicting.

第二個大挑戰是由斯科特·貝昆在他的書[《項目管理藝術》](http://www.amazon.com/gp/product/0596007868?ie=UTF8&tag=swingwiki-20&linkCode=as2&camp=1789&creative=9325&creativeASIN=0596007868)中解釋的（另一個強有力的論點是從另一個角度關注目標——我將在這篇文章的續集中與該書的評論一起發佈）。真正的目標是整合和優先排序的，一個項目不應有太多目標。讓每個人都同意少於10個目標非常困難，而識別2-3個「_必須擁有_」並將其餘的視為「_可有可無_」則更難。客戶往往會定義過多的目標，而且所有的目標都將是「_必須擁有_」——但識別非關鍵的「_可有可無_」目標是必要的，以提供一個正確妥協的框架。然而，將目標清單寫在紙上實際上為你在談判中提供了一些槓桿，因為當項目過於廣泛或某些目標不現實或相互衝突時，這一點會變得顯而易見。

The third big challenge was to actually pass on the goals throughout the organisation without overloading people with information. The project as a whole might have 10 goals, but each sub-project or thread will have it’s own set of objectives on a lower level. Several thread goals will typically map into one main project goal, and thread objectives might also express benefits for the solution provider, not just the client. ‘_Enabling easy integration of third-party e-wallets_’ is an internal goal of the software provider that will speed up later development, but helps the overall customer goal of ‘_supporting all popular payment gateways_’. People working on a single module should be concerned with overall objectives and goals of their project thread. Overloading developers with goals of other threads will just introduce confusion. Not defining thread-specific aims is equally bad, since those individual sub-projects will not have a focused goal framework.

第三個大挑戰是實際上將目標傳遞給整個組織，而不會讓人們信息過載。整個項目可能有10個目標，但每個子項目或線程將在較低層次上有自己的目標。一些線程目標通常會映射到一個主要項目目標，而線程目標也可能表達對解決方案提供者的益處，而不僅僅是客戶。「_實現第三方電子錢包的輕鬆集成_」是軟體提供商的內部目標，將加速後續開發，但有助於實現客戶的整體目標「_支持所有流行的支付網關_」。從事單個模塊工作的員工應關注其項目線程的整體目標和目標。用其他線程的目標讓開發人員信息過載只會引入混亂。不定義線程特定的目標同樣糟糕，因為那些個別子項目將沒有集中的目標框架。

## Focusing on goals pays off

I find this technique very positive in practice, living up to the theoretical promise. Though it is by no means a magical ACME powder that will help you conquer all the software problems just by adding water, it improves understanding and helps to establish a common framework for communication. The amount of effort required to implement it is a small price to pay for making the project cruise somewhat easier.

我發現這種技術在實踐中非常積極，與理論承諾一致。儘管這絕不是一種神奇的ACME粉，只需加水即可幫助你征服所有的軟體問題，但它改善了理解，並有助於建立一個共同的溝通框架。實施這種技術所需的努力是為使項目稍微順利進行而付出的微不足道的代價。