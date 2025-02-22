---
date: '2025-02-22T11:00:10+08:00'
title: "It's Not Just Standing Up Patterns for Daily Standup Meetings"
tags: ["Martin Fowler"]
---

[Source](https://martinfowler.com/articles/itsNotJustStandingUp.html)


_Daily stand-up meetings have become a common ritual of many teams, especially in Agile software development. However, there are many subtle details that distinguish effective stand-ups and a waste of time._

每日站立會議已成為許多團隊的常見儀式，尤其是在敏捷軟體開發中。然而，存在許多細微的細節，區分了有效的站立會議和浪費時間的會議。

## We stand up to keep the meeting short

The daily stand-up meeting (also known as a “daily scrum”, a “daily huddle”, “morning roll-call”, etc.) is simple to describe:

每日站立會議（也稱為“每日 Scrum”、“每日聚集”、“早點名”等）很容易描述：

_The whole team meets every day for a quick status update. We stand up to keep the meeting short._

整個團隊每天會面，進行快速的進度更新。我們站著開會是為了縮短會議時間。

That's it.

就這樣。

But this short definition does not really tell you the subtle details that distinguish an effective stand-up from a waste of time.

但這個簡短的定義並沒有真正告訴你區分有效站立會議和浪費時間會議的細微細節。

So how can you tell?

那麼你如何判斷呢？

For experienced practitioners, when things go wrong with the stand-up, they will instinctively know what to adjust to fix the situation.

對於經驗豐富的從業者來說，當站立會議出現問題時，他們會本能地知道該調整什麼來解決問題。

For novices, when things go wrong, it is much less likely that they'll figure out what to do... and it’s much more likely that, given no assistance, they will simply abandon the practice altogether.

對於新手來說，當事情出錯時，他們不太可能知道該做什麼……而且更有可能的是，在沒有任何幫助的情況下，他們會直接放棄這種做法。

This would be unfortunate since well-run stand-ups add significant value to teams.

這將是不幸的，因為運營良好的站立會議可以為團隊增加顯著的價值。

In order to address this, it is important to make explicit the benefits and consequences of common practices for daily stand-ups. These patterns of daily stand-up meetings can help less experienced practitioners as well as remind more experienced practitioners of the reasons behind their intuition.

為了解決這個問題，重要的是要明確每日站立會議的常見做法所帶來的好處和後果。 這些每日站立會議的模式可以幫助經驗不足的從業者，也可以提醒更有經驗的從業者其直覺背後的原因。

## What might “good” look like?

Bob Marley’s “Get Up Stand Up” starts up... acting like a Pavlovian bell as the team gets up to wander over to stand in front of the card wall without any additional prompting. That particular song is part of a rotation that plays at the same time in the morning, every day. Some people are moving cards to their correct points in the workflow, including affixing different coloured Post-Its with additional notes. A few interested people outside the direct project team have also wandered by to see how things have progressed.

巴布·馬利 (Bob Marley) 的《Get Up Stand Up》開始播放……就像巴甫洛夫的鐘聲一樣，團隊在沒有任何額外提示的情況下起身走到卡片牆前站立。 那首特定的歌曲是每天早上同一時間播放的輪播歌曲的一部分。 有些人正在將卡片移動到工作流程中的正確位置，包括貼上不同顏色的便利貼並附上額外的註釋。 一些對直接項目團隊以外的人也過來看看進度如何。


> [!NOTE]
> As well as this article, Jason Yip writes [an active blog](http://jchyip.blogspot.com/) on software development, touching many issues around lean and agile development. The entries are short and disproportionately thought-provoking, with many links to interesting articles on people and software.
> 
> 除了這篇文章，Jason Yip 也撰寫了一個關於軟體開發的[活躍部落格](https://www.google.com/url?sa=E&q=http%3A%2F%2Fjchyip.blogspot.com%2F)，涉及關於精實和敏捷開發的許多問題。這些文章簡短且非常發人深省，其中包含許多關於人和軟體的有趣文章連結。


Noticing that the activity at the wall has stopped, the team leader starts a large timer that the team had previously purchased; they were interested in how long the daily stand-up meeting actually took.

注意到牆上的活動停止了，團隊領導啟動了一個團隊先前購買的大型計時器；他們想知道每日站立會議實際上花了多長時間。

One of the team members steps up to talk about the card on the furthest right of the board, closest to the point of deployment. He’s still having some problems with the deployment script. Another team member suggests that she can help resolve that. The sequence continues from right-to-left, top-to-bottom, people describing what is happening with each of the work items, and others chiming in if they can help resolve obstacles. On the side, the team leader is recording the raised obstacles on the improvement board.

其中一名團隊成員走上前談論看板上最右側的卡片，也就是最接近部署點的卡片。 他在部署腳本方面仍然遇到一些問題。 另一名團隊成員建議她可以幫助解決這個問題。 順序從右到左、從上到下，人們描述每個工作項目的情況，其他人則在可以幫助解決障礙時插話。 團隊領導在旁邊的改善看板上記錄著提出的障礙。

At one point, there is a slightly longer discussion exploring how to deal with a particular problem. Noticing the stall, the team leader subtly raises a finger to interrupt... just before one of the people suggest that they should take it offline.

在某個時間點，出現了一個稍長的討論，探討如何處理一個特定的問題。 團隊領導注意到停頓，巧妙地舉起一根手指來打斷……就在其中一個人建議他們應該離線討論之前。

A short time later all the cards are covered and the team leader asks if anyone else has anything else to share. Someone points out an interesting idea she had about a new feature that would make some of what was planned obsolete. This piques the interest of the product manager who always attempts to attend the stand-ups and they both agree to talk about it after.

不久之後，所有的卡片都被涵蓋了，團隊領導詢問是否還有其他人有其他要分享的事情。 有人指出她對一項新功能有一個有趣的想法，該功能會使一些計劃中的內容過時。 這引起了產品經理的興趣，他總是試圖參加站立會議，他們都同意之後再談。

The team leader then rolls his eyes as the team starts the traditional ending ceremony... 1... 2... 3... Excelsior! Not his thing, but he had to admit, it ended things on a high note.

然後，當團隊開始傳統的結束儀式時，團隊領導翻了個白眼……1... 2... 3... Excelsior! 這不是他喜歡的，但他不得不承認，它以一個高亢的音調結束了一切。

People separate and start discussing various things that were raised, including the obstacles, the new ideas, and questions about certain work items.

人們分開並開始討論提出的各種事情，包括障礙、新想法以及關於某些工作項目的問題。

## The particular set of problems that occur when people attempt to work together 當人們試圖一起工作時發生的特定問題

Daily stand-up meetings are a recurring solution to a particular set of problems that occur when a group of people attempt to work together as a team.

每日站立會議是一個反覆出現的解決方案，旨在解決當一群人試圖作為團隊一起工作時發生的特定問題。

Stand-ups are a mechanism to regularly synchronise so that teams...

站立會議是一種定期同步的機制，以便團隊...

- **Share understanding of goals.** Even if we thought we understood each other at the start (which we probably didn’t), our understanding drifts, as does the context within which we’re operating. A “team” where each team member is working toward different goals tends to be ineffective.
- **Coordinate efforts.** If the work doesn’t need to be coordinated, you don’t need a team. Conversely, if you have a team, I assume the work requires coordination. Poor coordination amongst team members tends to lead to poor outcomes.
- **Share problems and improvements.** One of the primary benefits of a team versus working alone, is that team members can help each other when someone encounters a problem or discovers a better way of doing something. A “team” where team members are not comfortable sharing problems and/or do not help each other tends to be ineffective.
- **Identify as a team.** It is very difficult to psychologically identify with a group if you don’t regularly engage with the group. You will not develop a strong sense of relatedness even if you believe them to be capable and pursuing the same goals.

- **分享對目標的理解。** 即使我們一開始認為我們彼此理解（我們可能沒有），我們的理解也會隨著我們運營的背景而漂移。 每位團隊成員都朝著不同目標努力的「團隊」往往效率低下。
- **協調努力。** 如果工作不需要協調，你就不需要團隊。 相反地，如果你有一個團隊，我假設工作需要協調。 團隊成員之間協調不力往往會導致不良的結果。
- **分享問題和改進。** 團隊與單獨工作相比的主要好處之一是，當有人遇到問題或發現更好的做事方式時，團隊成員可以互相幫助。 團隊成員不願意分享問題和/或不互相幫助的「團隊」往往效率低下。
- **以團隊身份認同。** 如果你不定期與群體互動，就很難在心理上認同一個群體。 即使你相信他們有能力並且追求相同的目標，你也不會發展出強烈的關聯感。

## Patterns of daily stand-up meetings

I’ve organised the patterns to answer the following questions:

我整理了這些模式，以回答以下問題：

- Who attends?
- What do we talk about?
- What order do we talk in?
- Where and when?
- How do we keep the energy level up?
- How do we encourage autonomy?

- 誰參加？
- 我們談論什麼？
- 我們以什麼順序談論？
- 地點和時間？
- 我們如何保持能量水平？
- 我們如何鼓勵自主性？

## Who attends?

### All Hands 全員參與

People and representatives from various areas (e.g., marketing, production support, upper management, training, etc.) wish to know about and/or contribute to the status and progress of the project. Communicating status in multiple meetings and reports requires a lot of duplicate effort.

來自各個領域的人員和代表（例如，行銷、生產支援、高層管理、培訓等）希望了解和/或貢獻項目的狀態和進度。 在多次會議和報告中傳達狀態需要大量的重複工作。

_Therefore_

Replace some or all of the meetings and reports with the daily stand-up. Anyone who is directly involved in or wants to know about the day-to-day operation of the project should attend the single daily stand-up meeting.

用每日站立會議取代部分或全部的會議和報告。 任何直接參與或想了解項目日常運營的人都應該參加單一的每日站立會議。

_But_

People not directly involved can disrupt the stand-up if they are unclear about what is expected behaviour. This may be addressed by simply informing new participants and observers of expected norms beforehand.

如果直接參與度不高的人不清楚預期的行為，他們可能會擾亂站立會議。 這可以透過事先簡單地告知新的參與者和觀察者預期的規範來解決。

Not all forms of reporting will be, nor should be, covered by the stand-up format. For example, overall project progress would be better communicated with a [Big Visible Chart](http://xprogramming.com/articles/bigvisiblecharts/) such as burn-down, burn-up, cumulative flow diagram, etc.

並非所有形式的報告都將被站立會議的形式涵蓋，也不應該這樣做。 例如，整體項目進度最好透過[大型可視化圖表](https://www.google.com/url?sa=E&q=http%3A%2F%2Fxprogramming.com%2Farticles%2Fbigvisiblecharts%2F)來傳達，例如燃盡圖、燃起圖、累積流程圖等。

### Work Items Attend 工作項目參加

**_Also Known As:_** Story-focused stand-up

**又名：** 以故事為中心的站立會議

> if the stories are so important to the project, **they** ought to be the ones speaking in the standup
> 
> 如果這些故事對項目如此重要，**它們**應該是在站立會議中發言的人
> 
> -- [Brian Marick, “Latour 3: Anthrax and standups”](http://www.exampler.com/blog/2007/11/06/latour-3-anthrax-and-standups/)

People are too **Focused on the Runners, not the Baton**. That is, everyone is busy but not necessarily progressing work items.

人們太**關注跑步者，而不是接力棒**。 也就是說，每個人都很忙，但不一定在推進工作項目。

_Therefore_

Instead of thinking of the daily stand-up as a ritual for the people, think of it as a ritual where the **Work Items Attend** (e.g., [User Stories](https://martinfowler.com/bliki/UserStory.html) in an Agile context) and the people attend only to speak for the work items... since obviously the work items can’t actually talk.

不要將每日站立會議視為人們的儀式，而是將其視為**工作項目參加**（例如，敏捷環境中的[使用者故事](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Fbliki%2FUserStory.html)），而人們僅作為工作項目的代表參加……因為顯然工作項目實際上無法說話。

The **Yesterday Today Obstacles** questions may still be used but will be from the perspective of the work item, rather than the person. This also means that not every person may talk. There is no sense of obligation to say anything that is not relevant to progress the work.

**昨天、今天、障礙** 的問題仍然可以使用，但將從工作項目的角度，而不是從個人的角度來提問。 這也意味著並非每個人都必須發言。 沒有義務說任何與推進工作無關的事情。

Because of the clearer focus, it is more likely for people to raise, and sign- up to remove, obstacles without prompting.

由於重點更加明確，人們更有可能提出並主動承擔消除障礙的責任，而無需提示。

_But_

[The lack of obligation to speak may hide problems with people who are shy or otherwise not inclined to say anything.](http://blog.mountaingoatsoftware.com/should-the-daily-standup-%20be-person-by-person-or-story-by-story/comment-page-1#comment-23428) This is more difficult to detect with a work-item focus.

[不發言的義務可能會隱藏那些害羞或不願說任何話的人的問題。](https://www.google.com/url?sa=E&q=http://blog.mountaingoatsoftware.com/should-the-daily-standup-%20be-person-by-person-or-story-by-story/comment-page-1#comment-23428) 透過以工作項目為重點的方法更難以檢測到這一點。

## What do we talk about?

### Yesterday Today Obstacles

**_Also Known As:_** Three Questions

**又名：** 三個問題

Some people are talkative and tend to wander off into **Story Telling**. Some people want to engage in **Problem Solving** immediately after hearing a problem. Meetings that take too long tend to have low energy and participants not directly related to a long discussion will tend to be distracted.

有些人很健談，並且容易陷入**說故事**。 有些人希望在聽到問題後立即進行**問題解決**。 時間過長的會議往往能量較低，並且與長時間討論沒有直接關係的參與者往往會分心。

_Therefore_

Structure the contributions using the following format:

使用以下格式建構貢獻：

1. What did I accomplish yesterday?
2. What will I do today?
3. What obstacles are impeding my progress?

4. 我昨天完成了什麼？
5. 我今天要做什麼？
6. 什麼障礙阻礙了我的進展？

These are the minimum number of questions that satisfy the goals of daily stand-ups. Other topics of discussion (e.g., design discussions, gossip, etc.) should be deferred until after the meeting.

這些是滿足每日站立會議目標的最少問題數量。 其他討論主題（例如，設計討論、閒聊等）應延遲到會議結束後進行。

Olve Maudal suggested that the questions should be reversed in order to emphasise the correct order of importance:

Olve Maudal 建議應該顛倒問題的順序，以強調正確的重要性順序：

> 1. Any impediments in your way?
> 2. What are you working on today?
> 3. What have you finished since yesterday?
>    
> 4. 你有任何阻礙嗎？
> 5. 你今天在做什麼？
> 6. 1. 自昨天以來你完成了什麼？
> 
> -- [Olve Maudal, “Daily Stand-up Meetings - Perhaps the third question should go first?”](http://olvemaudal.wordpress.com/2008/05/15/daily-stand-up-meetings-%20perhaps-the-third-question-should-go-first/)

Lasse Koskela proposed another form of these questions in order to emphasise that team members should not be **Reporting to the Leader**:

Lasse Koskela 提出了這些問題的另一種形式，以強調團隊成員不應**向領導者報告**：

> Each team member updates his peers:
> 
> 每位團隊成員更新他的同事：
> 
> In turn, each team member provides his peers with 3 pieces of information:
> 
> 依次，每位團隊成員向他的同事提供 3 條資訊：
> 
> 1. Things I have done since yesterday's meeting
> 2. Things I am going to get done today
> 3. Obstacles that I need someone to remove
> 
> 1. 自昨天會議以來我所做的事情
> 2. 我今天要做的事情 
> 3. 我需要有人移除的障礙
> 
> -- [Lasse Koskela, “On Scrum and the curse of the three questions”](http://radio.javaranch.com/lasse/2006/05/07/1147034972559.html)

Jonathan Rasmussen offered different wording in order to change the dynamic of the stand-up:

Jonathan Rasmussen 提供了不同的措辭，以改變站立會議的動態：

> - What you did to change the world yesterday
> - How you are going to crush it today
> - How you are going to blast through any obstacles unfortunate enough to be standing in your way
> 
> - 你昨天做了什麼來改變世界 
> - 你今天如何征服它
> - 你將如何摧毀任何不幸地擋在你路上的障礙
> 
> Answering these types of questions completely changes the dynamic of the stand-up. Instead of just standing there and giving an update, you are now laying it all the line and declaring your intent to the universe.
> 
> 回答這些類型的問題完全改變了站立會議的動態。 你現在不是只是站在那裡並提供更新，而是將一切都放在檯面上，並向宇宙宣告你的意圖。
> 
> -- [Jonathan Rasmusson, The Agile Samurai](https://www.amazon.com/gp/product/1934356581/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1934356581&linkCode=as2&tag=martinfowlerc-20)

There are also a number of teams that have added additional questions.

也有許多團隊添加了額外的問題。

[Buffer added a section where people share something they're working on to improve themselves.](http://joel.is/an-invitation-to-come-and-hack-on-buffer/)

  
Buffer 新增了一個部分，人們可以在這裡分享他們正在努力改進自己的事情。

Thomas Cagley suggests [trolling for risks](https://tcagley.wordpress.com/2014/07/18/stand-up-meetings-add-ons/).

Thomas Cagley 建議[尋找風險](https://www.google.com/url?sa=E&q=https%3A%2F%2Ftcagley.wordpress.com%2F2014%2F07%2F18%2Fstand-up-meetings-add-ons%2F)。

Mark Levison has found it useful to add more targeted improvement questions. The last two questions would be changed to match the specific context.

Mark Levison 發現添加更有針對性的改進問題很有用。 最後兩個問題將會更改以符合特定背景。

> 1. What did you complete yesterday?
> 2. What do you commit to today?
> 3. What are your impediments/obstacles?
> 4. What Code Smell/Missing Unit Test/... did you spot yesterday?
> 5. What improvement did you make to the code yesterday?
>    
> 6. 你昨天完成了什麼？
> 7. 你今天承諾做什麼？
> 8. 你的障礙/阻礙是什麼？
> 9. 你昨天發現了什麼程式碼異味/遺失單元測試/……？
> 10. 你昨天對程式碼做了什麼改進？
> 
> -- [Mark Levison, “Daily Stand-Up Variations”](https://agilepainrelief.com/notesfromatooluser/2011/01/daily-stand-up-variations.html#.VsoT4ZMrLeQ)

_But_

The structure is not as important as the information the answers to the questions provide. If the information is provided in a less structured protocol, it is not important to stick to a checklist. As teams mature, you may find you want to adjust the structure, which is reflective of how this pattern has already evolved.

結構不如問題答案所提供的資訊重要。 如果資訊是以較不結構化的協定提供，則堅持清單並不重要。 隨著團隊成熟，你可能會發現你想要調整結構，這反映了此模式的發展方式。

The larger question is whether **Yesterday Today Obstacles** is creating too much of a focus on personal commitment versus paying attention to the right things... which leads to **Walk the Board**.

更大的問題是**昨天 今天 障礙** 是否過於關注個人承諾，而不是關注正確的事情……這導致了 **瀏覽看板**。

### Improvement Board

**_Also Known As:_** Blockage Board, Impediment Board, Kaizen Newspaper

**又名：** 封鎖看板、阻礙看板、改善新聞報

Obstacles raised in the stand-up are not removed or otherwise addressed in a timely fashion.

在站立會議中提出的障礙沒有被及時移除或以其他方式解決。

_Therefore_

Post raised obstacles to an **Improvement Board**. This is a publicly visible whiteboard or chart that identifies raised obstacles and tracks the progress of their resolution. An **Improvement Board** can be updated outside of stand-ups and serves as a more immediate and perhaps less confronting way to initially raise obstacles. A common mistake is to not write large enough to allow people to read the blockages from a distance.

將提出的障礙張貼到**改善看板**上。 這是一個公開可見的白板或圖表，用於識別提出的障礙並追蹤其解決進展。 **改善看板**可以在站立會議之外進行更新，並且可以作為更直接、可能更不具對抗性的方式來初步提出障礙。 一個常見的錯誤是寫得不夠大，無法讓人在遠處閱讀阻礙。

The simple act of writing an issue down and therefore explicitly acknowledging it is a very reliable way to reduce drawn out conversations. So even if not everyone agrees that any particular item is an obstacle, it is worth simply writing it down for discussion after the meeting has ended.

簡單地寫下一個問題，從而明確地承認它，是一種非常可靠的方式，可以減少冗長的對話。 因此，即使不是每個人都同意任何特定項目都是障礙，也值得簡單地將其寫下來以便在會議結束後進行討論。

Including an occurrence count with each raised obstacle highlights which issues are generally more important to deal with first.

在每個提出的障礙中包含發生次數，可以突顯哪些問題通常更重要，需要首先處理。

The board design can vary in a few ways. For example a table structure such as the following:

看板設計可以有幾種不同的方式。 例如，一個表格結構，如下所示：

| Problem         | Count                        | Containment         | Countermeasure                                  | Status                  |
| --------------- | ---------------------------- | ------------------- | ----------------------------------------------- | ----------------------- |
| Name of problem | Ongoing count of occurrences | Short-term solution | Long-term solution based on root cause analysis | Plan - Do - Check - Act |

| 問題   | 次數      | 遏制措施   | 對策              | 狀態                |
| ---- | ------- | ------ | --------------- | ----------------- |
| 問題名稱 | 持續發生的次數 | 短期解決方案 | 基於根本原因分析的長期解決方案 | 計劃 - 執行 - 檢查 - 行動 |

Another style is more like a task board:

另一種風格更像是一個任務看板：

|Todo|In Progress|Done|
|---|---|---|
|Index cards representing raised obstacles|Obstacle cards move here when we’re actively working on them|Obstacle cards move here when we’ve resolved them|

| 待辦事項        | 進行中                  | 已完成                 |
| ----------- | -------------------- | ------------------- |
| 代表提出的障礙的索引卡 | 當我們積極處理障礙卡時，障礙卡會移到這裡 | 當我們解決了障礙卡時，障礙卡會移到這裡 |

_But_

The **Improvement Board** risks devolving into a whinging board if too many obstacles are raised on it that the team has no influence on resolving.

如果提出了太多團隊無法影響解決的障礙，**改善看板** 有演變成抱怨看板的風險。

## What order do we talk in?

### Last Arrival Speaks First 最後到達者先說

During a stand-up, attendees need to know who is supposed to speak first. Having the facilitator decide who should speak first is a subtle though definite force against self-organisation. The team should know without intervention who speaks first.

在站立會議期間，參與者需要知道誰應該先說話。 讓主持人決定誰應該先說話是一種微妙但明確地反對自組織的力量。 團隊應該在沒有干預的情況下知道誰先說話。

_Therefore_

Agree that the **Last Arrival Speaks First**. This is a simple rule that also has the added benefit of encouraging people to be punctual about showing up for the stand-up.

同意**最後到達者先說**。 這是一個簡單的規則，同時也具有鼓勵人們準時參加站立會議的額外好處。

_But_

The last arrival is also likely to be the person who is least prepared to start off the meeting well.

最後到達者也可能是最沒有準備好良好開始會議的人。

### Round Robin

During a stand-up, attendees need to know who is supposed to speak next. Having a facilitator decide who speaks next is a subtle though definite force against self-organisation. The team should know without intervention who speaks next.

在站立會議期間，參與者需要知道接下來誰應該說話。 讓主持人決定接下來誰說話是一種微妙但明確地反對自組織的力量。 團隊應該在沒有干預的情況下知道接下來誰說話。

_Therefore_

Use a simple predetermined rule like a **Round Robin** to determine who should go next. It doesn't matter if it is clockwise or counter-clockwise. What does matter is that the team runs the meeting, not the facilitator or manager.

使用一個簡單的預定規則，例如**輪流發言**，以確定接下來誰應該說話。 順時針或逆時針都無關緊要。 重要的是團隊運營會議，而不是主持人或經理。

### Pass the Token

With simple, predictable ordering mechanisms (e.g., **Round Robin**), it is very easy for participants to ignore other speakers until it is closer to their turn. There may be a tendency to think of other things rather than pay attention to what others are saying.

對於簡單、可預測的排序機制（例如，**輪流發言**），參與者很容易忽略其他演講者，直到輪到他們。 可能有一種傾向是思考其他事情，而不是注意其他人說什麼。

_Therefore_

Introduce an unpredictable ordering mechanism, like tossing a speaking token (e.g., a ball) to determine who should speak next. Having a speaking token also simplifies deciding who speaks first as it will be the person who happens to have retrieved the token (or the first person s/he tosses the token to).

引入一種不可預測的排序機制，例如拋擲說話令牌（例如，一個球）來確定接下來誰應該說話。 擁有說話令牌也簡化了決定誰先說話，因為它將是碰巧撿到令牌的人（或者他/她拋擲令牌給的第一個人）。

Tossing something around introduces a bit of fun to the daily stand-up ritual and thus serves as a good infection mechanism for other observing teams.

拋擲東西在每日站立儀式中引入了一點樂趣，因此可以作為其他觀察團隊的良好感染機制。

I first learned of this pattern on a project I was on with Simon Stewart. We used a small juggling ball but almost anything can be used as token. Other teams have used [rugby balls](http://edgibbs.com/2006/08/14/passing-a-rugby-ball-in-standups/) or even [plush toys](http://kanemar.wordpress.com/2006/05/13/controling-the-flow-of-daily-%20meetings-with-a-team-mascot/).

我第一次了解這種模式是在我和 Simon Stewart 一起參與的一個專案中。 我們使用了一個小雜耍球，但幾乎任何東西都可以用作令牌。 其他團隊使用[橄欖球](https://www.google.com/url?sa=E&q=http%3A%2F%2Fedgibbs.com%2F2006%2F08%2F14%2Fpassing-a-rugby-ball-in-standups%2F)，甚至[毛絨玩具](https://www.google.com/url?sa=E&q=http://kanemar.wordpress.com/2006/05/13/controling-the-flow-of-daily-%20meetings-with-a-team-mascot/)。

_But_

With larger teams, it may become difficult to remember who has already spoken. In those cases, it may be easier to stick to simpler mechanisms like **Round Robin**.

對於較大的團隊來說，可能很難記住誰已經說過了。 在這些情況下，堅持使用更簡單的機制，例如**輪流發言**，可能更容易。

Depending on the culture of the organisation or even team, tossing a ball around may also be seen as unprofessional and would create an unnecessary negative perception of the underlying ritual.

根據組織甚至團隊的文化，拋擲球也可能被視為不專業，並且會對底層儀式產生不必要的負面看法。

### Take a Card

During a stand-up, attendees need to know who is supposed to speak first and after that, who is supposed to speak next. Having a facilitator decide who should speak is a subtle though definite force against self-organisation. The team is not keen on **Pass the Token** because they typically have coffee cups in their hands.

在站立會議期間，參與者需要知道誰應該先說話，然後接下來誰應該說話。 讓主持人決定誰應該說話是一種微妙但明確地反對自組織的力量。 團隊不熱衷於 **傳遞令牌**，因為他們通常手裡拿著咖啡杯。

_Therefore_

[Have each team member **Take a Card** to determine which order to speak](http://%20web.archive.org/web/20090526072425/http://www.robbyonrails.com/articles/2006/05/29/the-%20daily-stand-up-part-2). Imagine a stack of cards, each of which has a number on it. As each team member comes to the meeting, they can select a card which then tells them what order to speak in.

[讓每位團隊成員 **拿一張卡片** 以確定說話的順序](https://www.google.com/url?sa=E&q=http://%20web.archive.org/web/20090526072425/http://www.robbyonrails.com/articles/2006/05/29/the-%20daily-stand-up-part-2)。 想像一下一疊卡片，每張卡片上都有一個數字。 當每位團隊成員來參加會議時，他們可以選擇一張卡片，然後卡片會告訴他們以什麼順序說話。

### Walk the Board

**_Also Known As:_** Walk the Wall

**又名：** 瀏覽牆

> [S]tandups keep everyone busy. [W]alking the board keeps everyone focused on the most important things.
> 
> [S]tandups 讓每個人都很忙。[W]瀏覽看板讓每個人都專注於最重要的事情。
> 
> -- Bret Pettichord via Twitter

> Another issue with the conventional format is that tasks or workstreams aren't discussed coherently; instead, each subject comes up briefly depending on the order in which team members speak. This can make it hard to tell what's really going on.
> 
> 傳統形式的另一個問題是，任務或工作流程沒有被連貫地討論； 相反，每個主題都會根據團隊成員的發言順序簡要地提出。 這可能會讓人難以判斷到底發生了什麼事。
> 
> -- [Dave Nicolette, “An alternative format for the daily stand-up”](http://web.archive.org/web/20090611160352/http://dnicolet1.tripod.com/%20agile/index.blog?entry_id=1900488)

People are more focused on being busy than actually progressing the work so you switched to a model where **Work Items Attend** rather than people, however, even with this work item focused stand-up, it is still difficult to understand what is happening when using typical ordering mechanisms like **Round Robin** or **Pass the Token**.

人們更關注忙碌而不是實際推進工作，因此你切換到 **工作項目參加** 而不是人的模型，然而，即使使用這種以工作項目為重點的站立會議，在使用典型的排序機制（例如 **輪流發言** 或 **傳遞令牌**）時，仍然很難理解正在發生的事情。

_Therefore_

**Walk the Board**, that is, structure the stand-up by walking through each work item that is displayed on your visual management board.

**瀏覽看板**，也就是說，透過瀏覽顯示在可視化管理看板上的每個工作項目來組織站立會議。

Most Agile and Lean teams will use a visual management system to expose what is being worked on. For Agile software development, this might be called a “task board”, “story wall”, or “Kanban board”. These boards will present a process that the work items will move through. Progress is typically represented by physically moving cards across the board. Ideally, vertical positioning will indicate priority.

大多數敏捷和精實團隊將使用可視化管理系統來揭示正在進行的工作。 對於敏捷軟體開發來說，這可能被稱為「任務看板」、「故事牆」或「看板」。 這些看板將呈現工作項目將經歷的流程。 進度通常透過在看板上實際移動卡片來表示。 理想情況下，垂直位置將指示優先順序。

With this board in place, the stand-up moves through each work item from end of process to start of process (e.g., right-to-left) and from highest-to-lowest priority (e.g., top-to-bottom). You may even explicitly indicate on the board what sequence should be used.

有了這個看板，站立會議會瀏覽從流程結束到流程開始（例如，從右到左）以及從最高到最低優先順序（例如，從上到下）的每個工作項目。 你甚至可以在看板上明確指出應該使用的順序。

Pawel Brodzinski proposed a [default sequence](http://brodzinski.com/2011/12/effective-standups.html):

Pawel Brodzinski 提出了一個[預設順序](https://www.google.com/url?sa=E&q=http%3A%2F%2Fbrodzinski.com%2F2011%2F12%2Feffective-standups.html)：

1. Blockers
2. Expedite or emergency items
3. Items that haven't moved since last standup (likely to be stuck)
4. Everything else by priority

5. 阻擋者
6. 加速或緊急項目
7. 自上次站立會議以來沒有移動的項目（可能卡住了）
8. 根據優先順序排列的所有其他項目

_But_

Obviously having a board is a pre-requisite which not all teams will have. In that case, a person-by-person structure is more appropriate.

顯然，擁有看板是一個先決條件，並非所有團隊都具備。 在這種情況下，以人為單位結構更合適。

**Walk the Board** has a much greater tendency to succumb to **Reporting to the Leader** if **Rotate the Facilitator** or some other pattern for self-organisation is not also applied.

如果沒有同時應用 **輪換主持人** 或其他自組織模式，**瀏覽看板** 更容易屈服於 **向領導者報告**。

## Where and when?

### Meet Where the Work Happens

> Do the meeting in the gemba, not a conference room.
> 
> 在現場（gemba）開會，而不是在會議室。
> 
> -- [Marc Graban, “Video of a Daily Huddle at Everett Clinic”](http://www.leanblog.org/2010/08/video-of-a-daily-huddle-at-everett-clinic/)

The workplace has many memory triggers about what is going on.

工作場所有很多關於正在發生的事情的記憶觸發器。

We also don’t want the daily meeting to require a lot of overhead coordinating, finding, and walking to rooms.

我們也不希望每日會議需要大量的協調、尋找和走到房間的開銷。

_Therefore_

**Meet Where the Work Happens**, not in a meeting room. If you have a “story wall” or “Kanban board”, meet in front of that.

**在工作發生的地點開會**，而不是在會議室。 如果你有一個「故事牆」或「看板」，就在它前面開會。

_But_

Other people nearby may find the noise of the meeting disruptive. This typically indicates underlying poor workspace design but must still be acknowledged.

附近的其他人可能會覺得會議的噪音具有干擾性。 這通常表示底層工作空間設計不佳，但仍必須承認。

### Same Place, Same Time

We want the team to have a sense of ownership of the stand-up. We also want interested parties to be able to drop by to observe a stand-up to avoid having to schedule yet another status meeting. This is difficult if any particular team member is allowed to force a delay or change of location of the stand-up.

我們希望團隊對站立會議有主人翁意識。 我們也希望感興趣的人能夠順道拜訪以觀察站立會議，以避免不得不安排另一個狀態會議。 如果允許任何特定的團隊成員強行延遲或更改站立會議的地點，這將很困難。

_Therefore_

Have the team agree on and run the daily stand-up at the **Same Place, Same Time**. Do not wait for stragglers, including architects and managers. The meeting is for the whole team, not for any particular individual. This is especially important if you **Use the Stand-up to Start the Day**.

讓團隊同意並在**同一地點，同一時間**執行每日站立會議。 不要等待落後者，包括架構師和經理。 會議是為整個團隊而設，而不是為任何特定個人而設。 如果你**使用站立會議開始一天**，這尤其重要。

Some stricter teams may impose a “fine” for latecomers. I tend to be wary of any kind of punishment mechanism and prefer discussion.

一些更嚴格的團隊可能會對遲到者處以「罰款」。 我往往對任何種類的懲罰機制持謹慎態度，並且更喜歡討論。

_But_

**Same Place, Same Time** is not intended to be blindly inflexible. The important thing is for the start time to be mostly consistent and rescheduling to be rare. If rescheduling is required often, it may be an indication that the start time should change. If a particular location is inconvenient for everyone to get to, it's probably an indication the location should change.

**同一地點，同一時間** 無意盲目地不靈活。 重要的是開始時間要大致一致，並且很少重新安排時間。 如果經常需要重新安排時間，這可能表示應該更改開始時間。 如果特定地點不方便所有人到達，這可能表示應該更改地點。

### Use the Stand-up to Start The Day

The daily stand-up meeting provides focus and awareness of outstanding issues. If it occurs late in the day, this focus and awareness is wasted.

每日站立會議提供對未決問題的關注和意識。 如果它在一天中較晚發生，這種關注和意識就會被浪費。

_Therefore_

**Use the Stand-up to Start the Day**. With flexible work hours, not every team member will arrive at work at the same time. A common practice with “flex-time” is to use a set of core working hours. The start time should be at the start of these core working hours. Similarly, if team members need to arrive later for personal reasons (e.g., need to drop off kids at school), the start time should be set at a time so that everyone can attend.

**使用站立會議開始一天**。 由於工作時間靈活，並非每位團隊成員都會在同一時間到達工作崗位。 「彈性時間」的常見做法是使用一組核心工作時間。 開始時間應在這些核心工作時間的開始。 同樣地，如果團隊成員由於個人原因需要稍後到達（例如，需要在學校放下孩子），則應將開始時間設置為每個人都可以參加的時間。

_But_

There may be a tendency not to work on any project-related tasks until the stand-up. If the **Stand-up Meeting Starts the Day... Late**, this slack time may be significant. To some extent, this may simply be used as an opportunity to check e-mail, fill in timesheets, etc. but it may be worth investigating removing the stand-up as a “start of day” ritual by scheduling it later in the day.

可能會有一種傾向是不在站立會議之前處理任何與專案相關的任務。 如果 **站立會議開始一天……晚了**，這種寬鬆時間可能很重要。 在某種程度上，這可能只是用作檢查電子郵件、填寫時間表等的機會，但可能值得調查透過在一天中的稍後時間安排來移除站立會議作為「一天的開始」儀式。

### Don't Use the Stand-up to Start the Day

The stand-up tends to serve as the ritual to set focus for the day, especially if you **Use The Stand-up to Start the Day**. Because of this, team members tend not to work on features until the stand-up. When the meeting is not actually held first thing, this tendency may have a significant impact on productivity.

站立會議往往充當為一天設定重點的儀式，尤其是如果你 **使用站立會議開始一天**。 因此，團隊成員往往不會在站立會議之前處理功能。 當會議實際上不是第一件事舉行時，這種傾向可能會對生產力產生重大影響。

_Therefore_

**Don’t Use the Stand-up to Start the Day**. Schedule the daily stand- up meeting far enough into the day that it will not be psychologically associated as starting the day.

**不要使用站立會議開始一天**。 將每日站立會議安排在一天中足夠遠的時間，使其不會在心理上與開始一天相關聯。

_But_

If the daily meeting doesn't start the day, then it can no longer be used as a shared ritual to set team focus at the start the day. Depending on the team, this price may not be worth the apparent increase in efficiency.

如果每日會議沒有開始一天，那麼它將無法再用作在一天開始時設定團隊重點的共享儀式。 根據團隊的不同，這種代價可能不值得表面上提高的效率。

When there are many project using stand-ups, it is possible that multiple stand-ups are occurring simultaneously. Observers interested in multiple projects may want to change stand-up times to allow them to be able attend. This is problematic since it risks the sense of ownership for the team if an observer can force a stand-up to adjust to his/her schedule. Nevertheless, this must also be a consideration when deciding when to have the daily stand-up.

當有許多專案使用站立會議時，可能會同時發生多個站立會議。 對多個專案感興趣的觀察員可能想要更改站立會議時間，以便他們能夠參加。 這是成問題的，因為如果觀察員可以強迫站立會議調整到他/她的時間表，這會冒著團隊的主人翁意識的風險。 儘管如此，在決定何時舉行每日站立會議時，也必須考慮到這一點。

## How do we keep the energy level up?

### Huddle 聚集

> The problem that I frequently see crop up is that people have a tendency to treat the Daily Stand-up as simply individual reporting. “I did this . . . I’ll do that”—then on to the next person. The more optimum approach is closer to a football huddle.
> 
> 我經常看到的浮現問題是，人們傾向於將每日站立會議視為簡單的個人報告。「我做了這個……我會做那個」—然後輪到下一個人。 更好的方法更接近足球隊的聚集。
> 
> -- [Jeff Sutherland, “The Origin of The Daily Stand-up”](https://www.linkedin.com/pulse/20140926150354-136414-the-origin-of-the-daily-stand-up)

Volume of speech affects attentiveness as well as effectiveness of communication. Physical distance changes the level of volume required to communicate well. Some people don't speak loudly and don't feel comfortable doing so.

語音的音量會影響專注力以及溝通的有效性。 物理距離會改變良好溝通所需的音量水平。 有些人說話不大聲，也不覺得這樣做舒服。

_Therefore_

The stand-up should be more of a **Huddle** than a meeting. If it's difficult to hear, bring everyone closer. Beyond allowing for a more relaxed speaking volume, being physically closer tends to cause participants to be more attentive on their own. Being able to stand physically closer is also an expression of greater trust within the team. If the stand-up is a new thing, it's usually enough to use hand gestures to wave people in and say something to the effect of “Let's bring it in”. If the size of the circle has been established for a while, consider explaining the reasons for closing the circle before trying to shrink it.

站立會議應該更像一個**擠在一起**，而不是一個會議。 如果難以聽到，讓每個人都靠近一點。 除了允許更輕鬆的說話音量之外，物理上更靠近往往會使參與者更加專注。 能夠在物理上更靠近也是團隊內部更大信任的表達。 如果站立會議是一個新事物，通常用手勢示意人們進來並說一些「讓我們更靠近一點」之類的話就足夠了。 如果圓圈的大小已經建立了一段時間，請考慮在嘗試縮小圓圈之前解釋關閉圓圈的原因。

_But_

The team must balance closeness with personal comfort zones. Even on a very trusting team, there is a point when people are just standing too close for comfort. Symptoms tend to be participants that are tense and/or fidgety.

團隊必須平衡親密度與個人舒適區。 即使在一個非常信任的團隊中，也存在人們站得太近而感到不舒服的情況。 症狀往往是參與者感到緊張和/或坐立不安。

### Stand Up

Some people are talkative and tend to wander off into **Story Telling**. Some people want to engage in **Problem Solving** immediately after hearing a problem. Meetings that take too long tend to have low-energy and participants not directly related to a long discussion will tend to be distracted.

有些人很健談，並且容易陷入**說故事**。 有些人希望在聽到問題後立即進行**問題解決**。 時間過長的會議往往能量較低，並且與長時間討論沒有直接關係的參與者往往會分心。

_Therefore_

Have all attendees **Stand Up** during the meeting. Use standing up to link physical with mental readiness. Physical discomfort will also remind attendees when a meeting is taking too long. A simple way to encourage this is to simply hold the meeting where there are no chairs.

讓所有參與者在會議期間**站起來**。 使用站起來將身體上的準備與精神上的準備聯繫起來。 身體上的不適也會提醒參與者何時會議時間過長。 一個簡單的鼓勵方法是簡單地在沒有椅子的地方舉行會議。

_But_

Standing up tends to cause meetings to shorten, but does not guarantee that they will shorten to an optimal length. People may learn to cope with the discomfort instead of taking a more appropriate response. Also if the meetings are _not_ taking too long nor wandering off-topic, standing up is an unnecessary ritual.

站起來往往會縮短會議時間，但不能保證它們會縮短到最佳長度。 人們可能會學會應對不適，而不是採取更適當的反應。 此外，如果會議沒有花費太長時間也沒有偏離主題，站起來是一個不必要的儀式。

### Fifteen Minutes or Less

Most people will wander mentally when they are in long meetings. A long, droning meeting is a horrible, energy-draining way to start the day. A specific number helps remind us when to consider adjustment to reduce the time of the meeting.

大多數人在長時間的會議中都會精神渙散。 一個漫長、沉悶的會議是一種可怕的、耗費能量的方式來開始一天。 一個特定的數字有助於提醒我們何時考慮調整以減少會議時間。

_Therefore_

Keep the daily stand-ups to **Fifteen Minutes or Less**. As a general rule, after fifteen minutes, the average person's mind is going to wander which doesn't help with setting focus.

將每日站立會議保持在**十五分鐘或更短**。 作為一般規則，在十五分鐘後，普通人的思想會開始游離，這不利於設定重點。

_But_

Fifteen minutes may even be too long for smaller teams. Because of the mind-wandering effect, even for larger teams, fifteen minutes is a good limit. Also, it is also possible to have a meeting that is too short where on ending, the attendees still have no idea what's going on nor who to talk to in order to find out.

對於較小的團隊來說，十五分鐘甚至可能太長了。 由於精神游離效應，即使對於較大的團隊，十五分鐘也是一個很好的限制。 此外，也有可能舉行一個太短的會議，結束時，參與者仍然不知道發生了什麼事，也不知道該與誰交談以找出答案。

### Signal the End

After the last person has spoken, the team may not immediately realise that the meeting is over. The gradual realisation that it's time to walk away doesn't end the meeting on a high note and may contribute to **Low Energy**.

在最後一個人發言後，團隊可能不會立即意識到會議已經結束。 逐漸意識到該離開的時候並沒有以高昂的音調結束會議，並且可能會導致 **能量低落**。

_Therefore_

**Signal the End** of the stand-up with a throwaway phrase (e.g., [“Well, enjoy your lunch everyone.”](http://edgibbs.com/2006/08/07/signaling-the-end-of-a-standup/)) or some other action.

用一個隨意的短語（例如，[“好吧，祝大家午餐愉快。”](https://www.google.com/url?sa=E&q=http%3A%2F%2Fedgibbs.com%2F2006%2F08%2F07%2Fsignaling-the-end-of-a-standup%2F)）或一些其他行動來**發出結束信號**。

### Time the Meetings

It is difficult to qualitatively judge whether a stand-up is taking too long, especially if it only gradually increases in length.

很難定性地判斷站立會議是否花費太長時間，特別是如果會議時長只是逐漸增加。

_Therefore_

**Time the Meetings** and publish the results. Most of the time, attendees just don't realise the impact of **Story Telling**, not being prepared to Take It Offline, or not preparing have on how long the meeting will take. Make it quantifiable.

**為會議計時** 並發布結果。 大部分時間，參與者只是沒有意識到 **說故事**、沒有準備好 **離線討論** 或沒有做好準備會如何影響會議時間。 使其可量化。

_But_

As with all measures, timing the meetings shouldn't be introduced unless there is an actual goal to accomplish due to a problem with energy levels. Once the goal is accomplished, the measurement should be dropped. Measuring for no particular reason leads to suspicion and metrics apathy.

與所有衡量標準一樣，除非存在因能量水平問題而需要達成的實際目標，否則不應引入會議計時。 一旦達成目標，就應取消衡量。 無特殊原因的衡量會導致懷疑和指標麻木。

Time is a proxy for energy, attention, and pace. Pay attention more to those things than the time.

時間是能量、注意力和步調的代表。 更關注這些事情，而不是時間。

### Take it Offline

Some people want to engage in **Problem Solving** immediately after hearing a problem. Meetings that take too long tend to have low-energy and participants not directly related to a long discussion will tend to be distracted. It is still important to acknowledge that further discussion will be required to solve the raised problem. Some people may find it uncomfortable to enforce the structure of the stand-up by interrupting.

有些人希望在聽到問題後立即進行**問題解決**。 時間過長的會議往往能量較低，並且與長時間討論沒有直接關係的參與者往往會分心。 承認需要進一步討論以解決提出的問題仍然很重要。 有些人可能會覺得透過打斷來執行站立會議的結構令人不舒服。

_Therefore_

Use a simple and consistent phrase like “**Take It Offline**” as a reminder that such discussions should take place outside of the daily stand-up. If the discussion was **Socialising**, nothing more is required. If the discussion was **Problem Solving**, the facilitator (and eventually just the team) should ensure that the right people are nominated or sign up to deal with the issue later.

使用一個簡單且一致的短語，例如「**離線討論**」來提醒大家，此類討論應在每日站立會議之外進行。 如果討論是 **社交**，則不需要更多。 如果討論是 **問題解決**，主持人（最終只是團隊）應確保提名或註冊適當的人員以稍後處理該問題。

Alternatively, some teams use more indirect signaling.

或者，一些團隊使用更間接的信號。

For example, Mike Cohn described one that uses [a rubber rat to indicate “we're going down a rathole”](http://www.marketplace.org/2012/02/20/life/hate-those-endless-meetings-try-standing).

例如，Mike Cohn 描述了一種使用[橡皮鼠來表示「我們正在走下水道」](https://www.google.com/url?sa=E&q=http%3A%2F%2Fwww.marketplace.org%2F2012%2F02%2F20%2Flife%2Fhate-those-endless-meetings-try-standing)。

Benjamin Mitchell described a Two Hand Rule:

Benjamin Mitchell 描述了一條兩手規則：

> ...if anyone thinks the current conversation has gone off topic, or is no longer effective, then they raise a hand. Once a second person raises a hand then that’s a sign to stop the conversation and continue with the rest of the stand up. Those speaking can continue the conversation after the stand up has finished.
> 
> ……如果有人認為當前的對話已經偏離主題，或者不再有效，他們就會舉手。 一旦第二個人舉手，那就是停止對話並繼續進行其餘站立會議的信號。 發言的人可以在站立會議結束後繼續對話。
> 
> -- [Benjamin Mitchell, “Stuck in an overlong Agile stand up? Try the two hands rule”](http://blog.benjaminm.net/2012/02/23/overlong-agile-stand-up-two-hand-rule/)

_But_

There is a difference between **Problem Solving** and a clarifying question. Information that is not understood is not useful. The extent upon which clarifying questions are allowed should vary depending on how large the team is and whether it will impact **Fifteen Minutes or Less**.

**問題解決** 和澄清問題之間存在差異。 無法理解的資訊是沒有用的。 允許澄清問題的程度應根據團隊的大小以及它是否會影響 **十五分鐘或更短** 而有所不同。

## How do we encourage autonomy? 我們如何鼓勵自主性？

### Rotate the Facilitator 輪換主持人

Team members are **Reporting to the Leader**, that is, they're only talking to the meeting facilitator instead of each other. Only the meeting facilitator is raising and addressing process issues related to the stand- up. We want the team to take ownership of the stand-up and this requires removing any dependence on a single facilitator.

團隊成員 **向領導者報告**，也就是說，他們只與會議主持人交談，而不是彼此交談。 只有會議主持人在提出和解決與站立會議相關的流程問題。 我們希望團隊擁有站立會議的所有權，這需要消除對單一主持人的任何依賴。

_Therefore_

**Rotate the Facilitator**. Rotate assignment of a role responsible for ensuring people attend the stand-up and stick to the agreed upon rules.

**輪換主持人**。 輪換分配一個負責確保人們參加站立會議並遵守約定規則的角色。

_But_

Teams that are not experienced with stand-ups benefit greatly from having a coach experienced in the process. It is more that the team should be weaned into taking greater control of the stand-up. At some point, no explicit facilitator should be required at all.

沒有站立會議經驗的團隊可以從擁有精通該流程的教練中受益匪淺。 更多的是，團隊應該逐漸地被引導到對站立會議的更多控制。 在某個時候，根本不需要明確的主持人。

### Break Eye Contact

Team members are **Reporting to the Leader**, that is, they're only talking to the meeting facilitator instead of each other. We want the team to take ownership of the stand-up and this requires removing any dependence on a single facilitator.

團隊成員 **向領導者報告**，也就是說，他們只與會議主持人交談，而不是彼此交談。 我們希望團隊擁有站立會議的所有權，這需要消除對單一主持人的任何依賴。

_Therefore_

The facilitator should **[Break Eye Contact](http://radio.javaranch.com/lasse/2006/05/07/%201147034972559.html#comment1147110635098)** as a subtle way of reminding the speaker that s/he should be addressing the team, not just one person. One way to do this is to [move around so that the current speaker can't see the facilitator](http://www.netobjectives.com/podcasts/last20060719_podcasts.mp3).

主持人應 **[打破眼神接觸](https://www.google.com/url?sa=E&q=http://radio.javaranch.com/lasse/2006/05/07/%201147034972559.html#comment1147110635098)**，以此作為提醒發言者他/她應該向團隊發言，而不僅僅是一個人的微妙方式。 一種方法是[四處走動，使當前發言者無法看到主持人](https://www.google.com/url?sa=E&q=http%3A%2F%2Fwww.netobjectives.com%2Fpodcasts%2Flast20060719_podcasts.mp3)。

## How do we know when a stand-up is going poorly? 我們如何知道站立會議是否進行得不順利？

>

> I endured regular stand-up meetings for three years. What made the meetings most painful was my boss (I'll call him Wally). His main reason for the stand-up meeting was not to increase efficiency or embrace XP as much as it was to shorten human interaction beyond anything directly related to the work product. ... For Wally, however, the stand-up meeting (like the 7 a.m. Monday meeting and the 5 p.m. Friday meeting) was a loyalty test designed to reinforce the employer- employee relationship.
> 
> 我忍受了三年的常規站立會議。 讓會議最痛苦的是我的老闆（我會叫他 Wally）。 他舉行站立會議的主要原因不是為了提高效率或擁抱 XP，而是為了縮短與工作產品沒有直接關係的人際互動。 ... 然而，對於 Wally 來說，站立會議（就像早上 7 點的星期一會議和下午 5 點的星期五會議一樣）是一種忠誠度測試，旨在加強僱主與僱員之間的關係。
> 
> -- [Phillip A. Laplante, “Stand and Deliver: Why I Hate Stand-Up Meetings”](http://queue.acm.org/detail.cfm?id=957730)

There are stand-up “smells” which are pretty good indicators that things are going wrong. It is important to note that even if you have no smells, this does not mean the stand-up is going right. It just means that it doesn't stink.

有一些站立會議的「氣味」，它們是事情進展不順利的很好指標。 重要的是要注意，即使你沒有任何氣味，這並不意味著站立會議正在順利進行。 這僅僅意味著它沒有發臭。

Most of the following smells are linked back to the previous patterns. For those that are not, the underlying issues tend to be more subtle or outside the scope of the daily stand-up, and people will have to come up with their own solutions.

以下大多數氣味都與先前的模式有關。 對於那些沒有的，底層問題往往更微妙或超出每日站立會議的範圍，人們將不得不提出自己的解決方案。

### Focused on the Runners, not the Baton 專注於跑步者，而不是接力棒

People are too focused on what they are doing but neglect to consider whether their activities are actually progressing the work. Reframe the stand-up such that the **Work Items Attend**.

人們太專注於他們正在做的事情，但忽略了考慮他們的活動是否實際推進了工作。 重新架構站立會議，以便 **工作項目參加**。

### Reporting to the Leader

Team members are facing and talking to the manager or meeting facilitator instead of to the team. This indicates that the daily stand-up is for the manager/facilitator when it is actually supposed to be for the team. There are various ways to break this dependence: **Rotate the Facilitator**, **Break Eye Contact**, change the form of **Yesterday Today Obstacles**, use **Pass the Token**, etc.

團隊成員面向並與經理或會議主持人交談，而不是面向團隊。 這表明每日站立會議是為經理/主持人而設的，而它實際上應該是為團隊而設的。 有多種方法可以打破這種依賴性：**輪換主持人**、**打破眼神接觸**、更改 **昨天 今天 障礙** 的形式，使用 **傳遞令牌** 等。

### People are Late

This is directly addressed by **Same Place, Same Time**, but as mentioned may indicate that the stand-up is being held at the wrong time or at the wrong place.

這由 **同一地點，同一時間** 直接解決，但如前所述，可能表明站立會議在錯誤的時間或錯誤的地點舉行。

There are other patterns to respond to this such as imposing a fine. However, I generally would not recommend them as they imply that the issue is about extrinsic motivation when it is much more likely to be something else.

還有其他模式可以應對這種情況，例如處以罰款。 但是，我通常不推薦它們，因為它們暗示問題是關於外在動機，而它更有可能是其他事情。

### Stand-up Meeting Starts the Day... Late

Because the stand-up is seen to start the work day, no work is done before the stand-up. Depending on how late in the morning the stand- up is, this can have a significant impact on available working hours. This leads to **Don't Use the Stand-up to Start the Day**.

因為站立會議被視為開始工作日，所以在站立會議之前沒有做任何工作。 根據早上站立會議的時間有多晚，這可能會對可用工作時間產生重大影響。 這導致 **不要使用站立會議開始一天**。

### Socialising 社交

One of the goals of the stand-up is to increase team socialisation. However, the daily stand-up is not intended for team members to “catch up” with each other on non project-related matters. It's difficult to provide examples of this since the degree to which socialising passes from team-building to distracting varies from team to team. The threshold can be detected from the behaviours of participants not directly involved in the socialisation. If their energy levels remain high, then it's probably just team-building; if their energy levels drop, then **Take It Offline** and perhaps provide another forum to act as a [Water Cooler](http://web.archive.org/web/20070103031934/http://www.easycomp.org/cgi-bin/%20OrgPatterns?TheWaterCooler).

站立會議的目標之一是增加團隊社交。 然而，每日站立會議並非旨在讓團隊成員在非專案相關事項上「趕上進度」。 由於社交從團隊建設到分散注意力的程度因團隊而異，因此很難提供此方面的示例。 可以從未直接參與社交的參與者的行為中檢測到閾值。 如果他們的能量水平保持在高位，那麼它可能只是團隊建設； 如果他們的能量水平下降，則 **離線討論**，並且可能提供另一個論壇來充當 [飲水機](https://www.google.com/url?sa=E&q=http://web.archive.org/web/20070103031934/http://www.easycomp.org/cgi-bin/%20OrgPatterns?TheWaterCooler)。

### I Can't Remember

> What did I do yesterday?... I can't remember... What am I doing today?... I dunno...
> 
> 我昨天做了什麼？... 我不記得了... 我今天在做什麼？... 我不知道...

Lack of preparation causes slower pace which causes lower energy. It also risks failing **Fifteen Minutes or Less**, which further reduces energy levels.

缺乏準備會導致速度變慢，從而導致能量降低。 它還可能無法達到 **十五分鐘或更短**，從而進一步降低能量水平。

A nice way to bypass this problem is to switch to a stand-up where **Work Items Attend** and we **Walk the Board**.

繞過這個問題的一個好方法是切換到 **工作項目參加** 並且我們 **瀏覽看板** 的站立會議。

Otherwise, this is a matter of expectation of responsibility to know the answers for **Yesterday Obstacles Today**.

否則，這是一個關於期望有責任知道 **昨天 障礙 今天** 答案的問題。

### Story Telling

Instead of providing a brief description of an issue, the participant provides enough details and context to cause others to tune out. The general rule is to identify obstacles during the stand-up and discuss the details after the stand-up. This can be summarised as “Tell the headline, not the whole story” or **Take it Offline**.

參與者不是提供問題的簡要描述，而是提供足夠的細節和背景，導致其他人失去興趣。 一般規則是在站立會議期間識別障礙，並在站立會議後討論細節。 這可以概括為「講述標題，而不是整個故事」或 **離線討論**。

### Problem Solving

> It’s a time to raise issues and surface ideas, not a time for in-depth problem-solving.
> 
> 這是一個提出問題和浮出想法的時間，而不是深入解決問題的時間。
> 
> -- [Marc Graban, “Video of a Daily Huddle at Everett Clinic”](http://www.leanblog.org/2010/08/video-of-a-daily-huddle-at-everett-clinic/)

The key to keeping the stand-ups **Fifteen Minutes or Less** is to limit the **Story Telling** and not succumb to **Problem Solving** during the meeting. **Take it Offline**.

保持站立會議 **十五分鐘或更短** 的關鍵是限制 **說故事** 並不要在會議期間屈服於 **問題解決**。 **離線討論**。

### Low Energy

Could indicate a slow-down of pace due to **Story Telling**, **Problem Solving**, etc. In which case **Take it Offline**. Could be simply a matter of team size. Could be the time of day which suggests trying the alternative of **Use the Stand-up to Start the Day** and **Don't Use the Stand-up to Start the Day**.

可能表示由於 **說故事**、**問題解決** 等導致速度減慢。 在這種情況下 **離線討論**。 可能只是團隊規模的問題。 可能是時間的問題，這表明嘗試 **使用站立會議開始一天** 和 **不要使用站立會議開始一天** 的替代方案。

### Obstacles are not Raised

**_Also Known As:_** [Travelogue](http://blog.jbrains.ca/permalink/stand-updaily-scrum-focus-on-achievement-and-commitment)

**又名：** [遊記](https://www.google.com/url?sa=E&q=http%3A%2F%2Fblog.jbrains.ca%2Fpermalink%2Fstand-updaily-scrum-focus-on-achievement-and-commitment)

There may be several reasons for obstacles not being raised. Not remembering, high pain threshold, lack of trust in raising issues (because **Obstacles are not Removed**), no convenient way of raising issues, etc. The facilitator should take care to encourage people to raise obstacles.

沒有提出障礙可能有幾個原因。 不記得、高痛苦閾值、缺乏提出問題的信任（因為 **障礙未被移除**）、沒有提出問題的便捷方式等。 主持人應注意鼓勵人們提出障礙。

Introducing an **Improvement Board** may also provide a less confronting medium. [Retrospectives](http://www.retrospectives.com/) are an effective way of discovering the underlying reason why **Obstacles are not Raised**.

引入 **改善看板** 也可能提供一個對抗性較低的媒介。[回顧](https://www.google.com/url?sa=E&q=http%3A%2F%2Fwww.retrospectives.com%2F) 是發現 **沒有提出障礙** 底層原因的有效方法。

### Obstacles are not Removed

With the exception of a blaming environment, the surest way to stop people from raising obstacles is to not remove them. To make it difficult to forget and/or ignore obstacles, track them publicly with an **Improvement Board**.

除了指責環境外，阻止人們提出障礙的最可靠方法是不要移除它們。 為了使其難以忘記和/或忽略障礙，請使用 **改善看板** 公開追蹤它們。

### Obstacles are Only Raised in the Stand-up

Stand-ups act as a safety net. At worst, an obstacle will be communicated to the greater team within one day. However, doing stand-ups is not intended to stop issues from being raised and resolved during the day. Introducing an alternative medium to raise obstacles such as an **Improvement Board** may help. If not, underlying reasons may be discovered using retrospectives.

站立會議充當安全網。 在最壞的情況下，障礙將在一天內傳達給更大的團隊。 然而，進行站立會議並非旨在阻止在一天中提出和解決問題。 引入替代媒介來提出障礙（例如 **改善看板**）可能會有所幫助。 如果沒有，可以使用回顧來發現底層原因。

## It’s really just standing up together every day

Hopefully this paper has provided some more insight into the subtle details of effective stand-up practices and also common problem indicators. It should be clear that a daily stand-up is not just standing up together every day.

希望本文能提供更多關於有效站立會議實踐的微妙細節以及常見問題指標的見解。 應該清楚的是，每日站立會議不僅僅是每天一起站立。

At the end of the day, it’s important to not be too concerned about having every pattern or even having some of the smells. Remember the problems we're trying to solve. Are people energised? Are people sharing problems and ideas? Are people focused on our objectives? Are people working together as a team? Does everyone know what’s going on?

歸根結底，重要的是不要太擔心擁有每個模式，甚至擁有某些氣味。 記住我們試圖解決的問題。 人們是否充滿活力？ 人們是否在分享問題和想法？ 人們是否專注於我們的目標？ 人們是否作為一個團隊一起工作？ 每個人都知道發生了什麼事嗎？

If you can answer those questions in the affirmative, the meeting is probably going okay. After all, it's really just standing up together every day.

如果你能肯定地回答這些問題，那麼會議可能進行得很順利。 畢竟，它真的只是每天一起站立。
