---
date: '2025-03-13T23:29:32+08:00'
title: 'First-Class Tests'
tags: ["Uncle Bob"]
---
[Source](https://blog.cleancoder.com/uncle-bob/2017/05/05/TestDefinitions.html)

I believe it may be my fate to find blogs written by people who have fallen prey to unfortunate disciplines that have led them to give up on unit testing. This blog is just another one of those.

我相信，我的命運可能就是不斷遇到那些因為不幸的學科而放棄單元測試的人所撰寫的部落格。這篇部落格也只是其中之一。

The author tells of how his unit tests are fragile because he mocks out all the collaborators. (sigh). Every time a collaborator changes, the mocks have to be changed. And this, of course, makes the unit tests fragile.

作者提到他的單元測試非常脆弱，因為他模擬了所有的協作者（嘆氣）。每次協作者發生變化，就必須修改模擬。這當然會讓單元測試變得脆弱。

The author goes on to tell us how he abandoned unit testing, and started doing, what he called “System Testing” instead. In his vocabluary, “System Tests” were simply tests that are more end-to-end than “unit tests”.

作者接著告訴我們他是如何放棄單元測試，並開始做他所謂的「系統測試」來取代。在他的詞彙中，「系統測試」只是比「單元測試」更端對端的測試。

So first, some definitions. Pardon me for my hubris, but there are so many different definitions of “unit test” and “system test” and “acceptance test” out there that it seems to me someone ought to provide a single authoritative definition. I don’t know if these definitions will stick; but I hope some set of definitions does in the near future.

所以首先，先來定義一些名詞。請原諒我的狂妄，但關於「單元測試」、「系統測試」和「驗收測試」的定義實在太多了，我覺得應該有人提供一個單一的權威定義。我不知道這些定義是否會被接受；但我希望在不久的將來能有一套定義。

- Unit Test: A test written by a programmer for the purpose of ensuring that the production code does what the programmer expects it to do. (For the moment we will ignore the notion that unit tests also aid the design, etc.)

- 單元測試 (Unit Test): 程式設計師為了確保生產程式碼按照他們的預期運作而編寫的測試。（目前我們將忽略單元測試也有助於設計等概念。）

- Acceptance Test: A test written by the business for the purpose of ensuring that the production code does what the business expects it to do. The authors of these tests are business people, or technical people who represent the business. i.e. Business Analysts, and QA.

- 驗收測試 (Acceptance Test): 業務人員為了確保生產程式碼按照他們的預期運作而編寫的測試。這些測試的作者是業務人員，或代表業務的技術人員。例如：業務分析師和 QA。

- Integration Test: A test written by architects and/or technical leads for the purpose of ensuring that a sub-assembly of system components operates correctly. These are plumbing tests. They are not business rule tests. Their purpose is to confirm that the sub-assembly has been integrated and configured correctly.

- 整合測試 (Integration Test): 架構師和/或技術負責人為了確保系統元件的子組合正確運作而編寫的測試。這些是管道測試。它們不是業務規則測試。它們的目的是確認子組合已正確整合和配置。

- System Test: An integration test written for the purpose of ensuring that the internals of the whole integrated system operate as planned. These are plumbing tests. They are not business rule tests. Their purpose is to confirm that the system has been integrated and configured correctly.

- 系統測試 (System Test): 為確保整個整合系統的內部運作按計畫進行而編寫的整合測試。這些是管道測試。它們不是業務規則測試。它們的目的是確認系統已正確整合和配置。

- Micro-test: A term coined by Mike Hill (@GeePawHill). A unit test written at a very small scope. The purpose is to test a single function, or small grouping of functions.

- 微型測試 (Micro-test): 由 Mike Hill (@GeePawHill) 創造的術語。在非常小的範圍內編寫的單元測試。目的是測試單個函數或小型函數組。

- Functional Test: A unit test written at a larger scope, with appropriate mocks for slow components.

- 功能測試 (Functional Test): 在較大範圍內編寫的單元測試，具有適用於慢速元件的模擬。

Given these definitions, the author of the above blog has given up on badly written micro-tests, in favor of badly written functional tests. Why badly written? Because in both cases the author describes these tests as coupled to things that they should not be coupled to. In the case of his micro-tests, he was using too many mocks, and deeply coupling his tests to the implementation of the production code. That, of course, leads to fragile tests.

根據這些定義，上述部落格的作者放棄了寫得不好的微型測試，轉而採用寫得不好的功能測試。為什麼說寫得不好？因為在這兩種情況下，作者都將這些測試描述為與它們不應該耦合的事物耦合在一起。在他的微型測試中，他使用了太多的模擬，並將他的測試與生產程式碼的實現深度耦合。這當然會導致脆弱的測試。

In the case of his functional tests, the author described them as going all the way from the UI to the Database; and made reference to the fact that they were slow. He cheered the notion that his tests could sometimes run in as little as 15 minutes. 15 minutes is much too long to wait for the kind of rapid feedback that unit tests are supposed to give us. TDDers are not in the habit of waiting for the continuous build system to find out if the tests pass.

在他的功能測試中，作者將它們描述為從 UI 一直到資料庫；並提到它們很慢。他慶幸他的測試有時可以在短短 15 分鐘內運行。 15 分鐘對於單元測試應該給我們的快速反饋來說太長了。 TDD 實踐者不會習慣等待持續構建系統來發現測試是否通過。

Skilled TDDers understand that neither micro-tests, nor functional tests, (nor Acceptance tests for that matter) should be coupled to the implementation of the system. They should (as the blog’s author urges us) be considered as part of the system and “…treated like a first-class citizen: [They] should be treated in the same way as one would treat production code.”

熟練的 TDD 實踐者明白，無論是微型測試、功能測試，還是驗收測試，都不應該與系統的實現耦合。它們應該（正如部落格的作者敦促我們的那樣）被視為系統的一部分，並且「…像對待一等公民一樣對待：[它們] 應該以與對待生產程式碼相同的方式對待。」

What the blog author does not seem to recognize is that first class citizens of the system should not be coupled. Someone who treats their tests like first class citizens will take great pains to ensure that those tests are not strongly coupled to the production code.

部落格作者似乎沒有意識到的是，系統的一等公民不應該耦合。會將測試視為一等公民的人會竭盡全力確保這些測試不會與生產程式碼強烈耦合。

Decoupling micro-tests and functional tests from the production code is not particularly difficult. It does require some skill at software design; and some knowledge of decoupling techniques. Generally, a good dose of OO design and dependency inversion, along with the judicious use of a few Design Patterns (like Facade and Strategy) are sufficient to decouple even the most pernicious of tests.

將微型測試和功能測試與生產程式碼解耦並非特別困難。它確實需要一些軟體設計技巧；以及一些解耦技術的知識。通常，良好的 OO 設計和依賴反轉，以及對一些設計模式（例如 Facade 和 Strategy）的明智使用，足以解耦最有害的測試。

Unfortunately, too many programmers think that the rules for unit tests are different – that they can be “junk code” written using any ad hoc style that they find convenient. Also, too many programmers have read the books on mocking, and have bought in to the notion that mocking tools are an intrinsic, and necessary, part of unit testing. Nothing could be further from the truth.

不幸的是，太多的程式設計師認為單元測試的規則是不同的——它們可以使用他們認為方便的任何臨時風格編寫成「垃圾程式碼」。而且，太多的程式設計師讀過關於模擬的書籍，並且相信模擬工具是單元測試的一個固有且必要的組成部分。沒有什麼比這更離譜的了。

I, for example, seldom use a mocking tool. When I need a mock (or, rather, a Test Double) I write it myself. It’s not very hard to write test doubles. My IDE helps me a lot with that. What’s more, writing the Test Double myself encourages me not to write tests with Test Doubles, unless it is really necessary. Instead of using Test Doubles, I back away a bit from micro-testing, and write tests that are a bit closer to functional tests. This too helps me to decouple the tests from the internals of the production code.

例如，我很少使用模擬工具。當我需要模擬（或者更確切地說，是測試替身）時，我會自己編寫。編寫測試替身並不難。我的 IDE 在這方面對我幫助很大。更重要的是，自己編寫測試替身會鼓勵我不要編寫帶有測試替身的測試，除非真的必要。我不使用測試替身，而是稍微遠離微型測試，編寫更接近功能測試的測試。這也有助於我將測試與生產程式碼的內部結構解耦。

The bottom line is that when people give up on unit tests, it’s usually because they haven’t followed the above author’s advice. They have not treated the tests like first-class citizens. They have not treated the tests as though they were part of the system. They have not maintained those tests to the same standards that they apply to the rest of the system. Instead, they have allowed the tests to rot, to become coupled, to become rigid, and fragile, and slow. And then, in frustration, they give up on the tests.

最重要的是，當人們放棄單元測試時，通常是因為他們沒有遵循上述作者的建議。他們沒有將測試視為一等公民。他們沒有將測試視為系統的一部分。他們沒有以與應用於系統其餘部分的相同標準來維護這些測試。相反，他們讓測試腐爛，變得耦合，變得僵化、脆弱和緩慢。然後，在沮喪中，他們放棄了測試。

Moral: Keep your tests clean. Treat them as first-class citizens of the system.

道理：保持你的測試乾淨。將它們視為系統的一等公民。