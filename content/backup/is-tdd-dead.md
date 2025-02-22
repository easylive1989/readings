---
date: '2025-02-08T15:52:56+08:00'
title: 'Is Tdd Dead'
tags: ["Martin Fowler", "DHH"]
---

[Source](https://martinfowler.com/articles/is-tdd-dead/)

## Where This Came From

This conversation began as a consequence to [David’s RailsConf keynote](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html) where he expressed his unhappiness with TDD and Unit Testing in the Rails community. He shortly followed this with some blog posts, the first of which declared that [“TDD is dead”](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html).

這段對話始於 David 在 RailsConf 的主題演講中表達了他對 Rails 社區中 TDD 和單元測試的不滿。隨後不久,他發表了一些博客文章,其中第一篇宣稱「TDD 已死」。

A couple of days after this, I sent him a typo correction to a follow-on post, and he said he’d welcome my thoughts on his talk and blog post. We then had an enjoyable and thoughtful discussion for an hour on Skype. David had a similar discussion with Kent, and Kent suggested that we continue the discussion with all three of us, and make the conversation public. David tweeted the idea and got a lot of positive reactions.

在這之後的幾天裡,我給他發了一封電子郵件,糾正了他後續文章中的一個拼寫錯誤,他說他很歡迎我對他的演講和博客文章提出想法。隨後,我們在 Skype 上進行了一個小時的愉快而富有深度的討論。David 也與 Kent 進行了類似的討論,Kent 建議我們三個人一起繼續這個討論,並將對話公開。David 在推特上發布了這個想法,並獲得了大量積極反應。

We didn’t have much of a plan for this series, other than a rough notion of half a dozen 30 minute conversations. We mostly did this because we wanted to learn about each others’ experiences and points of view - we’re just narcissistic enough to think lots of other people wanted to watch us. We didn’t take questions in the early episodes, but did take a couple in the last one. As we did this we enjoyed how the discussion continued on blogs and twitter.

我們對這個系列討論沒有做太多計劃,只是粗略地想了半打 30 分鐘的對話。我們主要這麼做是因為我們想了解彼此的經驗和觀點——我們剛好有點自戀,認為許多人會想看我們的對話。在早期的集數中,我們沒有接受問題,但在最後一集我們接收了一些問題。在我們進行這些討論的過程中,我們很高興看到這些討論在博客和推特上持續進行。

We’d like to thank Thoughtworks, in particular Wesley Clock, for helping by doing much of the logistics of organizing the video setup and arranging the hangout times. After each episode I wrote up minutes of the discussion for those that preferred to read rather than watch.

我們想感謝 Thoughtworks,特別是 Wesley Clock,感謝他在組織視頻設置和安排討論時間方面提供的幫助。在每集之後,我都為那些更喜歡閱讀而非觀看的人寫了討論記錄。

## 1: TDD and Confidence

### Minutes

David opened the discussion by raising his three major issues with TDD and Unit Testing: confusion over the definition of TDD and unit testing, test-induced damage through using mocks to drive architecture, and how the red/green/refactor cycle of TDD never worked for him. I commented that to understand where TDD etc came from, it's useful to understand the history, so Kent explained the origins of TDD. He began by trying things out in Smalltalk, finding that TDD worked well for his personality.

David 開始討論時提到了他對 TDD(測試驅動開發)和單元測試的三個主要問題:對 TDD 和單元測試的定義感到困惑、使用 mock(模擬對象)來推動架構設計導致的測試引發的損害,以及 TDD 的紅/綠/重構循環對他來說從未真正有效。我提到,要理解 TDD 等等的起源,了解其歷史是有幫助的,於是 Kent 解釋了 TDD 的起源。他開始嘗試在 Smalltalk 中使用 TDD,發現 TDD 非常適合他的個性。

I commented that when we first worked together at C3, we didn't start using TDD, but ensured each programming episode delivered code and tests together. Kent said that programmers deserve to feel confident that their code works, TDD is one (not the only) way to reach that. David likes Ruby's design goal of programmer happiness, and is on board with the notion that you're not done till you have tests - but doesn't like TDD as a way to get there. He thinks people have different brains and thus like different techniques and languages, he doesn't like that TDD gets conflated with the confidence you get from self-testing code.

我還提到,我們在 C3 一起工作時,最初並沒有使用 TDD,但我們確保每次編程都能同時交付代碼和測試。Kent 說,程式員應該有信心相信他們的代碼是有效的,而 TDD 是達到這一目標的一種(但不是唯一)方式。David 喜歡 Ruby 的設計目標——程式員的快樂,他也同意「只有在有測試的情況下,你才算完成工作」的觀點,但他不喜歡使用 TDD 來達到這個目標。他認為,每個人的思維方式不同,因此喜歡的技術和語言也不同,他不喜歡 TDD 被與自測代碼的信心混為一談。

Kent talked about a recent hackathon at Facebook, about half of which he could use TDD and half wasn't suitable. In the TDDable code he found he was in an enjoyable flow, but found the other part more tricky. But in the non-TDD part he still used regression tests and short feedback loops. He has no problem mixing both styles, it's like playing both classical and jazz. TDD reminds him of how he learned mathematics at school - always needing examples.

Kent 提到他最近在 Facebook 的一次黑客馬拉松,差不多有一半的時間他可以使用 TDD,而另一半時間則不適合。在可進行 TDD 的代碼中,他發現自己處於一個愉快的工作流程中,而另一部分則更棘手。但是,即使在非 TDD 的部分,他仍然使用回歸測試和短迴路反饋。他認為混合使用這兩種風格沒什麼問題,就像彈奏古典音樂和爵士樂一樣。TDD 讓他想起了在學校學習數學的過程——總是需要範例。

David has been in situations where TDD flowed well, but most of his work isn't like that - his question is what are you willing to sacrifice to get that flow? Many people make bad trade-offs, especially with heavy mocking. Kent thinks it's about trade-offs: is it worth making intermediate results testable? He used the example of a compiler where an intermediate parse-tree makes a good test point, and is also a better design. But in response to David's question about mocks, he said he rarely uses them, he's concerned that those that do often find refactoring difficult, while he finds testing makes refactoring easier.

David 曾經遇到過 TDD 順暢運行的情況,但他的大部分工作並不是這樣的——他的問題是你願意犧牲什麼來達到這種順暢感?很多人做出了糟糕的取捨,尤其是在大量使用 mock 的情況下。Kent 認為這是關於取捨的問題:讓中間結果變得可測試是否值得?他舉了一個編譯器的例子,其中一個中間解析樹是一個很好的測試點,也是一個更好的設計。但是,在回應 David 關於 mock 的問題時,他說他很少使用它們,他擔心那些經常使用 mock 的人發現重構困難,而他認為測試使重構變得更容易。

I comment that there are two problems with terminology where different things get conflated: first that DHH's critique of TDD was based on an assumption that you had to use heavy mocking in TDD, which isn't the case; second that there is a difference between self-testing code and TDD. TDD is one way to achieve self-testing code. David said his reaction was to seeing people describe TDD in a mock-heavy style as a moral thing to do and the result was a lot of code that was poorly designed due to its desire to enable isolated unit tests.

我提到,術語中有兩個問題導致不同的事物被混淆了:首先,DHH 對 TDD 的批評是基於一個假設,即你必須在 TDD 中使用大量的 mock,而事實並非如此;其次,自測代碼和 TDD 是有區別的。TDD 是實現自測代碼的一種方式。David 說,他的反應是看到有人描述 TDD 時以大量使用 mock 為一種道德行為,結果是產生了許多由於希望進行獨立單元測試而設計不良的代碼。

I finished by playing time-cop and saying that in the next session we'll explore how TDD may lead to damage, if that is really damage, and how we can judge if it's damage.

最後,我擔任了時間監督員,並說在下一節中,我們將探討 TDD 是否真的會導致損害,如果確實如此,我們如何判斷這是否為損害。

### Further Reading
1. David's [RailsConf talk](https://www.youtube.com/watch?v=9LfmrkyP81M) that started it all off.
2. David's essays on [TDD is Dead](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html) and [test-induced design damage](http://david.heinemeierhansson.com/2014/test-induced-design-damage.html).
3. My distinction between [self-testing code](https://martinfowler.com/bliki/SelfTestingCode.html) and [TDD](https://martinfowler.com/bliki/TestDrivenDevelopment.html).
4. My approach to [defining unit tests](https://martinfowler.com/bliki/UnitTest.html)

## 2: Test-induced design damage

### Minutes

I began by opening with questions. Can TDD lead to design damage? Is the resulting damage really damage? How do we judge if a design is damaged or not? David describes [the gist he posted earlier](https://gist.github.com/dhh/4849a20d2ba89b34b201). It's an example of the kind of architecture he sees people arriving at using TDD with lots of mocks. Each layer of the application is separated, eg a controller can be tested without talking to real models, databases, or the request/response cycle. What matters to David isn't the specific example, so much as the unnecessary indirection and complexity required to make it easier to test in isolation.

我首先提出了幾個問題。TDD 會導致設計損害嗎?由此產生的損害真的算是損害嗎?我們如何判斷設計是否受損?David 描述了他早些時候發布的要點。這是一個例子,展示了他看到人們在使用大量 mock 的 TDD 達到的架構。應用程序的每一層都是分開的,例如控制器可以在不與真實模型、數據庫或請求/響應周期進行交互的情況下進行測試。對 David 來說,重要的不是具體的例子,而是為了便於隔離測試所引入的那些不必要的間接層和複雜性。

Kent said that ascribing test-induced damage to TDD was like driving a car to a bad place and blaming the car for it. The design David showed wasn't due to TDD, the real issue is that these indirections are all good tricks under some circumstances and we need to understand whether they are worth the cost or not. David disagreed, saying that once you jump on the TDD horse (or car) it encourages you to go a certain way - it leads to a monstrosity one test at a time. Kent countered that it was rather one _design decision_ at a time. TDD puts an evolutionary pressure on a design, people have different preferences for the grain-size of how much is covered by their tests.

Kent 說,將測試引發的損害歸因於 TDD,就像開車去了一個不好的地方然後怪罪於車子一樣。David 展示的設計並不是 TDD 造成的,真正的問題是這些間接層在某些情況下都是好的技巧,我們需要理解它們是否值得付出這樣的代價。David 不同意這種看法,說一旦你跳上了 TDD 這匹馬(或者車),它就會引導你走向某個方向——一步一步地導致一個怪物般的設計。Kent 反駁說,這更像是一次次的設計決策。TDD 對設計施加了一種演化壓力,人們對於測試所涵蓋的範圍有不同的偏好。

Kent asked David what kinds of thing he wanted to do with the gist that its structure made hard. ("If it's just sitting there who cares - it's when I want to change it that the design actually matters"). David replied that there's a direct correlation between the size of code and how easy it is to change it. All these indirections have to be kept in sync, something that's 10 lines of code is easier to understand and change than something that's 60 lines of code. Every layer of indirection introduces a high cost. David continued by saying that TDD's red/green/refactor flow was very addictive (Kent observed that he's the poorest drug dealer on the planet) and this addiction led people to these poor decisions. I disagreed with this, saying it wasn't due to TDD but due to a desire for isolation, the essence of a hexagonal architecture being isolation from its environment (in this case Rails).

Kent 問 David,他想在結構上做些什麼事情,這種結構讓他覺得很困難(「如果它只是靜靜地待在那裡,誰在乎呢——當我想要改變它時,設計才真正重要」)。David 回應說,代碼的大小與其更改的難易程度之間有直接的關聯。所有這些間接層必須保持同步,10 行代碼的東西比 60 行代碼的東西更容易理解和更改。每一層的間接都引入了高昂的成本。David 繼續說,TDD 的紅/綠/重構流程非常上癮(Kent 說他是這個星球上最差的毒販),而這種成癮導致人們做出了這些糟糕的決定。我不同意這種說法,說這不是由 TDD 引起的,而是由於對隔離的渴望,六邊形架構的本質就是從環境中隔離(在這種情況下是 Rails)。

David said the reason people wanted isolation was due to TDD, he'd heard various arguments for isolation, but only the testing one made sense. He thinks the idea that someone wants to turn a rails application into a command-line application is so rare it's laughable, similarly you can't just swap out an in-memory store for a call to a web service because they have different operational characteristics. These swapability pipedreams aren't the real goal - the real goal is isolated testing. Kent agreed that you can't treat in-memory and web services the same ("you may think you're decoupled, but you're really, really not") as the failure cases are different. The boundaries between elements will leak to some degree "the question is how much are we willing to spend to get how much decoupling between elements".

David 說,人們想要隔離的原因是由於 TDD,他聽過各種支持隔離的論點,但只有測試的理由是有道理的。他認為,某人想把 Rails 應用程序變成命令行應用程序的想法是如此罕見,以至於令人發笑,同樣,你不能簡單地將內存存儲與調用 Web 服務互換,因為它們有不同的操作特性。這些可替換性的空想不是目標——真正的目標是隔離測試。Kent 同意你不能把內存服務和 Web 服務當作一樣的對待(「你可能認為你是解耦的,但其實並不是」),因為失敗情況是不同的。元素之間的邊界在某種程度上會洩漏,「問題是我們願意花多少錢來獲得元素之間的解耦程度」。

Kent saw the difference between 10 lines of code and 60 as a cohesion issue. David agreed but argued that cohesion and coupling are often opposed. Higher coupling is usually worth the price to get better cohesion. Kent observed that there are other ways to eliminate external dependencies, you can also use intermediate results, this is what happens with compilers. Something that's hard to test is an indication that you need a design insight, it's often useful to get up and take a walk to find those insights that lead to better designs that are also more testable. David agreed that testing can lead to better designs, but said his experience often was also the opposite, that there wasn't a good testable design. Kent accused David of not having enough self-confidence, maybe you can't see the insight today, so you have to make progress in the meantime, but he's optimistic that he will find them eventually. David dismissed this as "faith-based TDD" - he used to feel this but got stuck in a depressing loop when he wasn't finding an ideal solution that wasn't there. Kent clarified he wasn't talking about TDD, but about software design in general, it's not about TDD it's about how to get feedback. Thinking about software design is the thing, because it pays off so big when you get a good design insight. Getting these insights isn't about your workflow, it's about things like knowing when to work and when to rest, gathering influences from other places, collaborating with other people.

Kent 認為,10 行代碼和 60 行代碼之間的差異是內聚性問題。David 同意,但他認為內聚性和耦合性通常是對立的。為了更好的內聚性,提高耦合性通常是值得的。Kent 指出,還有其他方法可以消除外部依賴,你也可以使用中間結果,這就是編譯器的工作方式。很難測試的東西表明你需要設計洞察力,這時經常有必要起來走走,以找到那些洞察力,這些洞察力會帶來更好的設計,同時也更容易測試。David 同意測試可以帶來更好的設計,但他說,他的經歷也經常是相反的,並沒有一個好的可測試設計。Kent 指責 David 缺乏自信,也許你今天看不到這些洞察力,所以你必須在此期間取得進展,但他樂觀地認為他最終會找到它們。David 對此嗤之以鼻,稱之為「基於信仰的 TDD」——他曾經有這種感覺,但當他找不到一個不存在的理想解決方案時,陷入了一個令人沮喪的循環中。Kent 澄清說,他不是在談論 TDD,而是在談論軟件設計總體上,這與 TDD 無關,重點是如何獲得反饋。思考軟件設計是關鍵,因為當你獲得一個好的設計洞察時,它會帶來巨大的回報。獲得這些洞察力與工作流程無關,而是與什麼時候工作,什麼時候休息,從其他地方收集影響,與其他人合作有關。

We finished by saying our theme next time would be to explore the trade-offs around how you seek out feedback while programming.

我們最後說,下次我們的主題將是探索編程時尋求反饋的權衡。

### Further Reading
1. [David's gist](https://gist.github.com/dhh/4849a20d2ba89b34b201) to show examples of the kind of design damage he's concerned about. The gist is based on an exploration of a hexagonal Rails architecture given in [a talk by Jim Weirich](https://www.youtube.com/watch?v=tg5RFeSfBM4).
2. David's blog post [introducing the problem of test-induced design damage](http://david.heinemeierhansson.com/2014/test-induced-design-damage.html)
3. The term "hexagonal architecture" was [originally coined by Alistair Cockburn](http://alistair.cockburn.us/Hexagonal+architecture), it's also less-known as "ports and adapters".
4. Following this hangout, I did another one with my colleague Badri Janakiraman about [the nature of a hexagonal Rails architecture and trade-offs around using it](https://martinfowler.com/articles/badri-hexagonal).

## 3: Feedback and QA

### Minutes

Kent opened by saying that decisions involving TDD were about trade-offs: "in some ideal world we would have instant, infallible feedback about our programming decisions"… "every key stroke that I make, if the code is ready to deploy, it would just instantly deploy." But that ideal is impossible at the moment so the question is how far do we back off from that. He went to enumerate several constraints in the trade-off.

Kent 開場時提到，與 TDD 相關的決策涉及權衡：「在某個理想的世界裡，我們對於編程決策能夠立刻得到完美無誤的反饋……每次我敲下一個鍵，如果代碼已經準備好部署，它就會立刻被部署。」但目前這個理想是無法實現的，所以問題是我們要在多大程度上退讓。他接著列舉了幾個在這種權衡中需要考慮的限制條件。

Frequency: how rapidly do we want our feedback ?

Fidelity: how accurate do we want the red/green signal to be?

Overhead: how much are we prepared to pay?

Lifespan: how long is this software going to be around, which is probability as well as time.

Those four are the constraints he thinks we need to compare. "We're not in this hangout to agree - my personal goal is just to understand the set of trade-offs by articulating them to people who are prepared to tear my ideas apart in a constructive way"

頻率：我們希望反饋多快？

準確性：我們希望紅/綠信號的準確度有多高？

開銷：我們願意為此支付多少代價？

壽命：這個軟體會存在多久，這涉及到時間和概率。

這四個是他認為我們需要比較的限制條件。「我們參加這次討論不是為了達成共識——我個人的目標只是通過向那些願意建設性地拆解我觀點的人闡述這些權衡來理解它們。」

I outline three things that we look to get feedback on.

我概述了我們尋求反饋的三個方面。

Is the software doing something useful for the user of the software? Sometimes tests help with this (eg payroll calculations) and sometimes not (eg html rendering).

Have I broken anything? "This is where self testing code… is such a lifesaver." I want to see every test fail at least once.

Is my code-base healthy? This so I can continue to build things quickly. This element gets more tricky when you're not sure who will take over the code.

軟體對使用者是否有用？有時測試有助於回答這個問題（如工資計算），有時則無法解決（如 HTML 渲染）。

我是否破壞了什麼？「這就是自測代碼……如此救命的原因。」我希望每個測試至少失敗一次。

我的代碼庫是否健康？這樣我才能繼續快速構建東西。當你不確定誰會接手代碼時，這一點會變得更加棘手。

David introduced the topic that TDD's success had led to a neglect of QA. Many shops took on TDD and got rid of QA, Basecamp didn't have QA until a couple of years ago. He thinks TDD got programmers to where "they got so over-confident that they felt they didn't need QA". While the old model was broken, the pendulum had swung too far: "I don't think you can work on anything of material quality and produce great software without having somebody who's not you test it." This is disappointing because he's seen how powerful it is to have a QA person come in.

David 引入了一個話題，即 TDD 的成功導致了對質量保證（QA）的忽視。許多公司採用了 TDD 並取消了 QA，Basecamp 直到幾年前才有 QA。他認為 TDD 讓程序員過於自信，以至於覺得不需要 QA。「雖然舊模式有問題，但擺動的鐘擺已經過頭了：我認為沒有外部測試的人，是不可能開發出高質量軟件的。」這讓人感到失望，因為他見識過 QA 進場時所帶來的強大力量。

The other issue is that to understand trade-offs you have to understand the costs, all the talk of TDD has been on the benefits. This neglect of costs is why people cannot comprehend that there is such a thing as test-induced damage. The trade-off continuum is true of other things. Consider the cost of reliability: going from 99% to 99.999% is exponentially more expensive than getting to 99%. We must also consider criticality. High reliability is important for space shuttles and pacemakers, but wrong for an exploratory web site. The rule of not writing a line of production code without a test doesn't fit in with trade-offs around criticality.

另一個問題是，要理解權衡，你必須理解成本，所有關於 TDD 的討論都集中在好處上。忽視成本是為什麼人們無法理解測試引發的損害的原因之一。這種權衡的連續性在其他方面也同樣存在。考慮可靠性的成本：從 99% 提升到 99.999% 的成本比達到 99% 要成倍增加。我們還必須考慮到關鍵性。高可靠性對航天飛機和心臟起搏器很重要，但對一個探索性網站來說卻是錯誤的。不能在沒有測試的情況下編寫一行生產代碼的規則與關鍵性的權衡不符。

Kent wanted to go back to the issue of QA, he considered the old relationship with QA was dysfunctional. His one piece of Facebook swag in his office is a poster that says "Nothing at Facebook is somebody else's problem" and he feels Facebook follows that remarkably well for a company its size. Facebook didn't have QA until recently and programmers live up to that responsibility. "It's a question of 'compared to what?'" Compared to having an effective QA then no-QA is worse, but no-QA is better than the old dysfunctional relationship. I added that at Thoughtworks we almost always have QA on our projects. I also feel that the big shift from the 90's is not just getting rid of the dysfunctional adversarial relationship, but also getting rid of manual scripted tests. And it's liberating that startups can operate without QA. David agreed that it was good to mindfully trade-off QA for initial speed, but some have taken programmer testing too far and don't see the value of exploratory testing. If developers think they can create high-enough quality software without QA they are wrong, your tests may be green but when it's in production users do things you don't expect.

Kent 想回到 QA 的問題上，他認為舊有的 QA 關係是功能失調的。他在辦公室裡擺放了一件 Facebook 周邊商品——一張寫著「在 Facebook 沒有誰的問題是別人的問題」的海報，他感覺 Facebook 這麼大一家公司竟然能把這點做得如此好。Facebook 直到最近才有 QA，而程序員們承擔起了這個責任。「這是個『比較而言』的問題。」與有效的 QA 相比，沒有 QA 是更糟糕的，但沒有 QA 比舊有的功能失調的關係要好。我補充說，在 Thoughtworks，我們幾乎總是在專案上有 QA。我還覺得，從 90 年代以來的重大變化不僅是擺脫了功能失調的對立關係，還擺脫了手動的腳本測試。而且，初創企業能夠在沒有 QA 的情況下運營，這令人解放。David 同意有意識地權衡 QA 與初始速度之間的取捨是好的，但有些人過度依賴程序員測試，而看不到探索性測試的價值。如果開發人員認為他們可以在沒有 QA 的情況下創建出高質量的軟件，那他們就錯了，你的測試可能是綠色的，但當它在生產環境中運行時，用戶會做一些你意想不到的事情。

David says that worst of all is when developers are not part of of customer service. Many programmers don't want to be on-call because it's drudgery, but it's also a feedback loop. Code with green tests can be a plateau that's below where you want to be. Kent considered that we should stipple a few red pixels in the green bar to remind us of these limitations . "The on-call is the feedback loop that teaches you what tests you didn't write." Facebook programmers have to go on-call, everyone complains about it, but there's no way they are going away from it. As soon as you think you don't make mistakes any more, that's a mistake, and you stop growing. Eventually "the world won't let you pretend that you're not screwing up any more." He'd rather pay the price of catching that early with a phone call at 2am.

David 說，最糟糕的是，當開發人員不參與客戶服務時。許多程序員不想待命，因為這是一項苦差事，但這也是一個反饋迴路。綠燈測試的代碼可能是低於你期望值的一個平臺。Kent 認為，我們應該在綠色欄中點綴一些紅色像素來提醒我們這些限制。「待命是反饋迴路，它告訴你哪些測試你沒有寫。」Facebook 的程序員必須待命，每個人都抱怨這一點，但他們不可能放棄這種做法。當你覺得自己不再犯錯的時候，那就是錯誤，並且你會停止成長。最終，「這個世界不會讓你假裝你不再犯錯。」他寧願以凌晨 2 點接電話的方式早點付出代價。

I finish by observing we didn't get to talk more about the costs of testing (David's second point from earlier) so propose that we look at that next time. I also mention that by chance there's an article by Mike Bland published on my site today that looks at exactly that topic.

我總結說，我們還沒來得及多談測試成本的問題（David 之前提到的第二個點），所以提議我們下次討論這個問題。我還提到，碰巧今天我網站上發表了一篇 Mike Bland 的文章，正好探討了這個話題。

## 4: Costs of Testing

### Minutes

David starts by saying "to talk about trade-offs, you really have to understand the drawbacks, because if there are no drawbacks there are no trade-offs." He continued by saying that TDD doesn't force you to do things, but it does nudge you in certain directions. The first issue he wanted to raise was over-testing. It's often said you shouldn't write a line of code without a failing test, at first this seems reasonable but it can lead to over-testing, such as where there are four lines of test code for every line of production code. This means that when you need to change behavior, you have more code to change . Kent has said 'you aren't paid to write tests, you just write enough to be confident' - so he asked if Kent and I wrote tests before every line of production code?

David 開場說道:「要談論權衡,你真的需要理解缺點,因為如果沒有缺點,就沒有權衡。」他繼續說 TDD 並不強迫你做某些事情,但它確實會推動你朝某些方向前進。他首先提出的問題是過度測試。人們經常說,不應該在沒有失敗測試的情況下寫一行代碼,這一開始似乎是合理的,但它可能會導致過度測試,比如每寫一行生產代碼就有四行測試代碼。這意味著當你需要改變行為時,你有更多的代碼需要更改。Kent 曾說過「你不是被支付來寫測試的,你只是寫足夠讓自己有信心的測試」,所以他問 Kent 和我是否在寫每行生產代碼之前都寫了測試?

Kent replied "it depends, and that's going to be the beginning to all of my answers to any question that's interesting". With JUnit they were very strict about test-first and were very happy with how it turned out - so he doesn't think you always get over-testing when you use TDD. Herb Derby came up with the notion of delta coverage - what coverage does this test provide that's unique? Tests with zero delta coverage should be deleted unless they provide some kind of communication purpose. He said he'd often write a system-y test, write some code to implement it, refactor a bit, and end up throwing away the initial test. Many people freak out at throwing away tests, but you should if they don't buy you anything. If the same thing is tested multiple ways, that's coupling, and coupling costs.

Kent 回答說:「這取決於情況,這將是我對任何有趣問題的所有回答的開端。」在使用 JUnit 的時候,他們對測試先行非常嚴格,並且對結果非常滿意——所以他並不認為使用 TDD 時會總是導致過度測試。Herb Derby 提出了增量覆蓋率的概念——這個測試提供了什麼獨特的覆蓋範圍?除非這些測試具有某種溝通目的,否則應該刪除沒有增量覆蓋率的測試。他說,他經常會寫一個系統性的測試,然後編寫一些代碼來實現它,進行一些重構,最後刪除最初的測試。許多人對刪除測試感到害怕,但如果這些測試對你沒有任何幫助,你就應該刪除它們。如果同樣的事情用多種方式進行測試,那就是耦合,而耦合是有代價的。

I said that I'm sure there is over-tested code, indeed if anyone does it would be Thoughtworks since we have a strong testing culture. It's hard to get the amount just right, sometimes you'll overshoot and sometimes undershoot. I would expect to overshoot from time to time and it's not something to worry about unless it's too large. On the test-every-line-of-code point I ask the question: "if I screw up this line of code is a test going to fail?" I sometimes deliberately comment a line out or reverse a conditional and run the tests to ensure one fails. My other mental test (from Kent) is only test things that can possibly break. I assume libraries work (unless they are really wonky). I ask if I can mess up my use of the library and how critical are the consequences of the mistake.

我說,我確信存在過度測試的代碼,事實上如果有任何人這樣做,那應該是 ThoughtWorks,因為我們有強大的測試文化。要精確掌握測試的數量是很困難的,有時候你會過頭,有時候又會不夠。我預期偶爾會過頭,這不是什麼需要擔心的問題,除非這個問題過於嚴重。在測試每一行代碼這一點上,我會問自己:「如果我搞砸了這行代碼,會有測試失敗嗎?」我有時會故意註釋掉一行代碼或反轉一個條件,然後運行測試,以確保有測試失敗。我另一個(來自 Kent 的)心理測試是只測試那些可能會出錯的東西。我假設庫的功能是正常的(除非它們真的有問題)。我會問自己是否會搞砸我對庫的使用,還有這個錯誤的後果有多嚴重。

Kent declared that the ratio of lines of test code to lines of production code was a bogus metric. A formative experience for him was watching Christopher Glaeser write a compiler, he had 4 lines of test code for every line of compiler code - but this is because compilers have lots of coupling. A simpler system would have a much smaller ratio. David said that that to detect commenting out a line of code implies 100% test coverage. Thinking about what can break is worth exploring, Rails's declarative statements don't lead to enough breakage to be worth testing, so he's comfortable with significantly less than 100% coverage.

Kent 宣稱測試代碼與生產代碼的行數比是一個虛假的指標。對他來說,一個成型的經驗是看著 Christopher Glaeser 編寫編譯器,他寫的每一行編譯器代碼都有四行測試代碼——但這是因為編譯器有很多耦合。一個簡單的系統會有更小的比率。David 說,要檢測註釋掉一行代碼意味著 100% 的測試覆蓋率。探索可能出錯的地方是值得的,Rails 的聲明式語句並不會導致足夠多的錯誤,因此他對顯著低於 100% 的覆蓋率感到滿意。

I replied that "you don't have enough tests (or good enough tests) if you can't confidently change the code," and "the sign of too much is whenever you change the code you think you expend more effort changing the tests than changing the code." You want to be in the Goldilocks zone, but that comes with experience of knowing what mistakes you and your team tend to make and which ones don't cause a problem. I said I like the "can I comment out a line of code" approach when I'm unsure of my ground, it's a starting place but as I work more in an environment I can come up with better heuristics. David felt that this tuning is different between product teams that are stable rather than consulting teams that are handing the code over to an unknown team and thus need more tests. Kent said that it's good to learn the discipline of test-first, it's like a 4WD-low gear for tricky parts of development.

我回覆道：「如果你不能自信地更改代碼，那麼你就沒有足夠的測試（或者測試質量不夠好），」以及「過度測試的一個跡象是，當你更改代碼時，發現自己花在更改測試上的時間比更改代碼的時間還多。」你想要達到一個「剛剛好」的區域，但這需要經驗，了解你和你的團隊容易犯的錯誤，以及哪些錯誤不會造成問題。我說，當我不確定時，我喜歡使用「我可以註釋掉一行代碼嗎」的方式作為起點，但隨著我在某個環境中的工作越來越多，我可以提出更好的準則。David 認為，這種調整在穩定的產品團隊與將代碼交給未知團隊的諮詢團隊之間是不同的，因此需要更多的測試。Kent 說，學習測試先行的紀律是有益的，就像在開發的棘手部分使用 4WD 低檔位一樣。

David introduced the next issue: many people used to think that documentation was more important than code. Now he's concerned that people think tests are more important than functional code. Connected with this is an under-emphasis on the refactor part of the TDD cycle. All this leads to insufficient energy to refactoring and keeping the code clear. Kent described that he just went through an episode where he threw away some production code, but keeping the tests and reimplementing it. He really likes that approach as the tests tell him if the new code is working. This leads to an interesting question: would you rather throw away the code and keep the tests or vice-versa? In different situations you'd answer that question differently.

David 接著介紹了下一個問題：許多人曾經認為文檔比代碼更重要。現在他擔心的是，人們認為測試比功能代碼更重要。與此相關的是，TDD 週期中的重構部分被低估了。所有這些都導致了在重構和保持代碼清晰方面投入不足。Kent 描述了他剛經歷的一次經驗，他扔掉了一些生產代碼，但保留了測試並重新實現了代碼。他非常喜歡這種方式，因為測試告訴他新代碼是否正常工作。這引出了一个有趣的问题：你會選擇扔掉代碼而保留測試，還是反之？在不同的情況下，你會做出不同的選擇。

I said I'd found situations where reading the tests helped me understand what the code was doing. I didn't think one was more important than the other - the whole point is the double check where there is an error if they get a mismatch. I agreed with David that I'd sometimes sensed teams making the bad move of putting more energy into the testing environment than in supporting the user, tests should be means to the end. I find I get a dopamine shot when I clarify code, but my biggest thrill is when I have to add a feature, think it will be tricky, but it turns out easy. That happens due to clean code, but there is a distance between cleaning the code and getting the dopamine shot. Kent showed a metaphor for this from Jeff Eastman, that is too tricky to describe in text. He got his rush from big design simplifications. He feels that it's easy to explain the value of a new test working, but hard to state the value of cleaning the design.

我說，我發現有時候閱讀測試有助於我理解代碼在做什麼。我不認為其中一個比另一個更重要——關鍵在於雙重檢查，如果出現不匹配，就會出現錯誤。我同意 David 的觀點，有時我感覺到團隊犯了一個錯誤，把更多的精力放在測試環境上，而不是支持用戶，測試應該是手段而不是目的。我發現當我澄清代碼時，我會得到一種多巴胺的激增，但我最大的快感是當我需要添加一個功能時，以為這會很棘手，但結果卻很容易。這是因為代碼乾淨，但在清理代碼和獲得多巴胺之間存在一段距離。Kent 用 Jeff Eastman 的一個隱喻來展示這一點，這在文字中太難描述了。他從大規模的設計簡化中獲得了快感。他覺得解釋一個新測試工作的價值很容易，但要說明清理設計的價值卻很難。

David said we often focus on things we can quantify, but you can't reduce design quality to a number - so people prioritize things that are low on the list like test speed, coverage, and ratios. These things are honey traps, and we need to be aware of their siren calls. Cucumber really gets his goat - glorification of a testing environment rather than production code. Only useful in the largely imaginary sweetspot of writing tests with non-technical stakeholders. It used to be important to sell TDD, but now it's conquered all, we need to explore its drawbacks. I disagreed that TDD was dominant, hearing many places where it's yet to gain traction.

David 說，我們經常關注那些可以量化的東西，但設計質量不能簡化為一個數字——所以人們優先考慮那些在列表中優先級較低的東西，如測試速度、覆蓋率和比率。這些東西是誘惑，我們需要注意它們的魅力。Cucumber 讓他非常生氣——這是一種對測試環境而不是生產代碼的過度推崇。它只在寫測試時與非技術性利益相關者合作的理想狀態下才有用。過去推廣 TDD 很重要，但現在 TDD 已經普及，我們需要探索它的缺點。我不同意 TDD 已經主導一切，因為在許多地方它仍然沒有獲得足夠的推廣。

### Further Reading
1. On the topic of misusing measurements, take a look at Pat Kua's article on [an appropriate use of metrics](https://martinfowler.com/articles/useOfMetrics.html).
2. I've written more on the [use and misuse of test coverage](https://martinfowler.com/bliki/TestCoverage.html).

## 5: Answering Questions

### Minutes

I begin by saying we'll structure this hangout by picking and answering some questions submitted by viewers. David starts by picking a question from Mike Harris, who asks for examples of open-source projects that have used TDD and either got test-induced design damage or used TDD well. David responds by saying there aren't good examples and this is one of the problems of debates like this. We don't have good application examples in general, because open-source contributors often work on private applications and open-source common frameworks and libraries. As a result we go into these discussions with different contexts, which often makes it look like there is more disagreement than there really is. People come together when you have real code rather than philosophical principles. The code examples we have are minor examples rather than things people are actually working on, so you have to understand people via presentations - which is why he used Jim Weirich's example for design damage. For good rails code he suggests Rails books that show standard testing approaches.

我先說明我們將通過選擇和回答觀眾提交的一些問題來結構這次的討論。David 先選擇了一個由 Mike Harris 提出的問題,他問能否舉例說明使用 TDD 的開源項目,這些項目要麼遭遇了由測試引起的設計損壞,要麼成功地使用了 TDD。David 回應說,目前沒有好的例子,這是此類辯論中的一個問題。我們普遍缺乏好的應用示例,因為開源貢獻者往往在私有應用上工作,而開源的是通用框架和庫。因此,我們在這些討論中會帶著不同的背景,這通常會讓分歧看起來比實際更多。當你有實際代碼而非哲學原則時,人們更能達成共識。我們擁有的代碼示例只是一些小例子,而不是人們實際在處理的內容,因此你需要通過演示來理解人們的想法——這也是為什麼他使用 Jim Weirich 的例子來說明設計損壞。關於好的 Rails 代碼,他建議參考展示標準測試方法的 Rails 書籍。

I comment that it takes lots of effort to understand real code. I do some some digging into our code bases but it takes a lot of time, and even then it's not the same understanding as you get by working with the team. Kent says that [JUnit](http://junit.org/) is an example of a project that used TDD strictly and turned out well. But it isn't a good example for this discussion because it has clear interfaces that make a sweet spot for TDD. We are talking here about different kinds of applications. If someone has a good example, they should write it up.

我評論說,理解實際代碼需要大量精力。我會深入挖掘我們的代碼庫,但這需要很多時間,即便如此,也無法像與團隊一起工作時那樣深入了解。Kent 說 JUnit 是一個嚴格使用 TDD 並成功的項目例子。但它並不是討論的好例子,因為它有清晰的接口,這對 TDD 來說是個甜蜜點。我們在這裡討論的是不同類型的應用程序。如果有人有好的例子,他們應該寫出來。

David says my comments illustrate that we can't treat programming as a science - we can't evaluate techniques objectively. This doesn't mean it isn't worth debating. We can't get a definitive answer, it's your job to figure out what makes sense. Kent agrees we can't replicate experiments, but says we still can look at things personally with a scientific mindset. We can try things out empirically for ourselves, but you can't get universal answers.

David 說我的評論說明我們不能把編程當作科學來看待——我們不能客觀地評估技術。這並不意味著辯論沒有意義。我們無法得到確定的答案,這是你自己需要弄清楚什麼有意義。Kent 同意我們不能複製實驗,但他說我們仍然可以用科學的心態來個人地看待問題。我們可以自己嘗試經驗性的方法,但無法得到普遍適用的答案。

Kent picks the next question (asked by Graham Lee): what could change about the way we write software to make TDD redundent or obsolete for Kent and Martin and what could change about how TDD is performed to make it useful for David? Kent replied by saying his [RIP TDD post](https://www.facebook.com/notes/kent-beck/rip-tdd/750840194948847) points out his postion (if rather sarcastically). TDD solves several problems, starting with confidence. TDD also allows him to break problems down piecemeal, tackling specific cases without having to solve the general case all at once. He's not prepared to give up on TDD just because it's hard.

Kent 選擇了下一個問題(由 Graham Lee 提出):什麼樣的變化可以使我們寫軟件的方式讓 TDD 對 Kent 和 Martin 來說變得冗餘或過時,而什麼樣的變化可以讓 TDD 更有用於 David?Kent 回答說,他的 RIP TDD 帖子中說明了他的立場(雖然有點諷刺)。TDD 解決了幾個問題,首先是信心。TDD 也讓他可以分解問題,一次處理具體情況而不必立即解決一般情況。他不準備因為 TDD 困難就放棄它。

I say that for me it's not about changes that would make TDD obsolete, rather the applicability of TDD in different contexts. I've experienced following TDD in a mechanical way with a calm "rapid unhurriedness" where I've blundered into good designs. Most of my programming these days is my website toolchain and while I do piecemeal progress, and have a good regression test suite - I don't find TDD applicable. But when I built the infodeck code, there was a lot of application behavior where TDD was effective. "Some contexts are very suitable for TDD, some contexts less so". And people bring their personality into that context.

我說對我來說,這不是關於哪些變化會讓 TDD 過時,而是關於 TDD 在不同情境下的適用性。我曾經以一種冷靜的“迅速但不慌忙”的機械方式來遵循 TDD,並且無意中設計出了好的設計。如今我大部分的編程工作是我的網站工具鏈,儘管我逐步推進,並且有一個好的回歸測試套件——但我發現 TDD 在這裡不太適用。但是當我構建 Infodeck 代碼時,存在很多應用行為,這讓 TDD 很有效。“有些情境非常適合 TDD,而有些情境則不那麼適合。”人們也會將他們的個性帶入這些情境。

David said his experience was similar. His introduction to testing was through TDD, he liked it and tried to apply it to everything but slowly realized that lots of areas in an MVC web application didn't feel good for TDD. This doesn't mean TDD isn't effective for some situations, just that those cases are a small percentage of his work. But abandoning TDD doesn't mean he wants to give up self-testing code - that's always been the value of TDD to him.

David 說他的經歷也類似。他是通過 TDD 來學習測試的,他喜歡 TDD 並嘗試將其應用到所有方面,但逐漸意識到,在 MVC Web 應用程序中的許多領域,TDD 並不適用。這並不意味著 TDD 在某些情況下無效,只是這些情況在他的工作中所佔比例很小。但放棄 TDD 並不意味著他想放棄自測代碼——這對他來說一直是 TDD 的價值所在。

I say that this is exactly how someone should take on TDD (or any technique). Try it out, overuse it, settle into a mode that works for you. Then also look a bit deeper: "tdd is gateway drug to self-testing code". Kent said he didn't mind where people end up with their workflow having done this kind of process. His experience of using TDD differs to David's. He finds there are moments in development where something is hard and he gets an idea of an object with a certain protocol that simplifies things. TDD gives him a mechanism to quickly get feedback on such an idea, trying and example usage of the API and implementing it. He also finds cases where TDD doesn't fit. He then finds command-R is a good way of getting feedback, but he's hoping for the moment when he can see the simplifying object and try it out with TDD.

我認為這正是應該採取 TDD（或任何技術）的方法：嘗試一下，過度使用它，然後定下來找到適合自己的模式。然後還要更深入地看看：「TDD 是通向自測代碼的入門藥物。」Kent 說他不介意人們在經歷這種過程後的工作流程最終會變成什麼樣子。他使用 TDD 的經驗與 David 的不同。他發現在開發過程中有時候遇到困難時，他會想到一個具有特定協議的對象來簡化事情。TDD 為他提供了一種快速獲得反饋的機制，讓他可以嘗試 API 的使用示例並實現它。他也發現有些情況下 TDD 不適合。這時他會發現按下 command-R 是獲得反饋的一個好方法，但他希望能有一個時刻看到能簡化問題的對象並嘗試用 TDD 來實現。

I pick the next question (from Tudor Pavel): how does TDD work with less experienced developers. I reply by saying that TDD forces people to do small pieces and helps them separate interface from implementation. It doesn't guarantee great results, because you can't do good design without experience. When less experienced people do TDD they typically don't refactor enough, leading to sub-optimal designs. You can't compare an inexperienced developer's output to an experienced developer's output, you have to compare it to what that indexperienced developer would have done without tdd. Although we can't measure this, it does seem to be advantageous, and creates a self-testing code base which is easier to improve later through refactoring. So TDD gives you a good start point.

接著我選擇了下一個問題（來自 Tudor Pavel）：TDD 對於經驗較少的開發人員來說效果如何。我回應說，TDD 迫使人們做小部分的工作，並幫助他們將接口與實現分開。這不保證能夠得到很好的結果，因為沒有經驗的話無法做出好的設計。當經驗不足的人做 TDD 時，他們通常不會進行足夠的重構，從而導致次優的設計。你不能將一個經驗不足的開發者的產出與有經驗的開發者的產出進行比較，而應該比較他們在沒有 TDD 的情況下會做什麼。雖然我們無法衡量這一點，但它似乎是有優勢的，並且創建了一個可以通過重構更容易改進的自測代碼庫。因此，TDD 為你提供了一個良好的起點。

David said that was the value he got from TDD. He started with TDD and found it was great training wheels, however he felt the discussion didn't move on enough since. He's skeptical when people say you must give simple, direct, bombastic advice to new people otherwise they won't do it. That shows a lack of confidence in what you're teaching. I agreed with that dislike for dogmatic statements. I get suspicious if I can't find arguments against something I'm describing. One twist, however, is that we do have to keep repeating introductory stuff for new people: some people resist repeating the basics.

David 說這正是他從 TDD 中獲得的價值。他開始時使用 TDD，發現它是很好的訓練輪，但他覺得討論沒有進一步推進。他對於當人們說你必須給新人提供簡單、直接、轟炸式的建議，否則他們不會去做的說法持懷疑態度。這表明你對自己所教的東西缺乏信心。我同意對教條式聲明的不喜歡。如果我不能找到反對某事的論據，我就會變得懷疑。然而，有一個小轉折是，我們確實需要不斷為新人重複基礎知識：有些人抵制重複基礎知識。

David thinks this is why we're having this conversation now. As people describe TDD they add their own spin to the basics, after ten years of this you end up a long way down the road from where you started, and not in a good place. You need to hit the reset button, an approach that's crude but effective. When he says TDD is dead, he's referring to this current mutation - we have to get back to first principles. Kent said his gut reaction to David's original keynote was at that level. Programmers will often do the same thing many times, make things too complicated, and stick with dysfunctional systems at work. He's happy to reboot to first principles, but doesn't want to lose the evolution of people's expectations about programming in the last ten years. You should be able to feel confident, point to progress, have productive technical collaborations. He feels he can be his whole self at work now in a way that he couldn't be when he started his career.

David 認為這就是為什麼我們現在要進行這次對話的原因。隨著人們描述 TDD，他們在基礎上添加了自己的見解，經過十年後，你最終會走到距離起點很遠的地方，而這並不是好事。你需要按下重置按鈕，這是一種粗糙但有效的方法。當他說 TDD 已死時，他指的是當前的這種變異——我們必須回到最初的原則。Kent 說他對 David 最初的主題演講的直覺反應正是在這個層面上。程序員經常會多次做同樣的事情，把事情弄得太複雜，並堅持在工作中使用功能失調的系統。他很樂意回到最初的原則，但他不希望失去人們對編程期望的進化。你應該能夠感到自信，指出進步，並進行富有成效的技術合作。他感覺自己現在能在工作中以一種他在職業生涯初期無法做到的方式完全做自己。

David thinks that several things came together at the same time : TDD, XP, and Ruby. People laughed at the notion that programming should be fun, but he wanted to continue with that notion in developing Rails. Now he thinks that the Ruby world takes that happiness for granted - these things have won - as has agile. I disagree that agile has won - the label has won, but many people say they do agile but don't really. This is typical for things like this, a process I call [semantic diffusion](https://martinfowler.com/bliki/SemanticDiffusion.html). The big win is that now we are able to do agile openly at clients. David observes this reboot problem with other things - after ten years you get lots of cruft. He used the example of Pinkberry which started with just two flavors, but then got a complicated range of flavors just like other ice-creams - "most people can't leave good ideas the fuck alone." TDD and Agile are very broad tents now. People who say they are doing agile do opposite things. Kent says that he hasn't seen the things with TDD that David has. He always applies TDD from first principles in his work.

David 認為有幾件事同時發生了：TDD、XP 和 Ruby。人們曾經嘲笑編程應該是有趣的這一概念，但他希望在開發 Rails 時繼續堅持這一概念。現在他認為 Ruby 世界把這種快樂視為理所當然——這些事情已經贏了，敏捷也贏了。我不同意敏捷已經勝利的說法——敏捷的標籤贏了，但很多人說他們在做敏捷卻並不是真的在做。這在這種情況下很典型，我稱之為**語義擴散**。最大的勝利是現在我們可以在客戶那裡公開做敏捷。David 觀察到其他事情也有這種重啟問題——十年後你會積累大量的雜亂。他舉了 Pinkberry 的例子，最初只有兩種口味，但隨後變成了一系列複雜的口味，就像其他冰淇淋一樣——「大多數人不能讓好的想法保持原樣」。TDD 和敏捷現在是非常廣泛的概念。聲稱自己在做敏捷的人會做完全相反的事情。Kent 說他並沒有在 TDD 上看到 David 所看到的問題。他總是從工作中的基本原則應用 TDD。

Kent appreciates that David has brought attention to TDD acquiring some barnacles and needs some scraping. David said he's seen similar issues with Rails. He uses Rails in its basic form and is shocked by some Rails usage he's seen. Kent remembers the first OOPSLA when XP got attention, Jim Rumbaugh said you won't recognize what happens to XP in ten years and he was right. I said that this is what success looks like, the alternative is that things don't take off. It's always hard to tell if something bad happens due to something inherent in a technique or due to misuse of a technique. All we can do is keep repeating the basics and the good lessons. David agreed: "you either die a hero or become the villan." Ruby was a reboot of good ideas about programming. Functional Programming is another reboot. These reboots are healthy. He's impressed by how long Rails and TDD have lasted. Before Rails he worked with PHP. You can write good code with PHP if you use it well, he feels that other languages do a better job of encouraging good usage. He things that using TDD well in an MVC web app is harder than writing clean PHP.

Kent 感謝 David 提出了 TDD 積累了一些附屬問題，並且需要進行一些清理。David 說他在 Rails 中也看到了類似的問題。他使用 Rails 的基本形式，對於看到的一些 Rails 使用方式感到震驚。Kent 回憶起第一次 OOPSLA 會議，當時 XP 引起了關注，Jim Rumbaugh 說你十年後不會認出 XP 的樣子，他是對的。我說這就是成功的樣子，另一種情況是這些東西沒有被廣泛使用。很難判斷某些壞事是由於技術本身固有的問題還是由於技術的誤用造成的。我們所能做的就是不斷重複基本原則和好的經驗教訓。David 同意：「你要麼英年早逝，要麼成為反派。」Ruby 是對編程中好想法的一次重啟。函數式編程是另一個重啟。這些重啟是健康的。他對 Rails 和 TDD 持續這麼久感到印象深刻。在 Rails 之前，他使用 PHP 工作。你可以用 PHP 編寫出好代碼，只要你使用得當，但他覺得其他語言在鼓勵良好使用方面做得更好。他認為在 MVC 網頁應用中良好地使用 TDD 比編寫乾淨的 PHP 更難。

Kent hasn't found that - any time he can break off a piece of a problem into a useful abstraction he can use TDD. He wants to explore other avenues to these big goals of bringing his whole self to programming. He will continue to explore this with experiments: doing it too much, not enough, finding the golidlocks zone, and understanding why. He comes out firmly contradicting David: TDD isn't dead, but is glad David set fire to it so it could come out like a phoenix.

Kent 沒有發現這種情況——只要他能將問題的一部分分解為有用的抽象，他就可以使用 TDD。他希望通過實驗來探索達到這些大目標的其他途徑：做得太多、做得不夠、找到最佳區域，並理解為什麼。他最終堅定地反駁 David：TDD 並沒有死，但很高興 David 點燃了它，讓它像鳳凰一樣重生。

David said he started this discussion because people wouldn't talk about cases where TDD wasn't effective. They weren't feeling good or confident, but were told they must use TDD. He wants to open the sphere of acceptable reactions so we can discuss when TDD is and isn't appropriate. Lots of people on the internet talk about how good TDD is, but people were afraid to say it wasn't working for them. For David the baby is self-testing code, we don't want to lose that when questioning TDD.

David 說他開始這次討論是因為人們不願意談論 TDD 不起作用的情況。他們感覺不好或不自信，但卻被告知必須使用 TDD。他想擴大可接受反應的範圍，以便我們能夠討論 TDD 何時適合、何時不適合。互聯網上有很多人談論 TDD 有多好，但有人害怕說它對自己不起作用。對於 David 來說，最重要的是自測代碼，我們在質疑 TDD 時不想失去這一點。

I concluded by saying that (as I suspected before we started) there is lots we agree on. We all value self-testing code a lot, we all agree TDD is valuable in some contexts, we might disagree on how many contexts (although it's hard to really tell). Everything still boils down to the point that if you're involved in software development you have to be thoughtful about it, you have to build up what practices work for you and your team, you can't take any technique blindly. You need to try it, use it, overuse it, and find what works for you and your team. We're not in a codified science, so we have to work with our own experience.

最後，我總結說（正如我在開始之前所懷疑的那樣），我們在很多方面都達成了一致。我們都非常重視自測代碼，我們都同意 TDD 在某些情況下是有價值的，我們可能在適用的情況上有不同意見（儘管很難真正分辨）。最終一切都歸結到一點，如果你從事軟件開發工作，你必須對此進行深思熟慮，你必須建立適合你和你的團隊的實踐，你不能盲目地接受任何技術。你需要嘗試、使用、過度使用，然後找到適合你和你的團隊的方法。我們不是在進行一門成文的科學，因此我們必須依靠自己的經驗來工作。