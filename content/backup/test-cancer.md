---
date: '2025-02-08T09:26:48+08:00'
title: 'Test Cancer'
tags: ["Martin Fowler"]
---

[Source](https://martinfowler.com/bliki/TestCancer.html)

As my career has turned into full-time authorship, I often worry about distancing myself from the realities of day-to-day software development. I've seen other well-known figures lose contact with reality, and I fear the same fate. My greatest source of resistance to this is Thoughtworks, which acts as a regular dose of reality to keep my feet on the ground.

隨著我的職業生涯逐漸轉向全職作者,我經常擔心自己會脫離日常軟體開發的現實。我見過其他知名人物失去與現實的聯繫,而我也擔心自己會面臨同樣的命運。讓我能抵抗這種命運的最大來源是 Thoughtworks,它定期讓我保持現實感,讓我腳踏實地。

Thoughtworks also acts as a source of ideas from the field, and I enjoy writing about useful things that my colleagues have discovered and developed. Usually these are helpful ideas, that I hope that some of my readers will be able to use. My topic today isn't such a pleasant topic. It's a problem and one that we don't have an answer for.

Thoughtworks 也成為了我從現場獲取想法的來源,我很享受寫作有關我同事們發現和開發的實用點子。通常這些都是有用的想法,我希望我的讀者能夠加以利用。不過,今天的主題並不那麼愉快。這是一個問題,而且我們還沒有找到解決辦法。

The scenario runs like this. We carry out a project for a client and hand over a shiny new piece of software. As is our habit these days, we also hand over a bevy of automated tests for this software (typically there are as many lines of code of tests as there are of functional code). These tests are usually a mix of unit tests and broader ranging functional and acceptance tests. Either way the tests act as an active description of what the software does and a bug detector to quickly find problems as we evolve the software. We treasure these tests, they are a key to our success in building software systems.

場景是這樣的:我們為客戶進行了一個項目,並交付了一個嶄新的軟體。同時,按照我們現在的習慣,我們還會交付大量的自動化測試(通常測試代碼的行數與功能代碼相當)。這些測試通常是單元測試和更廣泛的功能和驗收測試的混合。不管是哪種測試,它們都充當了軟體功能的活躍描述,並作為錯誤檢測器,能夠在我們進一步開發軟體時快速發現問題。我們非常珍視這些測試,它們是我們成功構建軟體系統的關鍵。

Some months later the happy customer calls us back to do some further work on the software, adding new features and capabilities. We come in, keen to work on a code base that may have faults - but at least are _our_ faults. Then we make an unpleasant discovery.

幾個月後,滿意的客戶再次聯繫我們,希望對軟體進行一些額外的工作,新增功能和能力。我們回來時滿懷熱情,因為我們即將接手的代碼基礎可能存在問題——但至少這些問題是我們造成的。然而,我們發現了一個令人不快的現實。

The tests no longer run.

測試不再運行了。

Sometimes the tests are excluded from the build scripts, and haven't been run in months. Sometimes the "tests" are run, but a good proportion of them are commented out. Either way our precious tests are afflicted with a nasty cancer that is time-consuming and frustrating to eradicate.

有時候,測試會被排除在構建腳本之外,已經有好幾個月沒運行了。有時候,"測試"被運行了,但其中相當一部分已被註釋掉。無論是哪種情況,我們寶貴的測試都遭受了一種難以治癒且耗時的惡性腫瘤。

We ask what happened and are told things like "we made a change and some tests broke, so we removed the tests". You can look at this as _our_ failing - we haven't managed to fully teach the client teams about the value of the tests. We need to do more to pass on that failing tests need to be investigated, not simply ignored. But whatever anyone says, we've discovered that cancer of the tests is a common disease.

我們詢問發生了什麼事,得到的回答通常是“我們做了改變,結果有些測試失敗了,所以我們刪除了這些測試”。你可以認為這是我們的失敗——我們沒能完全教會客戶團隊測試的重要性。我們需要更加努力地傳達一個信息,那就是當測試失敗時,需要進行調查,而不是簡單地忽略它們。但無論別人怎麼說,我們發現測試的癌症是一種常見的疾病。

We don't think that the fact that Test Cancer appears is a reason against writing tests. Even if a particularly virulant strain wipes them all out the day after we leave, we still got value from them while we were building the system. And tests don't always get cancer. We recently spoke to a developer who had become a convert to TDD after maintaining a system we'd handed over a few years ago. The tests made our code much easier to work with than code that other firms had added later.

我們不認為測試癌症的出現是一個不寫測試的理由。即使在我們離開的第二天,一種特別惡性的菌株消滅了所有的測試,我們在構建系統時仍然從中獲得了價值。而且,測試並不總是會得癌症。我們最近與一位開發人員交談,他在維護我們幾年前交付的系統後成為了 TDD 的信徒。他說,測試讓我們的代碼比其他公司後來添加的代碼更容易處理。
