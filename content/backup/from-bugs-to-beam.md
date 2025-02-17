---
date: '2025-02-17T13:22:58+08:00'
title: 'From Bugs to Beam'
tags: ["Gojko Adzic"]
---

[Source](https://gojko.net/2024/09/30/from-bugs-to-beam/)

**It’s never been really clear to me what’s the difference between a bug report and a feature request. Yes, of course, there are clear cases at the extremes, but there is a huge gray zone with a lot of overlap. And it recently dawned on me that clearing up this gray zone actually surfaces some very significant product opportunities.**

**我一直不太清楚 bug 回報和功能請求之間的區別。當然，在極端情況下是有明顯區別的，但有一個巨大的灰色地帶，兩者之間有很多重疊。我最近才意識到，釐清這個灰色地帶實際上會顯示出一些非常重要的產品機會。**

Both bugs and feature requests point to something missing from the product, and that something actually stands in the way of user success. Both need to be analysed, prioritised, developed and tested. Both come with a price tag. And in the words of an old-school project manager I worked with about two decades ago, the major difference is who picks up the bill. “Customers pay for feature requests, but we pay for bugs”, she said. “So make sure that from now on everything possible is a feature request”. Needless to say, that led to some awkward conversations.

無論是 bug 還是功能請求，都指出了產品中缺失的某些東西，而這些東西阻礙了用戶的成功。這兩者都需要分析、優先排序、開發和測試，並且都伴隨著成本。正如大約二十年前我與一位傳統的專案經理合作時所說的那樣，主要的區別在於誰來埋單。她說：“客戶為功能請求買單，而我們為 bug 買單。所以確保從現在起，盡可能將一切視為功能請求。”不用說，這導致了一些尷尬的對話。

Two decades later, I think that I found the missing clue in the wonderful book [Mismatch](https://amzn.to/3XIbz1S) by Kat Holmes. And cleared my thinking not just about bugs and feature requests, but also about what makes a software solution appropriate.

二十年後，我想我在 Kat Holmes 的精彩著作 [Mismatch](https://amzn.to/3XIbz1S) 中找到了缺失的線索。它不僅讓我對 bug 和功能請求有了更清晰的認識，也讓我理解了什麼樣的軟體解決方案才是合適的。

Holmes uses the concept of a [Persona Spectrum](https://www.votito.com/methods/persona-spectrum/) to show a range of people with different capabilities that might have the same needs and fit into the same user persona. For example, when considering people’s visual capabilities, there’s a whole range of disabilities people might face, from being fully or partially blind, to poor vision caused by age or medical conditions, being colour-blind and so on. But then there’s also a group of people whose vision is medically perfectly fine, but they still might experience visual impairment when using our products. Someone working in a dimly lit environment, or sitting on a beach under direct sunlight might need special design considerations that improve visual contrast and cognition. Figuring out what part of that range we want to support is crucial for avoiding unnecessary disappointments and missed expectations.

Holmes 使用 [Persona Spectrum](https://www.votito.com/methods/persona-spectrum/) 的概念來展示不同能力範圍內的群體可能有相同的需求並適合相同的用戶角色。例如，考慮到人們的視覺能力，有一整個範圍的障礙可能會影響人們，包括完全或部分失明、由於年齡或醫學條件導致的視力不佳、色盲等。但還有一群人，他們的視力在醫學上完全正常，但在使用我們的產品時可能仍會經歷視力受損。有人在光線昏暗的環境中工作，或在陽光直射下的海灘上坐著，可能需要特別的設計考量，以改善視覺對比和認知。弄清楚我們希望支持這個範圍的哪個部分，對於避免不必要的失望和未達期望是至關重要的。

That, I think, is the missing piece of the puzzle. _Expectations_. But whose expectations? Different people expect different things. To keep things simple, let’s just consider two perspectives for now. Does the product do what the people making it (stakeholders, developers, product managers, testers…) expect? Does it do what the users expect? I assume some readers will feel their blood boiling now, and jump into saying that this is an oversimplification. There’s never been a company in the history of software development where stakeholders, developers, testers and product managers all expect the same things. But that’s not the problem we’re solving now, there are plenty of good solutions for that kind of alignment. Also, of course, different groups of users expect different things, but that’s not necessarily what we’re solving now also. So consider some group of users that expect the same thing, somewhere on the persona spectrum, and some group of people who make the product and got aligned somehow, just so we can clear out the extremes.

這就是我認為缺失的那塊拼圖：_期望_。但誰的期望呢？不同的人期望不同的東西。為了簡化，我們暫時只考慮兩個角度。產品是否符合製作它的人（利益相關者、開發者、產品經理、測試人員……）的期望？它是否符合用戶的期望？我想有些讀者現在可能會感到血壓升高，認為這是過度簡化。歷史上沒有任何一個軟體開發公司能讓利益相關者、開發者、測試人員和產品經理都期望同樣的東西。但這不是我們現在要解決的問題，對於這種對齊有很多好的解決方案。同樣，當然，不同的用戶群體期望不同的東西，但這也不是我們現在要解決的問題。所以，考慮某個期望相同的用戶群體，位於角色光譜的某個地方，還有某個製作產品並以某種方式對齊的群體，這樣我們就可以消除極端情況。

If there were unit tests, acceptance tests, user tests and whatever else people could have used to evaluate the fitness, and a product feature passes all that, then it’s kind of acceptable. It matches the expectations of both groups. The other extreme is if it fails from both perspectives, then it’s clearly a bug.

如果有單元測試、驗收測試、用戶測試或其他人們可以用來評估適合性的工具，並且產品功能通過了所有這些，那麼它就基本可以接受。它符合這兩個群體的期望。另一個極端是，如果從兩個角度都失敗，那麼這顯然是個 bug。

![](https://gojko.net/assets/beam-quad-2.png)

But what’s in the remaining two quadrants? That’s usually where predatory pricing around feature requests fits in.

但剩下的兩個象限是什麼？這通常是功能請求中掠奪性定價的區域。

When the feature does what the product makers expect, but not what the users expect, bugs usually get labelled as “the user is not technical enough” or with some less kind variant that makes dubious claims about the user’s IQ. To take a cue from Kat Holmes, there’s a mismatch between the design and user capabilities. Thinking about it as a mismatch removes the need to discuss if the design is too smart or if the user is too stupid. There’s a mismatch, and we may need to deal with it. If the only person who is technical enough to understand the user interface is Data from Star Trek, the product doesn’t have a very big market.

當功能符合產品製作者的期望，但不符合用戶的期望時，bug 通常會被標記為“用戶不夠技術化”，或者用某些不太友好的變體來質疑用戶的智商。借用 Kat Holmes 的說法，這是一種設計和用戶能力之間的不匹配。將其視為不匹配可以避免討論設計是否太聰明或用戶是否太愚蠢的問題。這是一種不匹配，我們可能需要處理。如果唯一能理解用戶界面的技術人員是《星際迷航》中的 Data，這個產品的市場不會很大。

If the feature does something the user expected, but not what the product makers expected, someone is getting unplanned discounts by [ordering negative hamburgers from your sales terminal](https://mashable.com/video/mcdonalds-burgers-self-service). Rather than being too stupid, this is the zone where the user is too smart. It’s effectively an exploit.

如果功能符合用戶的期望，但不符合產品製作者的期望，那麼就有人正在通過 [從銷售終端訂購負數的漢堡](https://mashable.com/video/mcdonalds-burgers-self-service) 來獲得非預期的折扣。這不是太愚蠢，而是用戶太聰明了。這實際上是一種漏洞利用。

![](https://gojko.net/assets/beam-quad-final.png)

Instead of just considering right and wrong, there are 4 categories here: Bugs, Exploits, Acceptable and Mismatched functions. And of course, being a consultant, I can now claim that I invented a new model taken from the initial letters: BEAM.

不僅僅是考慮對錯，這裡有四個類別：Bug、漏洞、可接受和不匹配的功能。當然，作為顧問，我現在可以聲稱我從這些首字母中發明了一個新模型：BEAM。

Joking aside, the reason why I think this is a good way to think about problems is that straight bugs just tell us that we messed up. That’s a bit depressive. But exploits and mismatches are hidden product opportunities. That’s very optimistic.

開玩笑之外，我認為這是一種很好地思考問題的方式，因為直接的 bug 只是告訴我們我們搞砸了。這有點令人沮喪。但漏洞和不匹配是隱藏的產品機會。這非常樂觀。

Finding a user with a mismatch typically means that our persona definitions are too narrow, and that by considering a wider spectrum we have a significant opportunity to expand the appeal of our product to more people. Those additional users on the spectrum have the same motivations and same needs as the primary persona, and probably want almost the same product features, but the mismatch is stopping them from truly benefiting from our product. In most cases, a large part of the work required to make that person successful is already done. We just need to address the mismatches.

找到一個有不匹配的用戶通常意味著我們的角色定義太狹窄了，通過考慮更廣的範圍，我們有一個重要的機會可以將我們的產品吸引力擴展到更多人。這些光譜上的額外用戶與主要角色有相同的動機和需求，可能希望幾乎相同的產品功能，但不匹配阻止了他們真正受益於我們的產品。在大多數情況下，使這個人成功所需的大部分工作已經完成。我們只需要解決這些不匹配。

Finding a user with an exploit is a sign that someone is getting unexpected value from our product, or getting expected value but in an unexpected way. Some subgroup of people here will be truly malicious and need to be blocked, but some just figured a way to use our product in a way we didn’t expect. If someone is willing to go through the hoops to use a product in a sub-optimal way, we have a chance to help them achieve their true needs better.

找到一個有漏洞的用戶是一個信號，表明有人從我們的產品中獲得了非預期的價值，或者以非預期的方式獲得了預期的價值。在這裡的某些群體可能是惡意的，需要被阻止，但有些只是找到了使用我們產品的非預期方法。如果有人願意通過繁瑣的過程來以次優方式使用產品，我們有機會更好地幫助他們實現真正的需求。

Holmes makes a big point in her book talking about how fixing the issues for individual users with mismatches isn’t economically viable. Instead, she suggests to solve for one but expand to many. Finding a mismatch or an exploit is a great opportunity to improve our products for everyone.

Holmes 在她的書中大篇幅談到，為有不匹配的個別用戶解決問題在經濟上不可行。相反，她建議解決一個問題，但擴展到許多人。發現不匹配或漏洞是改善我們產品的絕佳機會。