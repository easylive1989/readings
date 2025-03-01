---
date: '2025-02-08T09:02:27+08:00'
title: 'What if We Rotate Pair Every Day?'
tags: ["Martin Fowler"]
---

[Source](https://martinfowler.com/articles/rotate-pairs-experiment.html)

#### Unveiling the benefits of frequent pair rotation through an experiment

#### 透過實驗揭示頻繁配對輪換的優勢

_Benefits of pair programming are widely accepted but advice around pair rotation remains controversial. When and how frequently should teammates rotate pairs? And… What if we rotate pairs every day? We worked with three teams through an exercise of daily pair rotation. We developed a lightweight methodology to help teams reflect on the benefits and challenges of pairing and how to solve them. Initial fears were overcome and teams discovered the benefits of frequently rotating pairs. We learned that pair swapping frequently greatly enhances the benefits of pairing. Here we share the methodology we developed, our observations, and some common fears and insight shared by the participating team members._

_配對程式設計的優勢已被廣泛接受，但關於配對輪換的建議仍存在爭議。隊友應該何時輪換？輪換的頻率應該是多少？如果我們每天都輪換配對會發生什麼？我們與三個團隊合作，進行了一項每日配對輪換的實驗。我們開發了一種輕量級的方法論，幫助團隊反思配對的優勢與挑戰，以及如何解決這些挑戰。最初的擔憂逐漸被克服，團隊成員發現頻繁輪換配對的優勢。我們學到，頻繁的配對交換能夠大幅提升配對編程的好處。在此，我們分享我們所開發的方法論、觀察結果，以及參與成員提出的一些常見擔憂與見解。_

---

The majority of developers work solo. Tasks are commonly assigned to single individuals in a practice that is called “solo coding”. Developers that practice solo coding are often isolated in silos that prevent knowledge sharing across the team. These silos also make it difficult for team members to bond and create personal relationships, especially in a remote working environment. Onboarding of new team members is complicated and the establishment of quality gates like code reviews result in a bottleneck for delivery efficiency. In addition, binding the work to individual team members also creates a risk for whenever this person leaves the team (eg. vacations or sick leave). Finally, individuals eventually become owners of regions of the system and the person to go to for feature-specific knowledge.

大多數開發人員都是獨自工作，這種做法被稱為「單人編碼」（solo coding）。在單人編碼的環境中，任務通常是指派給單個個體，導致開發人員被隔離在自己的「孤島」中，無法與團隊成員共享知識。這些孤島化的現象也會阻礙團隊成員之間的交流與個人關係的建立，特別是在遠端工作環境下更為明顯。此外，新成員的加入變得更為複雜，而像程式碼審查（code review）這類品質管控機制則可能成為交付效率的瓶頸。此外，當特定工作被綁定於個別成員時，一旦該成員離開團隊（如休假或生病），團隊將面臨風險。最終，個別開發人員可能成為系統某些區域的「所有者」，變成該領域的唯一知識來源。

Pair programming is a viable alternative to solo coding. [On Pair Programming](https://martinfowler.com/articles/on-pair-programming.html) explores its benefits and challenges. When developing in pairs, people can work closely together with the goal to constantly share knowledge and information. This leads to better refinement of stories because everyone will have the necessary context to contribute. Also, there is no need for specific code review processes since all code is being reviewed ​​on the fly. Pairing creates more opportunities for people to know each other and develop personal bonds thus increasing team’s cohesion. Pairing processes should be accompanied by a periodic pair rotation ceremony so that pair switching can happen. This enables people to experience working with everyone in the team. After this ceremony developers should share the current tasks’ context and progress with the new pair so that the delivery flow can continue.

配對程式設計（Pair Programming）是單人編碼的可行替代方案。[《論配對程式設計》（On Pair Programming）](https://martinfowler.com/articles/on-pair-programming.html) 探討了其優勢與挑戰。透過配對編程，開發人員可以緊密合作，持續共享知識與資訊，讓每個人都能掌握必要的背景知識，以便更好地完善開發需求。此外，由於所有程式碼都能即時獲得審查，便無需額外的程式碼審查流程。配對編程還能促進團隊成員之間的相互了解與個人聯繫，提高團隊凝聚力。為了確保配對機制的順暢進行，應該定期舉行「配對輪換儀式」，讓團隊成員能與不同夥伴合作。在輪換儀式後，開發人員需要向新的配對夥伴分享當前任務的背景與進展，以確保開發流程的連續性。

The frequency of pair rotations can vary between teams. Even though frequent pair rotations are preferred in order to maximize the benefits of pairing, some teams have reported that rotating pairs frequently creates friction. There is a perception that rotating pairs every day, or every other day, is more costly and more difficult than rotating once a week. On the other end of the spectrum, there are also teams which rotate pairs once a month. This means an individual would take at least 5 months to pair with other 5 people in the team at least once, assuming no repeated pairs during this period. Another routine is when pairs rotate only when they finish a task, which makes the frequency indeterminate. It is also not practical to rotate pairs on task completion since it is unlikely that other pairs finish at the same time.

配對輪換的頻率可依團隊需求而有所不同。儘管較高頻率的輪換有助於發揮配對編程的最大優勢，但某些團隊發現，過於頻繁的輪換可能會產生阻力。例如，每天或每兩天輪換一次，可能比每週輪換一次更具挑戰性與成本負擔。在另一個極端，有些團隊則選擇每月輪換一次，這表示成員至少需要五個月的時間才能與團隊內的五名其他成員配對一次（假設期間沒有重複的配對）。另一種做法則是根據任務完成時間進行輪換，但這樣的方式缺乏固定頻率，且難以確保所有配對能夠同時完成任務。

We started noticing that teams with infrequent pair rotations tend to present similar symptoms seen in teams that do solo coding. Long-lived pairs start to become “partners in crime”. Context sharing gets harder the longer it takes for pair switching to happen: Developers need to share all the context from the previous month with a new pair in the context of monthly rotations. We had evidence that our pair switching practice wasn't yielding the desired outcomes, so we decided to run an experiment with the goal to improve team performance through pairing best practices.

我們開始發現，配對輪換頻率較低的團隊，往往會出現與單人編碼類似的問題。例如，長時間不變更的配對關係可能演變為「共犯關係」，導致知識傳遞變得更加困難。以每月輪換一次為例，開發人員在更換夥伴時，需向新夥伴完整傳達過去一個月的所有背景資訊，這無疑增加了溝通負擔。我們意識到，現行的配對輪換方式未能達到理想的效果，因此決定進行一項實驗，以最佳配對實踐來提升團隊的整體效能。
## Our Experiment

We decided to challenge teams that practiced infrequent pair rotations to radically increase this frequency as part of an experiment. What if for two weeks we rotated pairs every day? What were the difficulties found during this time, and what can we do to address them? Did we reap the benefits of pairing during this time? Going forward, does the team want to keep rotating pairs every day or go back to the previous frequency?

我們決定挑戰那些配對輪換頻率較低的團隊，讓他們在實驗期間大幅提高輪換頻率。我們提出了一個假設：如果在兩週內每天更換配對會如何？在這段時間內，團隊遇到了哪些困難？我們可以如何解決這些問題？我們是否在這段期間真正體驗到了配對編程的優勢？未來，團隊是否願意繼續每天輪換配對，還是想回到先前的頻率？

We developed an exercise designed to help a team explore frequent pair rotation and make critical analysis of its impact. The exercise begins with a one hour, facilitated whiteboarding session, during which the team members write up and discuss their thoughts on the following three questions:

我們設計了一個練習，幫助團隊探索高頻率配對輪換，並對其影響進行深入分析。這個練習從一場為時一小時的引導式白板會議開始，團隊成員在會議中寫下並討論以下三個問題：

- Why is pairing valuable?
- What makes pairing difficult?
- What makes pairing easy?

- 配對編程的價值在哪裡？
- 配對編程有哪些挑戰？
- 什麼情況下配對編程變得更容易？

These questions are presented in order. The team has three minutes to post answers for each question on the board and seven minutes to discuss what they have shared.

這些問題按照順序提出，每個問題有三分鐘的時間讓成員在白板上貼出回應，接著進行七分鐘的討論。

![](https://martinfowler.com/articles/rotate-pairs-experiment/mural-board.png)

For the following days of the exercise the team continues working on their backlog while rotating pairs every day. For any task in progress one member of the pair stays with the task as “anchor” while the other rotates onto another task. “Anchors” of a task rotate every other day, ensuring that no team member will work on a single task for more than two days consecutively.

在接下來的數天裡，團隊持續處理待辦事項（backlog），並每天更換配對。對於進行中的任務，每組配對中的一名成員會留下來擔任「錨點」（anchor），確保任務的持續性，而另一名成員則輪換至另一項任務。「錨點」每兩天輪換一次，確保沒有任何成員會連續超過兩天參與同一個任務。

The team meets every morning for 30 minutes on a whiteboard session with the following three questions:

每天早上，團隊會舉行 30 分鐘的白板會議，並探討以下三個問題：

- What makes pairing difficult?
- What makes pairing easy?
- What practices should we try today, to make our pairing easier and more effective?

- 配對編程有哪些挑戰？
- 什麼情況下配對編程變得更容易？
- 今天我們應該嘗試哪些做法，使配對過程更順暢、更有效？

These questions are presented in order, each with three minutes to post ideas on the board and five minutes to discuss. When this is finished, the team identifies anchors for each task in progress and facilitates the assignment of new pairs.

這些問題按照順序提出，每個問題有三分鐘的時間貼出想法，接著五分鐘進行討論。討論結束後，團隊會確認每項進行中任務的「錨點」，並安排新的配對組合。

We facilitated this daily retrospective using the same board every day, with a unique color of sticky for each day. This allowed the team members to see the points raised in each area on each day, resulting in a visualization of the team’s learning and critical thinking throughout the week.

我們使用相同的白板來記錄每日的回顧，並為每一天使用不同顏色的便利貼。這樣，團隊成員可以清楚地看到每天提出的重點，進而視覺化團隊在這一週內的學習與思考過程。

On the last day of the exercise we facilitated the final whiteboard session, and then asked the team to decide on a pair rotation frequency to continue. We then encouraged the team to continue to revisit their pair rotation frequency in future team retrospectives.

在最後一天的練習中，我們進行了最終的白板會議，並請團隊決定未來的配對輪換頻率。我們鼓勵團隊在日後的回顧會（retrospective）中，持續檢討並調整適合自己的配對輪換頻率。

## Results of our Experiment

During 2022 - 2023 we engaged three separate teams to try this experiment for one week each. Each of these teams were fully distributed, working together online but never in person. Two of these teams were collocated between the US and Brazil.

在 2022 至 2023 年間，我們與三個不同的團隊合作，讓他們各自嘗試為期一週的實驗。這些團隊都是完全分散式的，成員僅在線上合作，從未實體見面。其中兩個團隊的成員分布於美國與巴西之間。

Each team raised similar concerns at the start of the experiment. In the first section below we share some of those concerns and describe how the teams’ position evolved over the course of the experiment. The second section presents some feedback that displays the realized benefits of pairing and frequent pair rotations.

在實驗開始時，各團隊都提出了相似的疑慮。以下的第一部分分享了這些擔憂，以及團隊在實驗過程中的觀點轉變。第二部分則整理了一些回饋，展現了配對編程與高頻率配對輪換所帶來的實際效益。

All teams that participated in our experiment used systems like Jira or Trello to document and track work items, and all used the term “card” to describe a record in that system. The following feedback and results use the word “card” in this sense.

所有參與實驗的團隊都使用 Jira 或 Trello 等系統來記錄和追蹤工作項目，並且統一使用「卡片」（card）一詞來指代這些系統中的紀錄。因此，以下的回饋與結果中皆沿用「卡片」這個詞彙。

### What makes pairing hard and how the perceptions changed

#### “Lack of empathy, alignment and communication makes pairing difficult”

#### 「缺乏同理心、對齊與溝通，使配對變得困難」

Frequent pair rotation can be a powerful tool in building stronger team dynamics. Initially, a lack of empathy and alignment can make pairing challenging, especially when team members are unfamiliar with each other's working patterns, pace, and areas of expertise. However, by switching pairs frequently, team members have the opportunity to get to know one another better, and quickly. This familiarity makes it easier to empathize and align with each other, ultimately fostering stronger bonds within the team. Moreover, the practice of frequent pair rotation encourages a culture of feedback. We suggested that team members intentionally share feedback during short sessions at the end of their pairing sessions, contributing to continuous improvement and better collaboration.

高頻率的配對輪換是一種強大的團隊動態建立工具。然而，在初期，團隊成員可能會因為彼此的工作方式、節奏和專業領域不同，而難以對齊和產生同理心，使得配對變得具有挑戰性。但透過頻繁的配對輪換，成員能夠更快地熟悉彼此，進而降低這些阻礙。這種熟悉度有助於提升團隊成員之間的同理心和對齊程度，最終促進更緊密的團隊關係。此外，高頻率的配對輪換也鼓勵了一種持續回饋（feedback）的文化。我們建議團隊在每次配對結束後安排簡短的回饋交流時間，讓成員分享心得與建議，進一步提升合作效率與團隊協作能力。

#### “There are a lot of interruptions to pairing time”

#### 「配對過程中有太多中斷」

Teams reported challenges in pairing due to frequent interruptions caused by a lack of long periods of uninterrupted working time. To address this issue, the teams established core working hours in the afternoon during which interruptions are minimized. As a result, meetings got shifted to the morning or the end of the day. Additionally, pairs within the team utilized the Pomodoro Technique or other explicit timeboxing methodology to maximize their efficiency and productivity during their limited working time.

團隊普遍反映，由於缺乏長時間不受干擾的工作時段，配對過程常被打斷。為了解決這個問題，各團隊制定了「核心工作時段」，通常安排在下午，以最大程度減少中斷。會議則改為上午或一天結束前進行。此外，配對成員也採用了番茄工作法（Pomodoro Technique）或其他時間區塊管理（timeboxing）方法，以提高有限時間內的工作效率與生產力。

#### “Switching pairs everyday makes us slower”

#### 「每天更換配對會降低開發速度」

There is a notion that increasing the frequency of rotations results in a decline in delivery performance, as perceived by the product team. They tend to believe that more rotation leads to reduced efficiency and slower output.

產品團隊普遍認為，提高配對輪換的頻率會導致交付效能下降。他們傾向認為，更頻繁的輪換會降低效率，進而減緩開發進度。

There also exists a developer perception that frequent rotations introduce additional overhead, consequently slowing down the team. This is attributed to the need to consistently share the evolving context of ongoing work, which is perceived as a time-consuming process.

此外，開發人員也有一種認知，認為頻繁的輪換會帶來額外的負擔，從而拖慢團隊的步調。這主要歸因於，每次更換配對時，都需要重新傳遞當前工作的背景資訊，而這被視為一個耗時的過程。

However, proponents of more frequent rotations argue that sharing context becomes more efficient as the frequency increases. This is attributed to the fact that there is typically less contextual information to communicate if pair switching is done frequently. Moreover, the efficiency of sharing context is further enhanced when every team member possesses a more comprehensive understanding of ongoing tasks. In addition, frequent pair switches creates an opportunity for team members to establish processes to facilitate context sharing.

然而，支持高頻率輪換的人則認為，隨著輪換頻率的提高，背景資訊的共享反而變得更加高效。這是因為，如果經常更換配對，每次需要傳遞的背景資訊就會相對較少。此外，當每位團隊成員對正在進行的任務有更完整的了解時，資訊共享的效率也會進一步提升。更頻繁的配對輪換還能促使團隊建立更有效的資訊共享機制，使得輪換過程更加順暢。

The practice of frequent rotation becomes more manageable and streamlined over time. As the team becomes accustomed to this approach, the initial challenges associated with frequent rotation diminish, making the process progressively easier and more effective.

隨著團隊逐漸適應這種工作方式，頻繁輪換所帶來的挑戰也會逐步減少，使得整體流程變得更加可控且高效。

### The experienced benefits of frequent pair rotation

#### “Context sharing is easy and quick when you do it more often”

#### 「當你更頻繁地共享背景資訊時，這件事就變得簡單又快速」

One concern that we heard from all three teams was that swapping pair members on work in progress would lead to a problem of sharing context with the new pair member. In fact, for each team this seemed to be the strongest motivation for long-lived pairs.

我們從所有三個團隊中聽到的一個主要擔憂是，在進行中的任務中更換配對成員會導致新的配對成員無法迅速掌握工作背景。事實上，這也是各團隊選擇長時間固定配對的最主要原因。

In each team’s board we found that this concern would be raised in the first couple of days. Team members would suggest common ways to make context sharing easier, and by the end of the experiment it was no longer a concern. A practice that emerged in each team was to have pairs end their day by adding a note to the card itself, briefly capturing the work and decisions completed that day. They might also add or remove items from a to-do list also maintained in the card. These simple practices helped the card itself to carry the context of the work in progress, rather than having that context reside with specific team members.

我們發現，在每個團隊的看板上，這種擔憂通常會在實驗的前幾天被提出。團隊成員會討論如何讓背景資訊共享變得更容易，而到了實驗結束時，這個問題已經不再是困擾。每個團隊都自然地採取了一種做法，即在每天結束時，配對成員會在「卡片」上添加一則簡要筆記，記錄當天完成的工作與決策。他們也可能增減待辦事項清單，以確保卡片本身能夠承載當前工作的背景資訊，而不需要完全依賴個別團隊成員的記憶。

We found that each team discovered new practices related to the cards. In our daily discussions the team members would ask for more context to be held in the card, smaller cards, and ongoing comments in the cards.

我們發現，每個團隊在實驗期間都發展出了新的卡片管理方式。在每日討論中，團隊成員會希望在卡片中保留更多背景資訊，縮小卡片範圍，並且在卡片上保持持續的評論與更新。

#### “Information is flowing through the team”

#### 「資訊在團隊內部流動起來了」

This is one of the more exciting and insightful comments we heard. Teams discovered that, in practice, it did not take very long for an anchor to share context with a new pair at the start of a coding session. There was not a lot of new context to share. Also, teams found it was easier to understand any card after working on many other cards of the team’s backlog. Frequent pair rotations accelerate this experience gain as team members are able to work on a wider variety of tasks every week.

這是我們聽到的一個非常令人振奮的回饋。團隊發現，實際上，在開始新的配對編程時，主要負責人（anchor）向新配對成員傳遞背景資訊並不需要花太長時間，因為沒有太多新的背景需要額外說明。此外，團隊成員發現，在處理多張不同的卡片後，他們能更快地理解任何新的卡片。高頻率的配對輪換加速了這種學習經驗的積累，使得團隊成員每週都能參與更多樣化的任務。
#### “Knowledge silos are impossible to maintain”

#### 「知識孤島已無法存在」

Each team included members of different experience levels and areas of expertise. The teams initially thought of this diversity as a challenge for frequent pair rotations. Prior to the experiment, each team was organizing pairs and the cards assigned to pairs with consideration of who is a junior or senior team member, who is a front-end, back-end or devops specialist, who has prior experience working in a particular area of the codebase, and so on. Maintaining this complex matrix made it difficult to switch pairs frequently, and reinforced knowledge silos in the team.

每個團隊的成員都有不同的經驗水平和專業領域。團隊在實驗開始前普遍認為，這種多樣性會使頻繁的配對輪換變得困難。在實驗之前，團隊會根據成員的資歷（如資深或初級開發人員）、技術專長（如前端、後端或 DevOps），以及對特定代碼區域的熟悉程度來分配配對和卡片。這種複雜的匹配方式使得配對難以頻繁更換，進而強化了團隊內部的知識孤島現象。

It was impossible to maintain these rules with the daily pair rotations of the experiment. With pairs rotating every day, team members were forced to work in unfamiliar areas of the codebase. In addition, there was far less risk for any team member working in an unfamiliar area since that member would only stay on a card for a day or two before passing it to someone else.

然而，在每日配對輪換的實驗中，這些原本的規則變得無法維持。由於每天都在更換配對，團隊成員被迫涉足自己不熟悉的代碼區域。此外，這種方式也大幅降低了風險，因為每位成員最多只會在同一張卡片上工作一到兩天，然後就會將其交接給其他人。

Our teams found that frequent pair rotations leveled the experience impact people have on cards. Longer-term team members could remove blockers from newer members and share knowledge that help accelerate their growth and learning curve of the codebase and development tools.

我們的團隊發現，高頻率配對輪換能夠均衡不同成員在各張卡片上的影響力。經驗豐富的成員可以幫助新成員解決瓶頸問題，並傳授知識，從而加速新成員對代碼庫與開發工具的學習曲線。

A few months after the experiment, one team gave us some interesting feedback: They found that when a problem came up in production, they didn't need to depend on just one person to look into and fix it. The team could assign anyone to troubleshoot the issue. In addition, another feedback mentioned an incoming pair rotation brought new context that changed implementation direction and helped resolve a problem in the early stages of the feature’s development, thus saving the team lots of time and rework. These highlight the benefits of having knowledge spread among the team.

在實驗結束後的幾個月內，一個團隊向我們提供了一個有趣的回饋：當生產環境出現問題時，他們不再需要依賴某一特定成員來排查與修復，而是可以指派任何一位成員來進行問題診斷與修復。此外，另一個團隊反映，在某次配對輪換時，新成員帶來了不同的背景資訊，促使團隊在功能開發的早期階段改變實作方向，從而節省了大量時間與返工成本。這些回饋突顯了知識在團隊內部廣泛流通的好處，使得團隊更加靈活與高效。

#### “The work is moving among the team members”

#### 「工作正在團隊成員之間流動」

Team members found that everyone developed context related to all the cards in progress, even before working on each card. This increased the effectiveness of the daily standup sessions: Team members would share insights, identify risks in advance and help each other in removing blockers. This is only possible when all developers have enough context and ownership of all cards in play. No single individual owns any piece of work, and everyone in the team is responsible for the progress of the tasks as a whole.

團隊成員發現，即使還未實際參與某張卡片的工作，他們仍能對所有進行中的卡片建立相關背景知識。這大幅提升了每日站會的效率：團隊成員能夠分享見解、提前識別風險，並幫助彼此消除障礙。這只有在所有開發人員對所有進行中的卡片都有足夠的背景理解和責任感時才有可能實現。在這樣的模式下，沒有任何一項工作屬於單一個人，而是整個團隊共同負責所有任務的進展。

## Conclusions

Even though the experiment involved daily pair rotations, the three participating teams did not opt for continuing at this frequency in the end. One team settled on 3 day rotations while the other two teams settled on 2 day rotations. We noticed that frequent rotations revealed bottlenecks and friction points in the development process of the teams. Opting for rotating every 3 days instead of everyday relates to working around these blockers.

儘管這次實驗嘗試了每日配對輪換，但最終三個參與團隊都沒有選擇維持這種頻率。一個團隊決定每 3 天輪換一次，而另外兩個團隊則選擇每 2 天輪換一次。我們觀察到，頻繁的輪換揭示了團隊開發流程中的瓶頸與摩擦點。選擇每 3 天輪換一次，而非每天輪換，與團隊試圖克服這些障礙有關。

It is common that on any day the team members have only a few hours, often fragmented throughout the day, to pair. Team members felt that they needed more than one day to achieve a meaningful pairing experience. In turn, this can also indicate high fragmentation of development time throughout the days. This was one of the reasons teams opted for less frequency than practiced in the experiment.

通常，每天團隊成員能用於配對編程的時間僅有幾個小時，且這些時間往往是零散的。成員們認為，他們需要超過一天的時間，才能獲得有意義的配對體驗。這也反映了團隊的開發時間高度碎片化，這是團隊選擇降低輪換頻率的其中一個原因。

Many of the perceived challenges during the experiment are not absolutes, but rather decrease when addressed head-on (and conversely increase if avoided). The experiment provided a daily opportunity for participants to reflect on pairing challenges and discuss alternatives to solve them as a team. The time and effort employed in the experiment ceremonies had a high return of investment.

在實驗期間，許多被認為是挑戰的問題，其實並非絕對無解，而是可以透過直接面對來減少影響（相反地，如果刻意避免，問題則可能會惡化）。這項實驗為團隊提供了每日機會，讓參與者反思配對過程中的挑戰，並討論解決方案。投入在實驗儀式中的時間與精力，帶來了極高的回報。

In general, running the experiment dramatically improved the frequency of pair rotations in these teams. One of the teams moved from rotating once a month to rotating every 3 days. This frequency increase was a result of the teams acknowledging the benefits of short-lived pairs such as better knowledge sharing and team building. During the experiments, team members also reported participating in the experiment made them learn more about pairing best practices. In addition, running pairing retrospectives and feedback exchange sessions promoted the feedback culture in the teams.

整體而言，這次實驗顯著提升了這些團隊的配對輪換頻率。例如，其中一個團隊從原本每月輪換一次，變成每 3 天輪換一次。這樣的頻率提升，源自團隊對於短期配對的優勢有了更深的認識，例如更好的知識共享與團隊協作。在實驗過程中，成員們也回饋說，參與這項實驗讓他們學到了更多關於配對編程的最佳實踐。此外，配對回顧與反饋交流的會議，進一步促進了團隊內部的反饋文化。