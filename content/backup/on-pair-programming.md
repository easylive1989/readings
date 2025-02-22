---
date: '2025-02-22T10:59:12+08:00'
title: 'On Pair Programming'
tags: ["Martin Fowler"]
---

[Source](https://martinfowler.com/articles/on-pair-programming.html)

> Betty Snyder and I, from the beginning, were a pair. And I believe that the best programs and designs are done by pairs, because you can criticise each other, and find each others errors, and use the best ideas.
> -- [Jean Bartik, one of the very first programmers](http://www.computerhistory.org/revolution/birth-of-the-computer/4/78/2258)
> 
> 從一開始，Betty Snyder 和我就是一對。我相信最好的程式和設計都是由兩人合作完成的，因為這樣可以互相批評、找出彼此的錯誤，並採用最好的點子。
> —— [Jean Bartik，最早的程式設計師之一](http://www.computerhistory.org/revolution/birth-of-the-computer/4/78/2258)

> Write all production programs with two people sitting at one machine.
> -- [Kent Beck](https://www.amazon.com/gp/product/0321278658/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0321278658&linkCode=as2&tag=martinfowlerc-20)
> 所有正式的生產程式都應該由兩個人坐在同一台機器前一起編寫。
> —— [Kent Beck](https://www.amazon.com/gp/product/0321278658/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0321278658&linkCode=as2&tag=martinfowlerc-20)

Jean Bartik was one of the [ENIAC women](https://en.wikipedia.org/wiki/ENIAC#Programmers), who are considered by many to be the very first programmers. They took on the task of programming when the word “program” was not even used yet, and there were no role models or books to tell them how to do this - and they decided that it would be a good idea to work in a pair. It took about 50 more years for pair programming to become a widespread term, when Kent Beck described the term in his book “Extreme Programming” in the late 1990s. The book introduced agile software development practices to a wider audience, pairing being one of them.

Jean Bartik 是[ENIAC 女性程式設計師](https://en.wikipedia.org/wiki/ENIAC#Programmers)之一，她們被許多人認為是最早的程式設計師。在「程式」這個詞尚未被使用、也沒有前人經驗或書籍可以參考的時候，她們便開始了這項工作，並決定以配對合作的方式來進行。大約 50 年後，「配對程式設計」（Pair Programming）這個概念才普及起來，當時 Kent Beck 在 1990 年代末的著作《極限編程》（Extreme Programming）中正式描述了這個術語。這本書向更廣泛的讀者介紹了敏捷軟體開發的實踐方式，而配對編程便是其中之一。

Pair programming essentially means that two people write code together on one machine. It is a very collaborative way of working and involves a lot of communication. While a pair of developers work on a task together, they do not only write code, they also plan and discuss their work. They clarify ideas on the way, discuss approaches and come to better solutions.

配對程式設計的核心概念是兩個人共用一台機器，一起撰寫程式碼。這是一種高度協作的工作方式，涉及大量的溝通。在配對編程的過程中，開發人員不僅僅是編寫程式碼，還會一起規劃與討論工作內容。他們會在過程中澄清想法、探討不同的做法，並最終找出更好的解決方案。

The first part of this article, [“How to pair”](https://martinfowler.com/articles/on-pair-programming.html#HowToPair), gives an overview of different practical approaches to pair programming. It's for readers who are looking to get started with pairing, or looking to get better at it.

本文的第一部分，[「如何進行配對程式設計」](https://martinfowler.com/articles/on-pair-programming.html#HowToPair)，概述了不同的配對程式設計方法，適合想要開始嘗試或提升配對編程技巧的讀者。

The second and third parts, [“Benefits”](https://martinfowler.com/articles/on-pair-programming.html#Benefits) and [“Challenges”](https://martinfowler.com/articles/on-pair-programming.html#Challenges), dive deeper into what the goals of pair programming are, and how to deal with the challenges that can keep us from those goals. These parts are for you if you want to understand better why pair programming is good for your software and your team, or if you want some ideas what to improve.

第二和第三部分，[「配對編程的優勢」](https://martinfowler.com/articles/on-pair-programming.html#Benefits)與[「挑戰」](https://martinfowler.com/articles/on-pair-programming.html#Challenges)，深入探討了配對程式設計的目標，以及如何克服可能阻礙我們達成目標的挑戰。如果你想更清楚地理解為何配對編程有助於提升軟體品質與團隊合作，或者想找到改進的方法，這兩部分將對你有所幫助。

Part four and five, [“To pair or not to pair?”](https://martinfowler.com/articles/on-pair-programming.html#ToPairOrNotToPair), and [“But really, why bother?”](https://martinfowler.com/articles/on-pair-programming.html#ButReallyWhyBother), will conclude with our thoughts on pairing in the grand scheme of team flow and collaboration.

第四與第五部分，[「該進行配對編程嗎？」](https://martinfowler.com/articles/on-pair-programming.html#ToPairOrNotToPair)和[「但真的值得嗎？」](https://martinfowler.com/articles/on-pair-programming.html#ButReallyWhyBother)，將以團隊協作與整體工作流程的角度，探討配對程式設計的價值並總結相關見解。

## How to pair

### Styles

#### Driver and Navigator

These classic pair programming role definitions can be applied in some way or other to many of the approaches to pairing.

這些經典的配對程式設計角色定義，可以在不同的配對方式中以各種形式應用。

The **Driver** is the person at the wheel, i.e. the keyboard. She is focussed on completing the tiny goal at hand, ignoring larger issues for the moment. A driver should always talk through what she is doing while doing it.

**駕駛者（Driver）** 是負責操作鍵盤的人，相當於掌控方向盤的角色。她的重點是完成當下的小目標，暫時不考慮更大的問題。駕駛者應該隨時口述自己正在做的事情，確保溝通順暢。

The **Navigator** is in the observer position, while the driver is typing. She reviews the code on-the-go, gives directions and shares thoughts. The navigator also has an eye on the larger issues, bugs, and makes notes of potential next steps or obstacles.

**導航者（Navigator）** 則處於觀察者的位置，當駕駛者輸入程式碼時，她負責即時審視、提供指導並分享想法。導航者同時關注更廣泛的問題，如潛在的錯誤，並記錄可能的下一步行動或潛在的障礙。

![](https://martinfowler.com/articles/on-pair-programming/driver_navigator.png)

The idea of this role division is to have two different perspectives on the code. The driver's thinking is supposed to be more tactical, thinking about the details, the lines of code at hand. The navigator can think more strategically in their observing role. They have the big picture in mind.

這種角色分工的理念，是希望對程式碼能有兩種不同的視角。駕駛者的思維應該是較為「戰術性」的，專注於當下的細節與程式碼本身；而導航者則負責「戰略性」思考，以旁觀者的角度掌握整體概況。

A common flow goes like this:

常見的流程如下：

- Start with a reasonably well-defined task
- Agree on one tiny goal at a time. This can be defined by a unit test, or by a commit message, or written on a sticky note.
- Switch keyboard and roles regularly. Shared active participation keeps the energy level up and we learn and understand things better.
- As navigator, avoid the “tactical” mode of thinking, leave the details of the coding to the driver - your job is to take a step back and complement your pair's more tactical mode with medium-term thinking. Park next steps, potential obstacles and ideas on sticky notes and discuss them after the tiny goal is done, so as not to interrupt the driver's flow.

- 先確定一個合理且明確的任務。
- 每次只專注於一個小目標，這個目標可以透過單元測試來定義，也可以用提交訊息（commit message）或便利貼寫下來。
- 定期交換鍵盤與角色，讓雙方都能積極參與，維持專注度，並加深學習與理解。
- 作為導航者時，避免進入「戰術性」思維模式，把程式碼的細節留給駕駛者處理。你的職責是從更高層次的角度思考，補充搭檔的戰術性思維。例如，可以將接下來的步驟、潛在的障礙或新想法記錄在便利貼上，等到當前的小目標完成後再討論，避免打斷駕駛者的思路。


#### Ping Pong

![](https://martinfowler.com/articles/on-pair-programming/ping_pong.png)

This technique embraces [Test-Driven Development](https://martinfowler.com/bliki/TestDrivenDevelopment.html) (TDD) and is perfect when you have a clearly defined task that can be implemented in a test-driven way.

這種技術融合了[Test-Driven Development（測試驅動開發，TDD）](https://martinfowler.com/bliki/TestDrivenDevelopment.html)，特別適用於那些可以以測試驅動方式實作的明確任務。

- “Ping”: Developer A writes a failing test
- “Pong”: Developer B writes the implementation to make it pass.
- Developer B then starts the next “Ping”, i.e. the next failing test.
- Each “Pong” can also be followed by refactoring the code together, before you move on to the next failing test. This way you follow the “Red - Green - Refactor” approach: Write a failing test (red), make it pass with the minimum necessary means (green), and then [refactor](https://martinfowler.com/tags/refactoring.html).

- **「Ping」**：開發者 A 撰寫一個失敗的測試。
- **「Pong」**：開發者 B 撰寫對應的實作，使測試通過。
- 接著，開發者 B 開始下一個 **「Ping」**，也就是撰寫下一個失敗的測試。
- 每次 **「Pong」** 之後，也可以先一起重構程式碼，然後再進入下一個失敗的測試。這樣就遵循了 **「紅 - 綠 - 重構（Red - Green - Refactor）」** 的流程：先撰寫一個失敗的測試（紅），再用最簡單的方法讓測試通過（綠），最後進行[重構](https://martinfowler.com/tags/refactoring.html)，提升程式碼品質。

> [!NOTE]
> For some great examples on the red-green-refactor workflow take a look into the [99 bottles of OOP](https://www.sandimetz.com/99bottles/sample/) book by Sandy Metz and Katrina Owen.
> 
> 如果想看到更多「紅 - 綠 - 重構（Red-Green-Refactor）」流程的精彩範例，可以參考 Sandy Metz 和 Katrina Owen 合著的《99 Bottles of OOP》一書。

#### Strong-Style Pairing

This is a technique particularly useful for knowledge transfer, described in much more detail by Llewellyn Falco [here.](https://llewellynfalco.blogspot.com/2014/06/llewellyns-strong-style-pairing.html)

這是一種特別適合知識傳遞的配對技術，Llewellyn Falco 在[這裡](https://llewellynfalco.blogspot.com/2014/06/llewellyns-strong-style-pairing.html)有更詳細的描述。

The rule: “For an idea to go from your head into the computer it MUST go through someone else's hands”. In this style, the navigator is usually the person much more experienced with the setup or task at hand, while the driver is a novice (with the language, the tool, the codebase, ...). The experienced person mostly stays in the navigator role and guides the novice.

規則：「要讓一個想法從你的大腦進入電腦，它必須透過別人的手來實作。」在這種配對方式中，導航者（Navigator）通常是對當前環境或任務較有經驗的人，而駕駛者（Driver）則是新手（可能是不熟悉該語言、工具或程式碼庫的人）。較有經驗的人大多擔任導航者的角色，負責指導新手完成任務。

An important aspect of this is the idea that the driver totally trusts the navigator and should be “comfortable with incomplete understanding”. Questions of “why”, and challenges to the solution should be discussed after the implementation session. In a setting where one person is a total novice, this can make the pairing much more effective.

這種方法的一個重要核心理念是，駕駛者應該完全信任導航者，並且「能夠接受不完全理解的狀態」。關於「為什麼這樣做？」或對解決方案的質疑，應該留到實作結束後再討論。在一個人完全是新手的情況下，這種方式可以大幅提升配對的效率。

While this technique borders on micro-management, it can be a useful onboarding tool to favor active “learning by doing” over passive “learning by watching”. This style is great for initial knowledge transfer, but shouldn't be overused. Keep in mind that the goal is to be able to easily switch roles after some time, and ease out of the micro management mode. That will be a sign that the knowledge transfer worked.

雖然這種技術接近於微觀管理（Micro-management），但它是一個很有用的入門工具，能讓新手透過「動手學習（learning by doing）」取代被動的「觀看學習（learning by watching）」。這種方式適合作為初期的知識傳遞手段，但不應該過度使用。請記住，最終的目標是能夠隨時輕鬆切換角色，並逐漸擺脫微觀管理模式。當你們能夠順利交換角色時，就代表知識傳遞已經成功。

#### Pair Development

“Pair Development” is not so much a specific technique to pair, but more of a mindset to have about pairing. (We first came across the term in [this thread](https://twitter.com/sarahmei/status/877738639991611392) on Sarah Mei's Twitter account.) The development of a [user story](https://martinfowler.com/bliki/UserStory.html) or a feature usually requires not just coding, but many other tasks. As a pair, you're responsible for all of those things.

「配對開發」與其說是一種特定的配對技術，不如說是一種對配對合作的心態。（我們最早在 [Sarah Mei 的 Twitter 討論串](https://twitter.com/sarahmei/status/877738639991611392) 中看到這個術語。）開發使用者故事（User Story）或某個功能，通常不只是寫程式碼，還包括許多其他工作。而在配對開發的模式下，兩人共同負責所有相關的事情，不只是程式碼本身。

To help get you into the mindset, the following are a few examples of the non-coding activities in a story life cycle that benefit from pairing.

為了幫助建立這種心態，以下是一些在使用者故事的生命週期中，適合進行配對的非程式相關活動。

##### Planning - what's our goal?

When you first start working on something together, don't jump immediately into the coding. This early stage of a feature's life cycle is a great opportunity to avoid waste. With four eyes on the problem this early on, catching misunderstandings or missing prerequisites can save you a lot of time later.

在開始編寫程式碼之前，不要急著直接跳進去。這個階段是避免浪費時間的好機會。在一開始就有四隻眼睛來審視問題，可以幫助發現誤解或缺少的前置條件，避免後續不必要的時間浪費。

- **Understand the problem:** Read through the story and play back to each other how you understand it. Clear up open questions or potential misunderstandings with the Product Owner. If you have a [Definition of Ready](https://www.agilealliance.org/glossary/definition-of-ready/) in your team, go through that again and make sure you have everything to get started.
- **Come up with a solution:** Brainstorm what potential solutions for the problem are. You can either do this together, or split up and then present your ideas to each other. This depends on how well-defined the solution already is, but also on your individual styles. Some people like some time to think by themselves, others like talking things through out loud while they are thinking. If one of you is less familiar with the domain or tech, take some time to share the necessary context with each other.
- **Plan your approach:** For the solution you chose, what are the steps you need to take to get there? Is there a specific order of tasks to keep in mind? How will you test this? Ideally, write these steps down, in a shared document or on sticky notes. That will help you keep track of your progress, or when you need to onboard somebody else to help work on the task. Writing this down also simply helps remember what needs to be done - in the moment, we too often underestimate how many things we will have forgotten even as quickly as the next day...

- **理解問題：**  
    仔細閱讀使用者故事，然後互相確認彼此的理解是否一致。如果有任何疑問或可能的誤解，應該與產品負責人（Product Owner）溝通澄清。如果你的團隊有「[準備就緒定義（Definition of Ready）](https://www.agilealliance.org/glossary/definition-of-ready/)」，請重新檢查，確保所有必要的資訊都已準備好，才開始工作。
- **找出解決方案：**  
    針對問題進行腦力激盪（brainstorming），討論可能的解決方案。你們可以一起思考，也可以先各自獨立思考，然後再向對方展示自己的想法。這取決於解決方案的明確程度，也取決於個人風格——有些人喜歡自己先思考一陣子，有些人則喜歡透過大聲說出來來整理想法。如果其中一人對該領域或技術較不熟悉，可以花點時間先分享必要的背景知識。
- **規劃執行方式：**  
    針對選定的解決方案，討論需要完成的步驟。是否有特定的執行順序需要注意？該如何進行測試？理想情況下，這些步驟應該記錄下來，無論是寫在共用文件裡，還是貼在便利貼上。這樣可以幫助你們追蹤進度，也能讓其他人更容易加入這個任務。此外，書面記錄還能幫助記憶——因為我們經常低估自己會忘記多少細節，甚至可能在**隔天**就已經忘得差不多了……

##### Research and explore

When implementing a feature that requires you to use a technology you are both unfamiliar with, you'll have to do some research and exploration first. This work does not fit into the clean-cut “driver-navigator” or “ping-pong” approaches. E.g., browsing search engine results together on the same screen is usually not very effective.

當你要實作一個功能，而該功能涉及你不熟悉的技術時，你需要先進行一些研究與探索。這類工作並不適合明確的「駕駛-領航」（Driver-Navigator）或「乒乓編程」（Ping-Pong）模式。例如，兩個人同時在同一個螢幕上瀏覽搜尋結果通常不是最有效的做法。

Here is one way to approach this in pair development mode:

以下是如何在**雙人開發模式**下進行這類工作的方式：

- Define a list of questions that you need to answer in order to come up with a suitable solution.
- Split up - either divide the questions among you, or try to find answers for the same questions separately. Search the internet or other resources within your organisation to answer a question, or read up on a concept that is new to both of you.
- Get back together after a previously agreed upon timebox and discuss and share what you have found.

- **定義問題清單**：列出你們需要解答的問題，以便找到合適的解決方案。
- **分工研究**：可以選擇將問題拆分，各自負責一部分，或是各自尋找相同問題的答案。你們可以透過搜尋網路資源或查閱組織內部的知識庫，來理解新的概念或找到解決方案。
- **重新聚合與分享**：在事先約定好的時間點回來討論彼此的發現與結論。

##### Documentation

Another thing to work on together beyond the code is documentation. Reflect together if there is any documentation necessary for what you've done. Again, depending on the case at hand and your individual preferences, you can either create the documentation together, or have one of you create it, then the other review and word-smith.

除了撰寫程式碼之外，文件也是值得一起處理的部分。可以一起討論是否有必要針對所做的工作撰寫文件。根據情境與個人偏好，你們可以選擇共同撰寫，或者讓其中一人先寫，另一人再進行審閱與修訂。

Documentation is a great example of a task where a pair can keep each other disciplined. It's often a task left for last, and when it's the last thing keeping us from the great feeling of putting our story into “Done”, then more often than not, we skip it, or “wing it”. Working in a pair keeps us honest about some of the valuable, but annoying things that we'll be very thankful for in the future.

文件撰寫是一個雙人合作能發揮極大作用的任務。這通常是最容易被忽略的部分，特別是在開發即將完成、只差這一步就能將任務標記為「完成」時，許多人會選擇略過或草率完成。然而，雙人合作可以讓我們在這些重要但不太吸引人的任務上保持紀律——雖然現在可能覺得麻煩，但未來一定會感激這份努力。

### Time management

![](https://martinfowler.com/articles/on-pair-programming/pomodoro.png)

In addition to the general styles for pairing, there are other little tools and techniques to make it easier.

除了前面介紹的配對風格之外，還有一些小工具和技巧可以讓配對更輕鬆。

The pomodoro technique is ones of those tools. It is a time management method that breaks work down into chunks of time - traditionally 25 minutes - that are separated by short breaks. The technique can be applied to almost all of the pairing approaches described and will keep you focused. Pairing can be an exhausting practice, so it is helpful to get a reminder to take breaks and to switch the keyboard regularly.

番茄工作法是其中一種工具。這是一種時間管理方法，將工作拆分成固定時長的時間塊——傳統上為 25 分鐘——並在其間安排短暫休息。這種技術幾乎可以應用於所有配對模式，並幫助你保持專注。配對工作可能會讓人感到疲憊，因此使用這種方法不僅能提醒你適時休息，還能確保你們定期交換鍵盤。

Here is an example of how using the pomodoro technique looks like in practice.

以下是一個實際應用番茄工作法的示例：

- Decide on what to work on next
- Set a timer for 25 minutes, e.g. with the help of the many pomodoro browser extensions - or even a real life tomato shaped kitchen timer...
- Do some work without interruptions
- Pause work when the timer rings - start with short breaks (5-10 minutes)
- After 3 or 4 of these “pomodoros”, take a longer break (15–30 minutes)
- Use the short breaks to _really_ take a break and tank energy, get some water or coffee, use the bathroom, get some fresh air. Avoid using these short breaks for other work, like writing emails.

- 確定接下來要做的工作
- 設定一個 25 分鐘的計時器，例如使用各種番茄工作法的瀏覽器擴展，或者乾脆用一個番茄造型的廚房計時器……
- 專心工作，不受干擾
- 當計時器響起時，暫停工作並休息 5-10 分鐘
- 每完成 3 到 4 個「番茄時間」後，進行較長的休息（15-30 分鐘）
- 利用短暫休息時間來真正放鬆，補充能量、喝水、喝咖啡、上廁所、呼吸新鮮空氣。避免在這段時間處理其他工作，如回覆電子郵件。

### Pair Rotations

[What if we rotate pair everyday?](https://martinfowler.com/articles/rotate-pairs-experiment.html)

Rotating pairs means that after working together for some time, one half of the pair leaves the story, while the other person onboards somebody new to continue. The person who stays is often called the “anchor” of a story.

配對輪換的概念是，在一起工作一段時間後，其中一位成員離開該故事（Story），而留下的成員則負責指導新夥伴，讓工作繼續進行。留下來的人通常被稱為該故事的「錨點（Anchor）」。

One category of reasons why to rotate is logistics. Your pairing partner could be sick or going on holiday. Or one of you is working remotely for a day, and the work requires physical presence on site, e.g. because there is a hardware setup involved.

其中一個進行配對輪換的原因是後勤考量。你的配對夥伴可能會因病請假或正在度假，或者有時候其中一人需要遠端工作，但這項工作卻需要在現場操作，例如涉及某些硬體設備的設定。在這種情況下，輪換可以確保進度不受影響，讓其他成員接手工作。

Another group of reasons why to rotate is to mix things up. Either the two of you have been working together for a while and are starting to show signs of “cabin fever” because you are spending too much time together. Or you're working on something very tedious and energy-draining - a rotation will give one of you a break, and a new person can bring in some fresh perspectives and energy.

另一個輪換的理由是為了打破單調。如果兩個人長時間搭檔，可能會開始出現「幽閉煩躁」的跡象，覺得彼此相處時間過長而影響工作效率。有時候，當你們正在處理一項特別繁瑣、耗費精力的任務時，輪換不僅能讓其中一人稍作喘息，也能引入新的夥伴，帶來不同的觀點和新的能量，避免思維僵化。

Finally, the most given reason for pair rotations is to avoid knowledge silos, increase collective code ownership, and get some more code review on-the-go. Pairing itself is already helping with those things, but rotations can further increase the average number of eyes on each line of code that goes to production.

然而，最常見的輪換理由則是為了防止知識孤島，提升團隊的集體程式碼所有權，並讓程式碼在開發過程中獲得更多即時的審查。雖然配對開發本身已經在促進這些目標，但輪換能進一步提高團隊對程式碼的熟悉度，確保進入正式環境的每一行程式碼都經過更多人檢視，從而提升品質和可維護性。

As to the ideal frequency of rotations, this is where opinions diverge. Some people believe that rotations every 2-3 days are crucial to ensure a sufficient knowledge spread and quality. Every rotation comes with some costs though. There's the time to onboard a new person, and the cost of a context switch for one of the two. If there is no constant anchor for continuity, the risk increases that tacit knowledge about the problem and solution space gets lost and triggers rework. For more junior developers it's sometimes more beneficial to stay on something for longer, so they have sufficient time to immerse themselves in a topic and give new knowledge time to settle.

至於輪換的最佳頻率，則存在不同的看法。有些人認為每 2-3 天 輪換一次是確保知識有效傳播與程式碼品質的關鍵。然而，輪換也帶來一定的成本，例如新成員需要時間熟悉上下文，而留下來的夥伴也需要適應新的搭檔，這些過程都可能影響開發效率。如果團隊內沒有穩定的「錨點」來維持連續性，還可能導致對問題和解決方案的隱性知識流失，最終增加返工的風險。對於較資淺的開發者來說，持續專注於同一項工作更長時間，反而能幫助他們更深入理解並吸收新的知識。


> [!NOTE] 
> The term “show and tell” is used in different ways on agile teams. What we are referring to here is a regularly scheduled developer huddle, a time of the week where developers get together and discuss tech debt, learnings, share a significant piece of new code with each other, etc.
> 
> 注意 「Show and Tell」在敏捷團隊中可能有不同的含義。在這裡，我們指的是定期舉辦的開發者聚會，開發者可以在這個時間分享技術債、學習心得，或展示重要的新程式碼等。

Think about the trade off between these costs and the benefits. For example, let's say you have high quality knowledge sharing already, with team “show and tells”, readable code and good documentation. In that case, maybe an insistence on frequent rotations only marginally improves your collective code ownership, while creating high amounts of friction and overhead.

因此，在決定是否輪換時，需要仔細權衡成本與收益。例如，假設你的團隊已經透過「Show and Tell」、清晰的程式碼風格和完善的文件記錄來建立良好的知識共享機制，那麼過於頻繁的輪換可能只會帶來額外的切換成本，而對於提升集體程式碼所有權的效果卻相當有限。在這種情況下，應該根據團隊的實際需求來決定輪換的頻率，而不是一味追求頻繁的變動。

### Plan the Day

Pairing requires a certain level of scheduling and calendar coordination. If you don't take time to acknowledge and accommodate this, it will come back to haunt you later in the day.

配對編程需要一定程度的行程安排與日曆協調。如果沒有事先做好規劃，這些問題很可能會在一天當中反覆影響你的工作效率。

Start the day with a calendar check - agree with your pairing partner on how many hours you are going to pair, and see if you need to plan around meetings or time needed to work on other things outside of the pairing task. If it turns out that one of you will have to work by themselves for a while, then make sure to prepare for things to continue without the other person, e.g. by not using that person's computer to code.

從檢查行事曆開始，與你的配對夥伴確認今天將配對工作的時數，並查看是否需要根據會議或其他個人工作來調整時間安排。如果發現某個時段其中一人需要獨立作業，那麼最好事先做好準備，確保工作可以順利進行，例如避免使用對方的電腦來編寫程式碼。

If you have meetings or other commitments during the day, make sure you have a reminder in place that you will notice, especially when working on your pairing partner's machine. If your team pairs by default, consider agreeing on regular “core coding hours” for everyone. This makes scheduling much easier.

如果當天有會議或其他承諾，請確保有適當的提醒機制，以免錯過，尤其是在使用配對夥伴的電腦時更容易忘記時間。如果你的團隊預設採用配對編程的方式，那麼可以考慮設定固定的「核心編碼時段」，讓所有人都能同步進行開發工作，這樣會讓時間安排變得更簡單。

### Physical Setup

Pair programming means you need to work very closely together in the physical space of one shared desk. This is quite different from having your own table to spread out on. Being that close to one another requires a certain level of respect and attention for each other's needs. That is why it is worth spending some time figuring out a comfortable setup for both of you.

配對編程意味著兩人需要在同一張桌子上緊密合作，這與個人獨立工作的情境完全不同。由於彼此距離較近，尊重對方的需求並營造一個舒適的工作環境就變得格外重要。因此，花點時間調整一個讓雙方都感到自在的環境是很值得的。

- Make sure both of you have enough space, clear up the desk if necessary.
- Is there enough space for both chairs in front of the desk? Get waste bins and backpacks out of the way.
- Do you want to use two keyboards or one? Same for the mouse, one or two? There's no clear rule that always works, we recommend you try out what works best for each situation. Some of the factors that play into this are hygiene, how good you are at sharing keyboard time, or how much space you have available.
- Do you have an external monitor available, or maybe even two? If not, you can also consider setting up some kind of screen sharing, as if you were remote pairing. In that setup, each of you would use their own laptop keyboards.
- Check with your partner if they have any particular preferences or needs (e.g. larger font size, higher contrast, ...)
- If you have an unusual keyboard/IDE setup check with your partner if they are okay with it. See if you can have a simple mechanism to switch your settings back to a more standard configuration for these situations.

It is beneficial if your team can agree on a default setup, so that you don't have to discuss these things again and again.

- 確保你們兩人都有足夠的空間，如果需要的話，先清理桌面。
- 桌前是否有足夠的空間容納兩張椅子？請將垃圾桶和背包移開，以騰出更多空間。
- 你們想要使用兩個鍵盤還是一個？滑鼠也是同樣的考量，一個或兩個？沒有固定的最佳解法，建議根據情境測試最適合的方法。影響選擇的因素包括衛生考量、對鍵盤時間的共享習慣，以及可用的空間大小。
- 是否有可用的外接螢幕，甚至是兩個？如果沒有，也可以考慮設定螢幕共享，就像遠端配對一樣，這樣雙方都可以使用自己的筆電鍵盤進行操作。
- 與你的配對夥伴確認是否有任何特定的偏好或需求（例如較大的字體大小、更高的對比度等）。
- 如果你的鍵盤或 IDE 設定較為特殊，請先與對方確認是否可以接受，並考慮準備一種簡單的方法來快速切換回較為標準的配置，以確保雙方都能順利操作。

如果你的團隊能夠事先就「預設的工作環境」達成共識，將能避免每次開始配對時重複討論與調整，提高整體工作效率。


### Remote Pairing

Are you part of a distributed team, or some team members occasionally work from home? You can still practice pair programming, as long as both of you have reasonably stable internet access.

你的團隊是分散式的嗎？或者有些成員偶爾會在家工作？只要雙方的網路連線穩定，你仍然可以進行結對程式設計（Pair Programming）。

#### The Setup

![](https://martinfowler.com/articles/on-pair-programming/remote_pairing.png)

For remote pairing, you need a screen-sharing solution that allows you to not only see, but also control the other person's machine, so that you are able to switch the keyboard. Many video conferencing tools today already support this, so if you're working at a company who has a license for a commercial VC tool, try that first. There are also open source tools for video calls with remote control, e.g. [jitsi](https://jitsi.org/). For solutions that work at lower bandwidths, try things like [ssh with tmux](https://www.hamvocke.com/blog/remote-pair-programming-with-tmux/) or the [Live Share extension for Visual Studio Code](https://visualstudio.microsoft.com/services/live-share/).

遠端配對時，你需要一個螢幕共享解決方案，不僅能夠看到對方的畫面，還能夠控制對方的電腦，以便切換鍵盤操作。許多現代視訊會議工具已經支援這項功能，因此，如果你的公司有授權商業版的視訊會議工具，建議優先嘗試。此外，也有一些開源工具支援遠端控制，例如 [jitsi](https://jitsi.org/)。如果你的網路頻寬較低，可以考慮使用 [ssh 搭配 tmux](https://www.hamvocke.com/blog/remote-pair-programming-with-tmux/) 或 [Visual Studio Code 的 Live Share 擴充功能](https://visualstudio.microsoft.com/services/live-share/)。

#### Tips

- **Use video:** Since people communicate a lot through gestures and facial expressions it is nice to see the shared screen and your pairing partner's video at the same time. Some video conference solutions come with this feature; if yours doesn't, consider opening up an additional call in order to see each other.
- **Planning and designing:** Use collaborative online visualization tools, to reproduce the experience of sketching out things on paper or a whiteboard.
- **Audio experience:** Look for a quiet area and use a good headset, maybe even with a directional microphone. If you can't get away from the noise, “push to speak” functionality can also help. To avoid distractions on your side, noise-cancelling headphones are your friend.
- **使用視訊（Use video）：** 由於人們在交流時經常透過手勢和表情來輔助溝通，因此，能夠同時看到共享的螢幕和配對夥伴的視訊是很有幫助的。如果你的視訊會議工具不支援這項功能，可以考慮開啟額外的通話視窗，以便能夠互相看到。
- **規劃與設計（Planning and designing）：** 使用線上協作的可視化工具，來重現紙上繪圖或白板討論的體驗。 
- **音訊品質（Audio experience）：** 找一個安靜的地方，使用良好的耳機，最好是帶有指向性麥克風的款式。如果無法避免背景噪音，可以使用「按鍵發話」（Push to speak）功能來減少干擾。此外，使用降噪耳機可以幫助你更專注於配對工作。
> [!NOTE] More on remote pairing
> Chelsea Troy has put together a [blog post series about advanced pair programming](https://chelseatroy.com/2017/04/01/advanced-pair-programming-pairing-remotely/), including a post on remote pairing.
> 
> Chelsea Troy 撰寫了一系列關於[進階結對程式設計的部落格文章](https://chelseatroy.com/2017/04/01/advanced-pair-programming-pairing-remotely/)，其中包括遠端配對的專門討論。
- Dealing with network lag: It can be exhausting to work on a remote computer for a longer period of time when there is a network lag. So make sure to switch computers regularly, so that each of you has a chance to work on their own machine without lag. A network lag can also be annoying when you scroll through files because it can be hard to follow. It helps to avoid scrolling in long files, try to use keyboard shortcuts to open different parts of the file or use the collapse/uncollapse functionality instead.
- **應對網路延遲（Dealing with network lag）：** 當網路延遲較大時，在遠端電腦上工作會變得很累人，因此應該定期切換控制權，讓彼此都有機會在自己的機器上操作，減少受延遲影響的時間。此外，當在遠端環境中快速滾動程式碼時，延遲可能會讓另一方難以跟上進度，因此，盡量避免長時間滾動，建議使用鍵盤快捷鍵來跳轉文件不同部分，或者利用「折疊/展開」功能來瀏覽程式碼。

#### The Human Part

If you work in a setup where not the whole team is distributed and just one or a few of you are remote, try to include the remote partner in all discussions that are happening in the office. We tend to forget how much we share incidentally just by sitting in the same room.

如果你的團隊並非完全分散式，而只是少數成員遠端工作，請盡量讓遠端夥伴參與所有辦公室內的討論。我們經常會在同一個空間內無意間分享許多資訊，而遠端工作者很容易錯過這些內容。

Working remotely with someone you haven't met and do not know creates an additional challenge. On the one hand, pairing is a chance to get closer to each other on a remote team. On the other hand, it's sometimes easy to forget that part of the collaboration. If there is no chance that you meet in person, think about other ways to get to know each other a bit better, e.g. try to have a remote coffee together.

如果你是與一位素未謀面的遠端夥伴合作，這會帶來額外的挑戰。從某個角度來看，配對程式設計可以幫助遠端團隊成員拉近距離，但有時我們可能會忘記這層人際互動的重要性。如果沒有機會見面，請考慮其他方式來增進彼此的認識，例如安排一次線上咖啡聊天。

Finally, while remote pairing can have its challenges, it can also make it easier to focus than when pairing on site, because it is easier to blend out distractions with headphones on.

最後，儘管遠端配對可能會面臨一些挑戰，但有時它比現場配對更容易讓人集中注意力，因為配戴耳機時，可以有效降低外部干擾。

### Have a Donut Together

Celebrate when you have accomplished a task together! High-fiving each other might seem corny, but it's actually a little “power pose” you can do together that can energize and get you ready for the next task. Or maybe you create your own way of celebrating success, like Lara Hogan, who celebrates career achievements with a [donut](https://larahogan.me/donuts/).

當你們一起完成一項任務時，慶祝一下吧！ 互相擊掌可能聽起來很俗氣，但這實際上是一種可以一起做的「能量姿勢」，它可以激勵你們，讓你們為下一個任務做好準備。 或者，也許你可以創造自己慶祝成功的方式，例如 Lara Hogan，她會用 [甜甜圈](https://www.google.com/url?sa=E&q=https%3A%2F%2Flarahogan.me%2Fdonuts%2F) 來慶祝職業成就。

### Things to Avoid

The different approaches and techniques help you to have a better pairing experience. Here are a few common pitfalls to avoid:

不同的方法和技巧可以幫助你獲得更好的協同工作體驗。 以下是一些常見的陷阱，應避免：

#### Drifting apart 漸行漸遠

When you pair, avoid to read emails or to use your phone. These distractions might come across as direspectful to your pair, and they distract you from the task you are working on. If you really need to check something, make it transparent what you are doing, and why. Make sure that everyone has enough time to read their emails by taking enough breaks and reserving some individual time each day.

當你協同工作時，避免閱讀電子郵件或使用手機。 這些干擾可能會被視為對你的夥伴不尊重，並且會分散你對正在進行任務的注意力。 如果你真的需要檢查某些東西，請坦誠說明你正在做什麼，以及為什麼。 確保每個人都有足夠的時間閱讀他們的電子郵件，方法是充分休息並每天預留一些個人時間。

#### Micro-Management Mode 微管理模式

Watch out for micro-management mode: It doesn't leave room for the other person to think and is a frustrating experience, if someone keeps giving you instructions like:

注意微管理模式： 它不會給另一個人思考的空間，並且如果有人一直給你指令，這將是一種令人沮喪的體驗，例如：

- “Now type 'System, dot, print, “...
- “Now we need to create a new class called...”
- “Press command shift O...”

- “現在輸入 'System, dot, print, “...
- “現在我們需要創建一個名為...的新類別”
- “按下 Command Shift O...”

#### Impatience 沒耐心

Apply the “5 seconds rule”: When the navigator sees the driver do something “wrong” and wants to comment, wait at least 5 seconds before you say something - the driver might already have it in mind, then you are needlessly interrupting their flow.

運用「五秒原則」： 當領航員看到駕駛員做錯事並且想要評論時，至少等待 5 秒鐘再說 - 駕駛員可能已經想到了，那麼你就是在不必要地打斷他們的思路。

As Navigator, don't immediately point out any error or upcoming obstacle: Wait a bit for the driver to correct or write a sticky note to remember later. If you intervene immediately, this can be disruptive to the driver's thinking process.

作為領航員，不要立即指出任何錯誤或即將到來的障礙： 等待駕駛員糾正或寫下便利貼以供稍後記住。 如果你立即介入，這可能會干擾駕駛員的思考過程。

#### Keyboard Hogging

![](https://martinfowler.com/articles/on-pair-programming/sharing_is_caring.png)

Watch out if you're “hogging the keyboard”: Are you controlling it all the time, not letting your pairing partner do some typing as well?

注意你是否“獨佔鍵盤”： 你是否一直控制著它，而不讓你的協同工作夥伴也打字？

This can be a really annoying experience for your pair and might cause them having a hard time focussing because of limited “active participation”. Try one of the approaches described earlier to make sure that you switch the keyboard frequently.

對於你的夥伴來說，這可能是一種非常惱人的體驗，並且由於有限的“積極參與”而可能導致他們難以集中注意力。 嘗試之前描述的方法之一，以確保你經常更換鍵盤。

#### Pairing 8 Hours per Day

Teams that are really committed to making pair programming work sometimes end up pairing for 8 hours a day. In our experience, that is not sustainable. First of all it is just too exhausting. And secondly, it does not even work in practice because there are so many other things you do other than coding, e.g. checking emails, having 1:1s, going to meetings, researching/learning. So keep that in mind when planning your day and don't assume it will be 100% coding together.

真正致力於讓協同工作發揮作用的團隊，有時最終會每天協同工作 8 小時。 根據我們的經驗，這是不可持續的。 首先，這太累了。 其次，它甚至在實踐中都行不通，因為除了編碼之外，你還會做很多其他事情，例如檢查電子郵件、進行一對一會議、參加會議、研究/學習。 因此，在計劃你的一天時請記住這一點，不要假設它將 100% 一起編碼。

### There is not “THE” right way

There are many approaches to pair programming and there is not “THE” right way to do it. It depends on your styles, personalities, experience, the situation, the task and many other factors. In the end, the most important question is: Do you get the promised benefits out of it? If this is not the case, try out something else, reflect, discuss and adjust to get them.

協同工作有很多種方法，並且沒有“唯一”正確的方法來做。 這取決於你的風格、個性、經驗、情況、任務和許多其他因素。 最後，最重要的問題是： 你是否從中獲得了所承諾的好處？ 如果不是這種情況，請嘗試其他方法，反思、討論和調整以獲得這些好處。

## Benefits

What is pair programming good for? Awareness of all its potential benefits is important to decide when you do it, how to do it well, and to motivate yourself to do it in challenging times. The main goals pairing can support you with are software quality and team flow.

協同編程有什麼好處？ 了解其所有潛在好處，對於決定何時進行協同編程、如何做好以及在充滿挑戰的時期激勵自己至關重要。 協同編程可以幫助你實現的主要目標是軟體品質和團隊流動性。

![](https://martinfowler.com/articles/on-pair-programming/benefits_overview.jpg)

How can pairing help you achieve those goals then?

那麼，協同編程如何幫助你實現這些目標呢？

### Knowledge Sharing

Let's start with the most obvious and least disputed benefit of pairing: Knowledge sharing. Having two people work on a piece of the code helps the team spread knowledge on technology and domain on a daily basis and prevents silos of knowledge. Moreover, when two minds understand and discuss a problem, we improve the chances of finding a good solution. Different experiences and perspectives will lead to the consideration of more alternatives.

讓我們從協同編程最明顯和最不具爭議的好處開始：知識共享。 讓兩個人一起處理一段程式碼有助於團隊每天傳播關於技術和領域的知識，並防止知識孤島。 此外，當兩個人理解並討論一個問題時，我們更有可能找到一個好的解決方案。 不同的經驗和觀點將導致考慮更多選擇。

#### Practical Tips

Don't shy away from pairing on tasks when you have no idea about the technology involved, or the domain. If you keep working in the area that you feel most comfortable in, you will miss out on learning new things, and ultimately spreading knowledge in your team.

當你對所涉及的技術或領域一無所知時，不要迴避協同處理任務。 如果你一直在你覺得最舒適的領域工作，你將錯過學習新事物的機會，並最終錯過在你的團隊中傳播知識的機會。

If you notice that a team member tends to work on the same topics all the time, ask them to mix it up to spread their expertise. It can also help to create a skill matrix with the team's tech & business topics and each person's strengths and gaps in each area. If you put that on a wall in your team area, you can work together on getting a good spread of knowledge.

如果你發現某個團隊成員傾向於一直處理相同的議題，請要求他們混合處理，以傳播他們的專業知識。 創建一個技能矩陣也會有所幫助，其中包含團隊的技術和業務主題，以及每個人在每個領域的優勢和差距。 如果你將其貼在團隊區域的牆上，你可以一起努力獲得良好的知識傳播。

### Reflection

Pair programming forces us to discuss approaches and solutions, instead of only thinking them through in our own head. Saying and explaining things out loud pushes us to reflect if we really have the right understanding, or if we really have a good solution. This not only applies to the code and the technical design, but also to the user story and to the value a story brings.

協同編程迫使我們討論方法和解決方案，而不僅僅是在我們自己的頭腦中思考它們。 大聲說出並解釋事情會促使我們反思我們是否真的有正確的理解，或者我們是否真的有一個好的解決方案。 這不僅適用於程式碼和技術設計，也適用於使用者故事以及故事帶來的價值。

#### Practical Tips

It requires trust between the two of you to create an atmosphere in which both of you feel free to ask questions and also speak openly about things you don't understand. That's why building relationships within the team becomes even more important when you pair. Take time for regular 1:1 and feedback sessions.

你們兩個人之間需要信任，才能營造一種氣氛，讓你們都能自由地提出問題，也能坦率地談論你不了解的事情。 這就是為什麼在協同工作時，建立團隊內部的關係變得更加重要。 花時間進行定期的 1:1 和回饋會議。

### Keeping focus

It's a lot easier to have a structured approach when there are two of you: Each of you has to explicitly communicate why you are doing something and where you are heading. When working solo, you can get distracted a lot easier, e.g. by “just quickly” trying a different approach without thinking it through, and then coming back out of the rabbit hole hours later. Your pairing partner can prevent you from going down those rabbit holes and focus on what is important to finish your task or story. You can help each other stay on track.

當有你們兩個人時，更容易採用結構化的方法： 你們每個人都必須明確地溝通你為什麼要做某事以及你要去哪裡。 當單獨工作時，你更容易分心，例如 “只是快速地” 嘗試一種不同的方法而不經過思考，然後在幾個小時後從兔子洞裡出來。 你的協同夥伴可以防止你掉進那些兔子洞，並專注於完成你的任務或故事的重要事項。 你可以互相幫助，保持正軌。

#### Practical Tips

Make plans together! Discuss your task at hand and think about which steps you need to make to reach your goal. Put each of the steps on sticky notes (or if remotely, subtasks in your ticket management system), bring them in order and go one by one. Try this in combination with the [Pomodoro technique](https://martinfowler.com/articles/on-pair-programming.html#TimeManagement) and try to finish one of the goals in one pomodoro.

一起制定計畫！ 討論你手頭的任務，並思考你需要採取哪些步驟才能達到你的目標。 將每個步驟放在便利貼上（或者如果遠端，則在你的工單管理系統中建立子任務），按順序排列，然後逐一完成。 嘗試將此方法與 番茄工作法 結合使用，並嘗試在一個番茄鐘內完成其中一個目標。

Never forget that communication is key. Talk about what you are doing and demand explanations from each other.

永遠不要忘記溝通是關鍵。 談論你正在做的事情，並要求彼此解釋。

### Code review on-the-go 即時程式碼審查

When we pair, we have 4 eyes on the little and the bigger things as we go, more errors will get caught on the way instead of after we're finished.

當我們協同工作時，我們隨時關注大大小小的事情，更多的錯誤會在過程中被發現，而不是在我們完成後。

The [refactoring](https://martinfowler.com/tags/refactoring.html) of code is always part of coding, and therefore of pair programming. It's easier to improve code when you have someone beside you because you can discuss approaches or the naming of things for example.

程式碼的 [重構](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Ftags%2Frefactoring.html) 始終是編碼的一部分，因此也是協同編程的一部分。 當你身邊有人可以與你討論方法或事物命名等問題時，改善程式碼會更容易。

Doing code reviews after the fact has some downsides. We will dive more into this later, in [“To pair or not to pair?”](https://martinfowler.com/articles/on-pair-programming.html#ToPairOrNotToPair).

事後進行程式碼審查有一些缺點。 我們稍後將在 [“要不要協同工作？”](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Farticles%2Fon-pair-programming.html%23ToPairOrNotToPair) 中更深入地探討這個問題。

#### Practical Tips

Ask each other questions! Questions are the most powerful tool to understand what you are doing and to come to better solutions. If code is not easy to read and understand for one of you, try a different way that is easier to understand.

互相提問！ 問題是了解你正在做什麼以及找到更好解決方案的最有力工具。 如果程式碼對你們其中一人來說不容易閱讀和理解，請嘗試一種更容易理解的方式。

If you feel the need to have more code review on pair programmed code, reflect if you can improve your pairing. Weren't you able to raise all questions and concerns during your pairing session? Why is that? What do you need to change?

如果你覺得需要對協同編程的程式碼進行更多程式碼審查，請反思是否可以改進你的協同工作。 你是否無法在協同工作期間提出所有問題和疑慮？ 為什麼？ 你需要改變什麼？

### Two modes of thinking combined

As mentioned when we described the classic [driver/navigator](https://martinfowler.com/articles/on-pair-programming.html#DriverAndNavigator) style earlier, pairing allows you to have different perspectives on the code. The driver's brain is usually more in “tactical” mode, thinking about the details, the current line of code. Meanwhile, the navigator's brain can think more strategically, consider the big picture, park next steps and ideas on sticky notes. Could one person combine these two modes of thinking? Probably not! Having a tactical and strategic view combined will increase your code quality because it will allow you to pay attention to the details while also having the bigger picture in mind.

正如我們在前面描述經典的 [駕駛員/領航員](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Farticles%2Fon-pair-programming.html%23DriverAndNavigator) 風格時提到的，協同工作讓你能夠對程式碼有不同的視角。 駕駛員的大腦通常更多處於“戰術”模式，思考細節、當前的程式碼行。 同時，領航員的大腦可以更具策略性地思考，考慮大局，並將下一步驟和想法記錄在便利貼上。 一個人可以結合這兩種思考模式嗎？ 可能不行！ 將戰術和戰略觀點結合起來將提高你的程式碼品質，因為它讓你能夠在關注細節的同時，也將大局銘記在心。

#### Practical Tips

Remember to switch the keyboard and thus the roles regularly. This helps you to refresh, to not get bored, and to practice both ways of thinking.

記得定期切換鍵盤，從而切換角色。 這有助於你提神、不感到厭煩，並練習兩種思考方式。

As navigator, avoid the “tactical” mode of thinking, leave the details of the coding to the driver - your job is to take a step back and complement your pair's more tactical mode with medium-term thinking.

作為領航員，避免“戰術”思考模式，將編碼的細節留給駕駛員 - 你的工作是退一步，並用中期思考來補充你的協同夥伴更具戰術性的模式。

### Collective Code Ownership 集體程式碼所有權

> Collective code ownership abandons any notion of individual ownership of modules. The code base is owned by the entire team and anyone may make changes anywhere.
> -- [Martin Fowler](https://www.martinfowler.com/bliki/CodeOwnership.html)
> 
> 集體程式碼所有權放棄了任何關於個人擁有模組的概念。 程式碼庫由整個團隊擁有，任何人都可以隨時隨地進行更改。

Consistent pairing makes sure that every line of code was touched or seen by at least 2 people. This increases the chances that anyone on the team feels comfortable changing the code almost anywhere. It also makes the codebase more consistent than it would be with single coders only.

持續的協同工作確保每一行程式碼都至少被 2 個人接觸或看到。 這增加了團隊中的任何人都可以在幾乎任何地方輕鬆更改程式碼的可能性。 它也使程式碼庫比只有單個程式設計師的情況更加一致。

#### Practical Tips

Pair programming alone does not guarantee you achieve collective code ownership. You need to make sure that you also rotate people through different pairs and areas of the code, to prevent knowledge silos.

單獨的協同編程並不能保證你實現集體程式碼所有權。 你需要確保你還讓人員輪流與不同的合作夥伴和程式碼區域合作，以防止知識孤島。

### Keeps the team's WIP low

Limiting work in progress is one of the core principles of Kanban to improve team flow. Having a [Work in Progress (WIP) limit](https://dzone.com/articles/the-importance-of-wip-limits) helps your team focus on the most important tasks. Overall team productivity often increases if the team has a WIP limit in place, because multi-tasking is not just inefficient on an individual, but also on the team level. Especially in larger teams, pair programming limits the number of things a team can work on in parallel, and therefore increases the overall focus. This will ensure that work constantly flows, and that blockers are addressed immediately.

限制在製品是看板提高團隊流動性的核心原則之一。 擁有 [在製品 (WIP) 限制](https://www.google.com/url?sa=E&q=https%3A%2F%2Fdzone.com%2Farticles%2Fthe-importance-of-wip-limits) 有助於你的團隊專注於最重要的任務。 如果團隊制定了 WIP 限制，整體團隊生產力通常會提高，因為多工處理不僅對個人效率低下，而且對團隊效率低下。 特別是在較大的團隊中，協同編程限制了團隊可以並行處理的事項數量，因此提高了整體專注力。 這將確保工作不斷流動，並且可以立即解決阻礙。

#### Practical Tips

Limit your team's WIP to the number of developer pairs on your team and make it visible in your team space (or, if you work remotely, in your online project management tool). Have an eye on the limit before picking up new tasks. WIP limit discipline might naturally force you into a pairing habit.

將你團隊的 WIP 限制在你團隊中開發人員協同配對的數量，並在你的團隊空間中（或者，如果你遠端工作，則在你的線上專案管理工具中）使其可見。 在接手新任務之前，請注意限制。 WIP 限制紀律可能會自然地迫使你養成協同工作的習慣。

### Fast onboarding of new team members

Since pairing facilitates knowledge sharing it can help with the onboarding of new team members. New joiners can get to know the project, the business and the organisation with the help of their pair. Changes in a team have an impact on the team flow. People just need some time to get to know each other. Pair programming can help to minimize that impact, because it forces people to communicate a lot more than they need when working solo.

由於協同工作有助於知識共享，它可以幫助新團隊成員上手。 在協同夥伴的幫助下，新加入者可以了解專案、業務和組織。 團隊的變更會對團隊流動性產生影響。 人們只需要一些時間才能互相了解。 協同編程可以幫助最大限度地減少這種影響，因為它迫使人們比單獨工作時需要更多地溝通。

#### Practical Tips

It is not enough to just put new joiners into a pair, and then they are “magically” included and onboarded. Make sure to provide the big picture and broader context before their first pairing session, and reserve some extra time for the onboarding. This will make it easier for them to follow along and contribute during the pairing, and get the most out of it. Always use the new joiners' machine when pairing, to make sure that they are set up to work by themselves as well.

僅僅將新加入者放入一對中是不夠的，然後他們就會“神奇地”被包含並上手。 確保在他們的第一次協同工作會議之前提供大局和更廣泛的背景，並為上手預留一些額外時間。 這將使他們更容易在協同工作期間跟進並做出貢獻，並從中獲得最大收益。 始終使用新加入者的機器進行協同工作，以確保他們也設定為可以自己工作。

Have an onboarding plan with a list of topics to cover. For some topics you might want to schedule dedicated sessions, for other topics the new team member can take the onboarding plan with her from pair to pair. If something is covered in the pairing session, you can check it off the list. This way the onboarding progress is visible to everyone on the team.

制定一個包含要涵蓋的主題清單的上手計畫。 對於某些主題，你可能需要安排專門的會議，對於其他主題，新團隊成員可以從一對一對地攜帶上手計畫。 如果在協同工作會議中涵蓋了某些內容，你可以從清單中勾選它。 這樣，團隊中的每個人都可以看到上手進度。

## Challenges

While pair programming has a lot of benefits, it also requires practice and might not work smoothly from the start. The following is a list of some of the common challenges teams experience, and some suggestions on how to cope with them. When you come across these challenges, keep the benefits in mind and remember why you pair. It is important to know what you want to achieve with a practice, so that you can adjust the way you do it.

雖然協同編程有很多好處，但也需要練習，並且一開始可能無法順利進行。 以下列出團隊遇到的一些常見挑戰，以及應對它們的一些建議。 當你遇到這些挑戰時，請記住這些好處，並記住你為什麼要協同工作。 重要的是要知道你想透過實踐達成什麼，這樣你才能調整你的做法。

### Pairing can be exhausting

When working alone, you can take breaks whenever you want, and your mind can drift off or shut down for a bit when it needs to. Pairing forces you to keep focus for potentially longer stretches of time, and find common ground with the other person's rhythm and ways of thinking. The increased focus is one of the benefits of pairing, but can also make it quite intense and exhausting.

當你獨自工作時，你可以隨時休息，並且你的思緒可以在需要時漂移或暫時關閉。 協同編程迫使你在可能更長的時間內保持專注，並找到與另一個人節奏和思考方式的共同點。 增加的專注力是協同編程的好處之一，但也可能使其相當緊張和令人筋疲力盡。

![](https://martinfowler.com/articles/on-pair-programming/take_breaks.png)
#### Ways to tackle

Taking enough breaks is key to face this challenge. If you notice you are forgetting to take regular breaks, try scheduling them with an alarm clock, for example 10 minutes per hour. Or use a time management technique like [Pomodoro](https://martinfowler.com/articles/on-pair-programming.html#TimeManagement). Don't skip your lunch break: Get away from the monitor and take a real break. Pairing or not, taking breaks is important and increases productivity.

充分休息是應對此挑戰的關鍵。 如果你注意到你忘記定期休息，請嘗試使用鬧鐘安排休息時間，例如每小時 10 分鐘。 或者使用諸如 [番茄工作法](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Farticles%2Fon-pair-programming.html%23TimeManagement) 之類的時間管理技巧。 不要跳過午餐休息時間： 離開螢幕，好好休息一下。 無論是否協同工作，休息都很重要，並且可以提高生產力。

Another important thing to prevent exhaustion is to not pair 8 hours per day. Limit it to a maximum of 6 hours per day. Regularly switching roles from driver to navigator can also help to keep the energy level up.

防止疲勞的另一個重要方法是每天不要協同工作 8 小時。 將其限制為每天最多 6 小時。 定期從駕駛員切換到領航員也可以幫助保持能量水平。

### Intense collaboration can be hard

Working so closely with another person for long stretches of time is intense. You need to communicate constantly and it requires empathy and interpersonal skills.

長時間與另一個人如此密切地合作是緊張的。 你需要不斷溝通，並且需要同理心和人際交往能力。

You might have differences in techniques, knowledge, skills, extraversion, personalities, or approaches to problem-solving. Some combinations of those might not match well and give you a rocky start. In that case, you need to invest some time to improve collaboration, and make it a mutual learning experience instead of a struggle.

你們可能在技術、知識、技能、外向性、個性和解決問題的方法上存在差異。 這些組合中的一些可能不太匹配，並給你一個不順利的開始。 在這種情況下，你需要投入一些時間來改善協作，並使其成為相互學習的體驗，而不是一場鬥爭。

#### Ways to tackle

A conversation at the beginning of your pairing session can help you to understand differences between your styles, and plan to adapt to that. Start your first session with questions like “How do we want to work together?”, “How do you prefer to pair?”. Be aware of how you like to work and how you are efficient, but also don't be closed off to other approaches - maybe you'll discover something new.

在你的協同工作會議開始時進行對話可以幫助你了解你的風格之間的差異，並計劃適應它。 從“我們想要如何一起工作？”、“你更喜歡如何協同工作？”等問題開始你的第一次會議。 注意你喜歡如何工作以及你的效率如何，但也不要對其他方法封閉 - 也許你會發現一些新東西。

At the end of a day of pairing, do a round of feedback for each other. If the idea of giving feedback seems daunting to you, think about it more as a mini retrospective. Reflect on how you both felt during the pairing session. Were you alert? Were you tired? What made you feel comfortable, what not? Did you switch the keyboard often enough? Did you achieve your goals? Is there anything you would like to try next time? It's good to make this a routine early on, so you have practice in giving feedback when something goes wrong.

在協同工作一天結束時，為彼此做一輪回饋。 如果給予回饋的想法對你來說似乎令人畏懼，請將其更多地視為一次小型回顧。 反思一下你們在協同工作期間的感受。 你們警覺嗎？ 你們累了嗎？ 什麼讓你感到舒服，什麼沒有？ 你們是否經常切換鍵盤？ 你們實現了目標嗎？ 下次有什麼你想嘗試的嗎？ 最好儘早將此作為例行公事，這樣你就可以在出現問題時練習給予回饋。

There are excellent trainings and [books](https://www.penguinrandomhouse.com/books/331191/difficult-conversations-by-douglas-stone-bruce-patton-and-sheila-heen/9780143118442/) that can help you deal with interpersonal conflicts and difficulties, for example on difficult conversations.

有一些優秀的培訓和 [書籍](https://www.google.com/url?sa=E&q=https%3A%2F%2Fwww.penguinrandomhouse.com%2Fbooks%2F331191%2Fdifficult-conversations-by-douglas-stone-bruce-patton-and-sheila-heen%2F9780143118442%2F) 可以幫助你處理人際衝突和困難，例如關於困難的對話。

Face the challenges as a team and don't leave conflicts to individuals. You can do this for example by organising a session on pairing in which you discuss how to deal with difficulties together. Start the session by collecting the benefits of pairing, so that you know what you all want to get out of it. Afterwards collect the challenges each individual feels when pairing. Now the group can think about which actions might help to improve. You could also collect the hot button triggers of the team members: What makes you immediately feel uncomfortable when pairing?

將挑戰視為一個團隊來面對，不要將衝突留給個人。 例如，你可以透過組織一次關於協同工作的會議來做到這一點，在會議中你討論如何一起處理困難。 透過收集協同工作的好處來開始會議，這樣你就可以知道你們都想從中獲得什麼。 然後收集每個人在協同工作時感受到的挑戰。 現在，小組可以思考哪些行動可能有助於改善。 你也可以收集團隊成員的熱點觸發器： 在協同工作時，什麼會讓你立刻感到不舒服？

### Interruptions by meetings

Have you ever had days full of meetings, and you get the impression you are not getting anything done? This probably happens in every delivery team. Meetings are necessary to discuss, plan and agree on things you are going to build, but on the other hand they interrupt the flow. When a team practices pair programming the effect of too many meetings can get even worse. If each of the persons pairing has meetings at different times, the interruptions are multiplied.

你是否曾經有過充滿會議的日子，並且你覺得你什麼都沒有完成？ 這可能發生在每個交付團隊中。 會議對於討論、規劃和協商你將要構建的內容是必要的，但另一方面，它們會打斷流程。 當團隊實踐協同編程時，過多會議的影響可能會變得更糟。 如果協同工作的每個人在不同的時間開會，則中斷次數會成倍增加。

#### Ways to tackle

One approach is to limit the time slots in which meetings happen, for example by defining core pairing hours without meetings, or by blocking out no-meeting-times with rules like “no meetings after noon”.

一種方法是限制會議發生的時間段，例如透過定義沒有會議的核心協同工作時間，或透過制定“中午後沒有會議”等規則來阻止非會議時間。

It is also worth thinking about meeting length and overall amount. Which meetings do you really need? What goals do they have and how can you improve their quality, for example with proper preparation, facilitation, and a clear agenda.

同樣值得思考會議時長和總量。 你真正需要哪些會議？ 它們有什麼目標，以及你如何提高它們的品質，例如透過適當的準備、引導和清晰的議程。

But one thing is for sure: There will always be meetings. So how to deal with that as a pair? Check your calendars together at the beginning of your pairing session, make sure you have enough time to start pairing at all. If you have any meetings consider attending them as a pair. Rely on your Product Owner, or other non-pairing team members, to keep interruptions away from the team in the core pairing hours.

但可以肯定的是： 總會有會議。 那麼，作為一對協同工作者，如何處理這個問題呢？ 在你的協同工作會議開始時一起檢查你的日曆，確保你有足夠的時間來開始協同工作。 如果你有任何會議，請考慮作為一對一起參加。 依靠你的產品負責人或其他非協同工作的團隊成員，以使干擾遠離核心協同工作時間的團隊。

### Different skill levels

When two people with different experience levels pair on a topic, this often leads to false assumptions on how much each of them can contribute, or frustrations because of difference in pace.

當兩個具有不同經驗水平的人協同處理一個主題時，這通常會導致對他們每個人可以貢獻多少的錯誤假設，或者因為步調差異而感到沮喪。

#### Ways to tackle

If your pair has more experience on the topic: Don't assume they know best. Maybe the need to explain why they are doing things the way they are will bring them new insights. Asking questions on the how and why can lead to fruitful discussions and better solutions.

如果你的協同夥伴對該主題有更多經驗： 不要假設他們最了解。 也許解釋他們為什麼以他們的方式做事的需求會給他們帶來新的見解。 詢問關於如何和為什麼的問題可以引發富有成效的討論和更好的解決方案。

If your pair has less experience on the topic: Don't assume they cannot contribute much to the solution. You might be stuck wearing blinders and a different viewpoint can help you to come to a better solution. Also, remember that having to explain a concept is a great opportunity to test if you've really understood it and thought it all the way through.

如果你的協同夥伴對該主題的經驗較少： 不要假設他們對解決方案貢獻不大。 你可能陷入了戴著眼罩的困境，不同的觀點可以幫助你找到更好的解決方案。 此外，請記住，必須解釋一個概念是一個很好的機會來測試你是否真正理解了它並徹底思考了它。

It also helps to be aware of different learning stages to understand how the learning process from novice to expert works. Dan North has described this very nicely in his talk [Patterns of Effective Teams](https://www.youtube.com/watch?v=lvs7VEsQzKY). He introduces the [Dreyfus Model of Skills Acquisition](https://apps.dtic.mil/dtic/tr/fulltext/u2/a084551.pdf) as a way to understand the different stages of learning, and what combining them means in the context of pairing.

了解不同的學習階段也有助於理解從新手到專家的學習過程是如何運作的。 Dan North 在他的演講 [高效團隊模式](https://www.google.com/url?sa=E&q=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3Dlvs7VEsQzKY) 中對此進行了很好的描述。 他介紹了 [技能習得的 Dreyfus 模型](https://www.google.com/url?sa=E&q=https%3A%2F%2Fapps.dtic.mil%2Fdtic%2Ftr%2Ffulltext%2Fu2%2Fa084551.pdf) 作為理解不同學習階段的一種方式，以及在協同工作的背景下，將它們結合起來意味著什麼。

### Power Dynamics

Dealing with power dynamics is probably one of the hardest challenges in this list. Pair programming does not happen in a space without hierarchies. There are formal hierarchies, for example between a manager and their report, and informal ones. Examples for informal hierarchies are:

處理權力動態可能是此清單中最困難的挑戰之一。 協同編程並不是在沒有層級結構的空間中發生的。 有正式的層級結構，例如經理和他們的下屬之間，以及非正式的層級結構。 非正式層級結構的例子有：

- junior - senior
- non-men - men
- career changers - folks with a CS degree
- People of color - white folks

- 初級 - 高級
- 非男性 - 男性
- 職業轉換者 - 擁有計算機科學學位的人
- 有色人種 - 白人

And these are just a few. Power dynamics are fluid and intersectional. When two people pair, multiple of those dynamics can be in play and overlap. To get an idea of how power imbalance can impact pairing, here are a few examples.

這些只是一些。 權力動態是流動的和交叉的。 當兩個人協同工作時，其中多個動態可能會發揮作用並重疊。 為了了解權力失衡如何影響協同工作，以下是一些例子。

- One person is dominating the pairing session by hogging the keyboard and not giving room to their pairing partner.
- One person stays in a teaching position and attitude all the time.
- One person is not listening to the other one, and dismissing their suggestions.

- 一個人透過霸佔鍵盤並且不給他們的協同工作夥伴空間來主導協同工作會議。
- 一個人始終保持教學地位和態度。
- 一個人不聽另一個人，並駁回他們的建議。

It sometimes can be subtle to tie these situations back to hierarchies, you often just think that you don't get along with each other. But the underlying issue is often times influenced by an imbalance between the two folks pairing.

有時很難將這些情況與層級結構聯繫起來，你通常只是認為你彼此不合。 但潛在的問題通常受到協同工作的兩個人之間不平衡的影響。

Sarah Mei has written an [excellent series of tweets](https://twitter.com/sarahmei/status/991001357455835136) on the topic and has also given a talk that covers [power dynamics in agile](https://www.youtube.com/watch?v=YL-6RCTywbc) in a more general way.

Sarah Mei 撰寫了一系列關於該主題的 [優秀推文](https://www.google.com/url?sa=E&q=https%3A%2F%2Ftwitter.com%2Fsarahmei%2Fstatus%2F991001357455835136)，並且還發表了一個演講，更廣泛地涵蓋了 [敏捷中的權力動態](https://www.google.com/url?sa=E&q=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DYL-6RCTywbc)。

#### Ways to tackle

The first step to tackle this is for the person on the upward side of the power dynamic to acknowledge and admit to themselves their position. Only then can you honestly reflect on interactions you have with your pairing partner, and how power dynamics impact them. Try to think about your own positionality and situatedness. What can you actively do to neutralize power imbalance?

解決這個問題的第一步是權力動態上層的人承認並承認自己所處的地位。 只有這樣，你才能誠實地反思你與協同夥伴的互動，以及權力動態如何影響他們。 嘗試思考你自己的位置性和情境性。 你可以積極做些什麼來消除權力失衡？

Recognizing these types of differences and adapting our behaviour to improve collaboration can be hard. It requires a lot of self reflection. There are trainings that can help individuals or teams with this, for example “anti-bias” or [ally skills](https://frameshiftconsulting.com/ally-skills-workshop/) trainings.

認識到這些類型的差異並調整我們的行為以改善協作可能很困難。 它需要大量的自我反思。 有一些培訓可以幫助個人或團隊完成此操作，例如“反偏見”或 [盟友技能](https://www.google.com/url?sa=E&q=https%3A%2F%2Fframeshiftconsulting.com%2Fally-skills-workshop%2F) 培訓。

### Pairing with lots of Unknowns

When you work on a large topic where both of you don't have an idea how to solve a problem, the usual pairing styles often don't work as well. Let's say you need to use a technology for the first time, or try out a new approach or pattern. Researching and experimenting together works in some constellations, but it can also be frustrating because we all have different approaches to figuring out how things work, and we read and learn at different paces.

當你處理一個大型主題，你們兩個人都不知道如何解決問題時，通常的配對方式通常效果不佳。 假設你需要第一次使用某項技術，或者嘗試一種新的方法或模式。 一起研究和實驗在某些情況下是可行的，但它也可能令人沮喪，因為我們都使用不同的方法來弄清楚事情是如何運作的，並且我們以不同的步調閱讀和學習。

#### Ways to tackle

When there are lots of unknowns, e.g. you work with a new technology, think about doing a [spike](https://en.wikipedia.org/wiki/Spike_\(software_development\)) to explore the topic and learn about the technology before you actually start working. Don't forget to share your findings with the team, maybe you have a knowledge exchange session and draw some diagrams you can put up in the team space.

當有很多未知數時，例如你使用一項新技術，請考慮執行 [尖峰](https://www.google.com/url?sa=E&q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FSpike_\(software_development\)) 以探索主題並在實際開始工作之前了解該技術。 不要忘記與團隊分享你的發現，也許你可以舉行一次知識交流會議，並繪製一些可以張貼在團隊空間中的圖表。

In these situations, remember to take on the mindset of [pair development](https://martinfowler.com/articles/on-pair-programming.html#PairDevelopment), as opposed to pair _programming_. It's okay to split up to do research - maybe after agreeing on the set of questions you need to answer together.

在這些情況下，請記住採取 [配對開發](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Farticles%2Fon-pair-programming.html%23PairDevelopment) 的心態，而不是配對 編程。 分開進行研究是可以的 - 也許在就你需要一起回答的一系列問題達成共識之後。

### No time for yourself

We've talked about how being in a constant conversation with each other can be pretty energy draining. Most people also need some time on their own throughout the day. That is especially true for more [introverted folks](https://www.ted.com/talks/susan_cain_the_power_of_introverts?language=en).

我們已經討論過，與彼此不斷對話可能非常耗費精力。 大多數人也需要一些時間獨自度過一天。 對於更多 [內向的人](https://www.google.com/url?sa=E&q=https%3A%2F%2Fwww.ted.com%2Ftalks%2Fsusan_cain_the_power_of_introverts%3Flanguage%3Den) 來說，尤其如此。

When working solo, we quite naturally take time to dig into a topic or learn when we need to. But that can feel like an interruption in pairing. So how can you take that alone and learning time when needed?

當我們獨自工作時，我們會很自然地花時間深入研究一個主題或在我們需要時學習。 但這在協同工作時可能會感覺像是一種打斷。 那麼，你如何在需要時抽出時間獨自學習？

#### Ways to tackle

Again, don't pair 8 hours a day, agree on core coding hours with your team and keep it to a maximum of 6 hours per day. Maybe you also want to allocate a few hours self learning time.

同樣，不要每天協同工作 8 小時，與你的團隊達成核心編碼時間協議，並將其限制為每天最多 6 小時。 也許你還想分配幾個小時的自學時間。

When a pair feels that they don't have the collective knowledge to approach a problem, split up to read up and share back, then continue implementation.

當一對搭檔覺得他們沒有集體知識來處理一個問題時，可以分開來閱讀並分享回去，然後繼續實施。

### Rotations lead to context switching

Knowledge sharing is one of the benefits of pairing, and we have mentioned how [rotations](https://martinfowler.com/articles/on-pair-programming.html#PairRotations) can further increase that effect. On the other hand, too may rotations leat to frequent context switching.

知識共享是協同工作的好處之一，我們已經提到 [輪換](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Farticles%2Fon-pair-programming.html%23PairRotations) 如何進一步增加這種效果。 另一方面，太多的輪換會導致頻繁的上下文切換。

#### Ways to tackle

Find a balance between frequency of rotations and the possibility for a new pairing partner to get enough context on the story and contribute properly. Don't rotate for the rotation's sake, think about if and why it is important to share a certain context, and give it enough time to be effective.

在輪換頻率和一個新的協同夥伴獲得足夠的故事背景並適當貢獻的可能性之間找到平衡。 不要為了輪換而輪換，想想分享某個背景資訊是否重要以及為什麼重要，並給予它足夠的時間才能有效。

### Pairing requires vulnerability

> To pair requires vulnerability. It means sharing all that you know and all that you don't know. This is hard for us. Programmers are supposed to be smart, really-crazy-smart. Most people look at what we do and say 'I could never do that.' It makes us feel a bit special, gives us a sense of pride and pride creates invulnerability.
> 
> 協同工作需要脆弱性。 這意味著分享你所知道的一切和你所不知道的一切。 這對我們來說很困難。 程式設計師應該很聰明，非常非常聰明。 大多數人看到我們所做的事情都會說「我永遠也做不到」。 這讓我們感覺有點特別，給我們一種自豪感，而自豪感會創造無懈可擊性。
> 
> -- [Tom Howlett](https://diaryofascrummaster.wordpress.com/2013/09/30/the-shame-of-pair-programming/)

When you pair, it can be hard to show that you don't know something, or feel insecure about a decision. Especially in an industry where myths like the [10x engineer](https://www.thoughtworks.com/radar/techniques?blipid=201911057) regularly make their rounds, and where we have a tendency to [judge each other](https://blog.aurynn.com/2015/12/16-contempt-culture) by what languages we use, or what design decisions we took 5 years ago.

當你協同工作時，很難表現出你不知道某件事，或者對一個決定感到不安全。 尤其是在像 [10 倍工程師](https://www.google.com/url?sa=E&q=https%3A%2F%2Fwww.thoughtworks.com%2Fradar%2Ftechniques%3Fblipid%3D201911057) 這樣的神話經常流傳的行業中，以及在我們傾向於透過我們使用的語言或我們 5 年前做出的設計決策來 [互相評判](https://www.google.com/url?sa=E&q=https%3A%2F%2Fblog.aurynn.com%2F2015%2F12%2F16-contempt-culture) 的地方。

Vulnerability is often connected with weakness and in most modern cultures the display of strength is the norm. But as the researcher Brené Brown has laid out in several talks and books, vulnerability is actually a very important ingredient for innovation and change.

脆弱性通常與軟弱聯繫在一起，並且在大多數現代文化中，展示力量是常態。 但正如研究人員 Brené Brown 在幾次演講和書籍中所闡述的那樣，脆弱性實際上是創新和變革的一個非常重要的因素。

>

> Vulnerability is the birthplace of Innovation, Creativity and Change.
> 
> 脆弱性是創新、創造力和變革的發源地。
> 
> -- [Brené Brown](https://www.youtube.com/watch?v=psN1DORYYV0)

#### Ways to tackle

Showing vulnerability requires courage and creating an environment where people feel safer to show that their vulnerable. Again, this is all about building teams where people trust each other (regular 1:1s, Feedback, culture where people can ask questions, etc)

展現脆弱性需要勇氣，並創造一個人們感到更安全地展現他們脆弱性的環境。 同樣，這一切都與建立人們彼此信任的團隊有關（定期 1:1、回饋、人們可以提問的文化等）

Being vulnerable is easier and less risky for people on the team who have more authority, either naturally (e.g. because they are well-respected already), or institutionally (e.g. because they have a title like “Tech Lead”). So it's important that those people start and role model this, making it the norm and therefore safer for others to be vulnerable as well.

對於團隊中擁有更多權力的人來說，表現出脆弱性更容易且風險更小，無論是自然地（例如，因為他們已經受到尊重），還是制度上（例如，因為他們擁有像“技術主管”這樣的頭銜）。 因此，重要的是，這些人開始並示範這一點，使其成為常態，因此對於其他人來說，也更安全地展現脆弱性。

### Convincing managers and co-workers

Advocates of pair programming often struggle to convince their managers or their co-workers to make pairing part of a team's daily routine.

協同編程的倡導者經常努力說服他們的經理或同事將協同工作納入團隊的日常工作中。

#### Ways to tackle

There is not a simple recipe to persuade others of the efficacy of pair programming. However, a key element should always be to take time to talk about it first, and make sure that everybody has the same understanding (e.g. by reading this article :-) ). Then find a way to try it out, either by starting with one pair who share their experience with the others, or by proposing a team experiment, like “let's pair by default for the next 2 sprints”. Make sure to build in opportunities for feedback and retrospection to share what is going well and what you are struggling with.

沒有一個簡單的方法來說服其他人協同編程的有效性。 然而，一個關鍵要素應該始終是花時間首先討論它，並確保每個人都有相同的理解（例如，透過閱讀本文 :-)）。 然後找到一種方法來嘗試它，可以從一對與其他人分享他們經驗的人開始，或者透過提出一個團隊實驗，例如“讓我們預設在接下來的 2 個衝刺中進行協同工作”。 確保建立回饋和回顧的機會，以分享什麼進展順利以及你在掙扎什麼。

Ultimately, you can't force a practice on people, and it does not work for everybody. You might end up pairing with only a part of the team - at least in the beginning. From our experience the best way to convince people is by having a regular exposure to the practice, experiencing the benefits and fun their team members have while pairing.

最終，你不能強迫人們實踐，而且它並不適用於每個人。 你最終可能只與團隊的一部分人協同工作 - 至少在開始時是這樣。 根據我們的經驗，說服人們的最佳方法是定期接觸實踐，體驗他們的團隊成員在協同工作時所獲得的好處和樂趣。

A question that comes up most frequently in this situation is the economics of the practice: Does pairing just cost double the money, and is it ultimately worth extra cost because of the increased quality and team benefits? There are a few studies on the topic, most notably [this one](https://collaboration.csc.ncsu.edu/laurie/Papers/XPSardinia.PDF), that are cited to provide evidence that pairing is worth it. We are wary though of attempts to “scientifically prove” pairing effectiveness. Software development is a process full of change and uncertainty, with a lot of outcome beyond lines of code that is hard to compare and measure, like analysis, testing, or [quality](https://martinfowler.com/articles/is-quality-worth-cost.html). Staunch opponents of pairing will always find ways to poke holes into the reproducibility of any “scientific” experiments set up to prove development productivity. In the end, you need to show that it works for YOU - and the only way to do that is to try it in your environment.

在這種情況下最常出現的一個問題是實踐的經濟性： 協同工作是否只花費雙倍的錢，並且由於提高了品質和團隊利益，最終是否值得額外花費？ 有一些關於該主題的研究，最值得注意的是 [這篇](https://www.google.com/url?sa=E&q=https%3A%2F%2Fcollaboration.csc.ncsu.edu%2Flaurie%2FPapers%2FXPSardinia.PDF)，它被引用來提供協同工作值得的證據。 然而，我們對“科學地證明”協同工作有效性的嘗試持謹慎態度。 軟體開發是一個充滿變化和不確定性的過程，除了程式碼行之外，還有很多難以比較和衡量的結果，例如分析、測試或 [品質](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Farticles%2Fis-quality-worth-cost.html)。 堅定的協同工作反對者總會找到方法來在旨在證明開發生產力的任何“科學”實驗的可複製性上挑毛病。 最後，你需要證明它對你有效 - 而做到這一點的唯一方法是在你的環境中嘗試它。

## To pair or not to pair

Our experience clearly shows that pair programming is a crucial practice to create high quality, maintainable software in a sustainable way (see [“Benefits”](https://martinfowler.com/articles/on-pair-programming.html#Benefits)). However, we also don't believe it is helpful to approach it dogmatically and _always_ pair. How exactly pair programming can be effective for you, how much of it, and for which tasks, can vary. We've found it useful to set pair programming as the “sensible default” on teams, and then discuss whenever we want to make an exception.

我們的經驗清楚地表明，協同編程是創建高品質、可維護軟體的關鍵實踐（參見 [“好處”](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Farticles%2Fon-pair-programming.html%23Benefits)）。 但是，我們也不認為以教條主義的方式進行處理並且 始終 協同工作是有幫助的。 協同編程對你來說如何有效、多少以及用於哪些任務可能會有所不同。 我們發現將協同編程設定為團隊的“明智預設”很有用，然後討論我們何時想要做出例外。

Let's look at a few examples where it's helpful to balance how and when you pair.

讓我們看幾個例子，在這些例子中，平衡你協同工作的方式和時間是有幫助的。
### Boring Tasks

Some coding tasks are “boring”, e.g. because they are about using some well defined boilerplate approach - so maybe you don't need to pair? The whole team already knows this type of approach, or it's very easy to grasp, so knowledge sharing is not that important? And live code review is less useful because the well-established pattern at hand has been used successfully in the past? So yes, maybe you don't need to pair

有些編碼任務是“無聊的”，例如因為它們是關於使用一些定義明確的樣板方法 - 所以也許你不需要協同工作？ 整個團隊已經知道這種方法，或者它很容易掌握，所以知識共享並不那麼重要？ 並且實時程式碼審查不太有用，因為手頭上完善的模式過去已經成功使用過？ 所以，是的，也許你不需要協同工作。

However, always consider that [rote tasks might be a smell for bad design](https://www.martinfowler.com/bliki/PairProgrammingMisconceptions.html): Pairing can help you find the right abstraction for that boring code. It's also more probable to miss things or make cursory errors when your brain goes into “this is easy” autopilot.

然而，始終考慮到 [重複的任務可能是不良設計的徵兆](https://www.google.com/url?sa=E&q=https%3A%2F%2Fwww.martinfowler.com%2Fbliki%2FPairProgrammingMisconceptions.html)： 協同工作可以幫助你為那個無聊的程式碼找到正確的抽象。 當你的大腦進入“這很容易”的自動駕駛模式時，也更有可能錯過事情或犯下粗略的錯誤。

### “Could I Really Do This By Myself?”

Pairing has a lot of benefits for programmers who are just starting out, because it is an opportunity to learn relatively quickly from a more experienced member of the team. However, junior programmers can also experience a loss of confidence in their own abilities when pairing. “Could I really do this without somebody looking over my shoulder?”. They also miss out on learning how to figure things out by themselves. We all go through moments of frustration and unobserved experimentation with debugging and error analysis that ultimately make us better programmers. Running into a problem ourselves is often a more effective learning experience than somebody telling us that we are going to walk into it.

協同編程對於剛入門的程式設計師來說有很多好處，因為它提供了一個相對快速地從團隊中更有經驗的成員那裡學習的機會。 然而，初級程式設計師在協同工作時也會對自己的能力失去信心。 「沒有人在我身邊看著，我真的可以完成嗎？」。 他們也錯過了學習如何自己弄清楚事情的機會。 我們都會經歷沮喪的時刻，並在沒有人注意到的情況下進行除錯和錯誤分析，最終使我們成為更好的程式設計師。 我們自己遇到問題通常比有人告訴我們我們將會遇到問題更有效。

There are a few ways to counteract this. One is to let junior programmers work by themselves from time to time, with a mentor who regularly checks in and does some code review. Another way is letting the more junior programmers on the team pair with each other. They can go through finding solutions together, and still dig themselves out of rabbit holes faster than if they were coding by themselves. Finally, if you are the more experienced coder in a pair, make sure to be in the navigator's seat most of the time. Give the driver space to figure things out - it's sometimes just a matter of waiting a little bit until you hit that next wall together, instead of pointing it out beforehand.

有幾種方法可以抵消這種情況。 一種是讓初級程式設計師不時地自己工作，並由一位導師定期檢查並進行一些程式碼審查。 另一種方法是讓團隊中較初級的程式設計師彼此協同工作。 他們可以一起尋找解決方案，並且仍然比他們自己編碼更快地擺脫困境。 最後，如果你是一對搭檔中更有經驗的程式設計師，請確保大部分時間坐在領航員的位置上。 給駕駛員空間來弄清楚事情 - 有時只是需要稍微等待一下，直到你一起遇到下一個障礙，而不是事先指出它。

### Code Review vs. Pairing

> The advantage of pair programming is its gripping immediacy: it is impossible to ignore the reviewer when he or she is sitting right next to you.
> 
> 協同編程的優勢在於其令人著迷的即時性： 當審查者坐在你旁邊時，不可能忽略他或她。
> 
> -- [Jeff Atwood](https://blog.codinghorror.com/pair-programming-vs-code-reviews/)

![](https://martinfowler.com/articles/on-pair-programming/code_review.png)

Many people see the existence of a code review process as reason enough not to need pair programming. We disagree that code reviews are a good enough alternative to pairing.

許多人認為程式碼審查流程的存在是沒有必要進行協同編程的充分理由。 我們不同意程式碼審查是協同工作的一個很好的替代方案。

Firstly, there are usually a few dynamics at play that can lead to sloppy or superficial code reviews. For example, when coder and reviewer rely too much on each other without making that explicit: The coder might defer a few little decisions and improvements, thinking that problems will be caught in the review. While the reviewer then relies on the diligence of the coder, trusting their work and not looking too closely at the code. Another dynamic at play is that of the [sunk cost fallacy](https://www.behavioraleconomics.com/resources/mini-encyclopedia-of-be/sunk-cost-fallacy/): We are usually reluctant to cause rework for something that the team already invested in.

首先，通常有一些動態在起作用，可能會導致草率或膚淺的程式碼審查。 例如，當程式設計師和審查者過度依賴彼此而沒有明確說明這一點時： 程式設計師可能會推遲一些小的決定和改進，認為問題會在審查中被發現。 而審查者則依賴程式設計師的勤奮，信任他們的工作，並且不會太仔細地查看程式碼。 另一個在起作用的動態是 [沉沒成本謬誤](https://www.google.com/url?sa=E&q=https%3A%2F%2Fwww.behavioraleconomics.com%2Fresources%2Fmini-encyclopedia-of-be%2Fsunk-cost-fallacy%2F)： 我們通常不願意為團隊已經投入的事情造成返工。

Secondly, a code review process can disrupt the team's flow. Picking up a review task requires a context switch for somebody. So the more often code reviews occur, the more disruptive they will be for reviewers. And they should occur frequently, to ensure continuous integration of small changes. So a reviewer can become a bottleneck to integrate and deploy, which adds time pressure - again, something that leads to potentially less effective reviews.

其次，程式碼審查流程可能會擾亂團隊的流程。 接手審查任務需要有人進行上下文切換。 因此，程式碼審查發生的越頻繁，它們對審查者的干擾就越大。 並且它們應該經常發生，以確保持續整合小的變更。 因此，審查者可能會成為整合和部署的瓶頸，這增加了時間壓力 - 同樣，這可能會導致效率較低的審查。


> [!NOTE] 
> Check out the [2017 edition of the “State of Devops Report”](https://services.google.com/fh/files/misc/state-of-devops-2017.pdf) for more on the performance of teams who practice trunk-based development: “Last year's results confirmed that the following development practices contribute to higher software delivery performance: Merging code into trunk on a daily basis; Having branches or forks with very short lifetimes (less than a day); Having fewer than three active branches.”
> 
> 請查看 [“Devops 狀況報告”的 2017 年版本](https://www.google.com/url?sa=E&q=https%3A%2F%2Fservices.google.com%2Ffh%2Ffiles%2Fmisc%2Fstate-of-devops-2017.pdf) 以了解更多關於實踐基於主幹開發的團隊的績效：“去年的結果證實，以下開發實踐有助於提高軟體交付績效： 每天將程式碼合併到主幹中； 擁有生命週期非常短（少於一天）的分支或複刻； 擁有的活動分支少於三個。”

With [Continuous Integration](https://martinfowler.com/articles/continuousIntegration.html) (and Delivery), we want to reduce risk by delivering small chunks of changes frequently. In its original definition, this means practicing [trunk-based development](https://paulhammant.com/2013/04/05/what-is-trunk-based-development/). With trunk-based development, delayed code reviews are even less effective, because the code changes go into the master branch immediately anyway. So pair programming and continuous integration are two practices that go hand in hand.

透過 [持續整合](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Farticles%2FcontinuousIntegration.html)（和交付），我們希望透過頻繁地交付小塊變更來降低風險。 在其原始定義中，這意味著實踐 [基於主幹的開發](https://www.google.com/url?sa=E&q=https%3A%2F%2Fpaulhammant.com%2F2013%2F04%2F05%2Fwhat-is-trunk-based-development%2F)。 透過基於主幹的開發，延遲的程式碼審查甚至效率更低，因為程式碼變更無論如何都會立即進入主分支。 因此，協同編程和持續整合是兩種密切相關的實踐。

An approach we've seen teams use effectively is to pair by default, but use pull requests and code reviews for the exceptional cases when somebody has to change production code without pairing. In these setups, you should carefully monitor as a team that your pull requests don't live for too long, to make sure you still practice continuous integration.

我們看到團隊有效使用的一種方法是預設進行協同工作，但對於當有人必須在沒有協同工作的情況下變更生產程式碼的特殊情況，則使用拉取請求和程式碼審查。 在這些設置中，作為一個團隊，你應該仔細監控你的拉取請求不要存在太長時間，以確保你仍然實踐持續整合。

## But really, why bother?

We talked a lot about the benefits of pair programming, but even more about its challenges. Pairing requires a lot of different skills to get it right, and might even influence other processes in the team. So why bother? Is it really worth the hassle?

我們談論了很多關於協同編程的好處，但更多的是關於它的挑戰。 協同工作需要很多不同的技能才能做好，甚至可能會影響團隊中的其他流程。 那麼，為什麼要費心呢？ 這真的值得嗎？

For a team to be comfortable with and successful at pair programming, they will have to work on all the skills helpful to overcome its challenges: Concentration and focus, task organisation, time management, communication, giving and receiving feedback, empathy, vulnerability - and these are actually all skills that help immensely to become a well-functioning, collaborative and effective team. Pairing gives everybody on the team a chance to work on these skills together.

對於一個團隊來說，要能夠舒適地並且成功地進行協同編程，他們將必須努力掌握所有有助於克服其挑戰的技能： 專注和集中、任務組織、時間管理、溝通、給予和接受回饋、同理心、脆弱性 - 而這些實際上都是極大地幫助成為一個運作良好、協作且高效團隊的技能。 協同工作讓團隊中的每個人都有機會一起努力掌握這些技能。


> [!NOTE] 
> Pairing unlocks the potential of individuals and teams to learn
> 
> 協同工作釋放了個人和團隊學習的潛力
> 
> -- [Dave Farley](https://martinfowler.com/articles/farley-unlock-learning)

Consider this headline from Harvard Business Review: [“Diverse Teams Feel Less Comfortable - and That's Why They Perform Better”](https://hbr.org/2016/09/diverse-teams-feel-less-comfortable-and-thats-why-they-perform-better). The authors are making the point that “Homogeneous teams feel easier - but easy is bad for performance. (...) this idea goes against many people's intuitions”. To explain, they point out a cognitive bias called the fluency heuristic: “We prefer information that is more easily processed, and judge it to be more true, or more beautiful.”

考慮一下哈佛商業評論的標題： [“多元化的團隊感到不太舒服 - 這就是它們表現更好的原因”](https://www.google.com/url?sa=E&q=https%3A%2F%2Fhbr.org%2F2016%2F09%2Fdiverse-teams-feel-less-comfortable-and-thats-why-they-perform-better)。 作者指出，“同質化的團隊感覺更容易 - 但容易對績效不利。(...) 這個想法違背了許多人的直覺”。 為了解釋，他們指出了 一種被稱為流暢性啟發法的認知偏差：“我們更喜歡更容易處理的資訊，並認為它更真實或更美麗。”

This bias makes us strive for simplicity, which serves us very well in a lot of situations in software development. But we don't think it serves us well in the case of pair programming. Pairing feels hard – but that doesn't necessarily mean it's not good for a team. And most importantly, it does not have to stay hard. In [this talk](https://www.youtube.com/watch?v=S92vVAEofes), Pia Nilsson describes measures her team at Spotify took to get over the uncomfortable friction initially caused by introducing practices like pair programming. Among other things, she mentions feedback culture, non-violent communication, [psychological safety](https://hbr.org/2017/08/high-performing-teams-need-psychological-safety-heres-how-to-create-it), humility, and having a sense of purpose.

這種偏差使我們力求簡潔，這在軟體開發的許多情況下都為我們提供了很好的服務。 但我們認為它在協同編程的情況下沒有為我們提供很好的服務。 協同工作感覺很困難 - 但這並不一定意味著它對團隊不好。 最重要的是，它不必一直很困難。 在 [這個演講](https://www.google.com/url?sa=E&q=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DS92vVAEofes) 中，Pia Nilsson 描述了她在 Spotify 的團隊為克服最初因引入像協同編程這樣的實踐而引起的不舒服的摩擦而採取的措施。 除此之外，她還提到了回饋文化、非暴力溝通、[心理安全](https://www.google.com/url?sa=E&q=https%3A%2F%2Fhbr.org%2F2017%2F08%2Fhigh-performing-teams-need-psychological-safety-heres-how-to-create-it)、謙遜以及擁有目標感。

Pair programming, extreme programming, and agile software development as a whole are all about embracing change. Agile software practitioners acknowledge that change is inevitable, so they want to be prepared for it.

協同編程、極限編程和敏捷軟體開發總體上都是關於擁抱變化的。 敏捷軟體從業者承認變化是不可避免的，因此他們希望為此做好準備。

We suggest that another thing we should embrace and prepare for is friction, because it's also inevitable on the way to becoming a highly effective, diverse team. By embracing friction we do NOT mean to say, “let's just have lots of conflicts and we'll get better”. What we mean is that teams should equip themselves with the tools necessary to deal with friction, and have them in their toolbox by default, not just when the team is already having problems. Practice feedback, improve team communication, take measures to create a psychologically safe environment.

我們建議我們應該擁抱和為之做好準備的另一件事是摩擦，因為它也是成為一個高效、多元化團隊的道路上不可避免的。 透過擁抱摩擦，我們並不是要說“讓我們只是有很多衝突，我們會變得更好”。 我們的意思是，團隊應該配備必要的工具來處理摩擦，並且預設將它們放在他們的工具箱中，而不僅僅是在團隊已經出現問題時才使用。 練習回饋、改善團隊溝通、採取措施創造一個心理安全的環境。

We believe that pair programming is often avoided because it can create friction, but we would ask you to give it a chance. If you consciously treat it as an improvable skill, and work on getting better at it, you will end up with a more resilient team.

我們認為協同編程經常被避免，因為它可能會產生摩擦，但我們希望你給它一個機會。 如果你有意識地將其視為一種可以改進的技能，並努力做得更好，你最終將會擁有一個更具韌性的團隊。


