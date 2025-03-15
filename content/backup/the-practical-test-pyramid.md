---
date: '2025-03-15T22:48:10+08:00'
title: 'The Practical Test Pyramid'
tags: ["Martin Fowler"]
---

[Source](https://martinfowler.com/articles/practical-test-pyramid.html)

![](https://martinfowler.com/articles/practical-test-pyramid/teaser.png)

Production-ready software requires testing before it goes into production. As the discipline of software development matured, software testing approaches have matured too. Instead of having myriads of manual software testers, development teams have moved towards automating the biggest portion of their testing efforts. Automating their tests allows teams to know whether their software is broken in a matter of seconds and minutes instead of days and weeks.

產品級軟體在上線前需要經過測試。隨著軟體開發這門學科的成熟，軟體測試方法也日趨成熟。開發團隊已逐漸將測試工作的大部分轉向自動化，而不是依賴大量的軟體手動測試人員。自動化測試使團隊能夠在幾秒或幾分鐘內得知軟體是否損壞，而不是花費數天或數週。

The drastically shortened feedback loop fuelled by automated tests goes hand in hand with agile development practices, continuous delivery and DevOps culture. Having an effective software testing approach allows teams to move fast and with confidence.

自動化測試大幅縮短了反饋迴圈，與敏捷開發實踐、持續交付和 DevOps 文化相輔相成。擁有有效的軟體測試方法能讓團隊快速且自信地行動。

This article explores what a well-rounded test portfolio should look like to be responsive, reliable and maintainable - regardless of whether you're building a microservices architecture, mobile apps or IoT ecosystems. We'll also get into the details of building effective and readable automated tests.

本文探討一個完善的測試組合應具備哪些特點，使其具有反應性、可靠性和可維護性——無論您構建的是微服務架構、行動應用程式還是物聯網生態系統。我們還將深入探討如何建構有效且易讀的自動化測試。

## The Importance of (Test) Automation

Software has become an essential part of the world we live in. It has outgrown its early sole purpose of making businesses more efficient. Today companies try to find ways to become first-class digital companies. As users everyone of us interacts with an ever-increasing amount of software every day. The wheels of innovation are turning faster.

軟體已成為我們生活世界中不可或缺的一部分。它早已超越了使企業更有效率的早期唯一目的。如今，公司正努力尋找成為一流數位企業的方法。作為使用者，我們每個人每天都與越來越多的軟體互動。創新的齒輪正在加速轉動。

If you want to keep pace you'll have to look into ways to deliver your software faster without sacrificing its quality. **Continuous delivery**, a practice where you automatically ensure that your software can be released into production any time, can help you with that. With continuous delivery you use a **build pipeline** to automatically test your software and deploy it to your testing and production environments.

如果您想跟上步伐，就必須尋找在不犧牲品質的前提下更快交付軟體的方法。**持續交付**是一種實踐，透過它您可以自動確保您的軟體隨時可以發佈到生產環境中，它可以幫助您實現這一目標。透過持續交付，您可以使用**建置管道**自動測試您的軟體並將其部署到您的測試和生產環境中。

Building, testing and deploying an ever-increasing amount of software manually soon becomes impossible — unless you want to spend all your time with manual, repetitive work instead of delivering working software. Automating everything — from build to tests, deployment and infrastructure — is your only way forward.

手動建置、測試和部署越來越多的軟體很快就會變得不可能——除非您想將所有時間都花在手動、重複性的工作上，而不是交付可用的軟體。自動化一切——從建置到測試、部署和基礎設施——是您唯一的出路。


![](https://martinfowler.com/articles/practical-test-pyramid/buildPipeline.png)

Figure 1: Use build pipelines to automatically and reliably get your software into production 使用建置管道自動且可靠地將您的軟體投入生產

Traditionally software testing was overly manual work done by deploying your application to a test environment and then performing some black-box style testing e.g. by clicking through your user interface to see if anything's broken. Often these tests would be specified by test scripts to ensure the testers would do consistent checking.

傳統上，軟體測試過於依賴手動工作，即將您的應用程式部署到測試環境中，然後執行一些黑箱式測試，例如點擊您的使用者介面以查看是否有任何問題。通常，這些測試會由測試腳本指定，以確保測試人員執行一致的檢查。

It's obvious that testing all changes manually is time-consuming, repetitive and tedious. Repetitive is boring, boring leads to mistakes and makes you look for a different job by the end of the week.

顯然，手動測試所有變更既耗時又重複且乏味。重複很無聊，無聊會導致錯誤，並讓您在本週末之前尋找不同的工作。

Luckily there's a remedy for repetitive tasks: _automation_.

幸運的是，對於重複性任務，有一種補救方法：自動化。

Automating your repetitive tests can be a big game changer in your life as a software developer. Automate these tests and you no longer have to mindlessly follow click protocols in order to check if your software still works correctly. Automate your tests and you can change your codebase without batting an eye. If you've ever tried doing a large-scale refactoring without a proper test suite I bet you know what a terrifying experience this can be. How would you know if you accidentally broke stuff along the way? Well, you click through all your manual test cases, that's how. But let's be honest: do you really enjoy that? How about making even large-scale changes and knowing whether you broke stuff within seconds while taking a nice sip of coffee? Sounds more enjoyable if you ask me.

自動化您的重複性測試可能是您作為軟體開發人員生活中的一個重大改變。自動化這些測試，您就不再需要盲目地遵循點擊協議，以檢查您的軟體是否仍然正常運作。自動化您的測試，您就可以毫不猶豫地更改您的程式碼庫。如果您曾經嘗試在沒有適當測試套件的情況下進行大規模重構，我相信您知道這會是多麼可怕的經歷。您怎麼知道您是否在過程中意外地破壞了東西？好吧，您會點擊所有手動測試案例，就是這樣。但讓我們誠實地說：您真的喜歡這樣做嗎？如果能夠在進行大規模變更時，一邊啜飲咖啡一邊在幾秒鐘內知道您是否破壞了東西，怎麼樣？如果您問我，聽起來更令人愉快。

## The Test Pyramid

If you want to get serious about automated tests for your software there is one key concept you should know about: the **test pyramid**. Mike Cohn came up with this concept in his book _Succeeding with Agile_. It's a great visual metaphor telling you to think about different layers of testing. It also tells you how much testing to do on each layer.

如果您想認真對待軟體的自動化測試，那麼您應該了解一個關鍵概念：**測試金字塔**。 Mike Cohn 在他的著作《Succeeding with Agile》中提出了這個概念。這是一個很好的視覺隱喻，告訴您思考不同的測試層級。它還告訴您在每個層級上應該進行多少測試。


![](https://martinfowler.com/articles/practical-test-pyramid/testPyramid.png)

Figure 2: The Test Pyramid 測試金字塔

Mike Cohn's original test pyramid consists of three layers that your test suite should consist of (bottom to top):

Mike Cohn 最初的測試金字塔由三個層組成，您的測試套件應包含（從下到上）：

1.  Unit Tests 單元測試
2.  Service Tests 服務測試
3.  User Interface Tests 使用者介面測試

Unfortunately the concept of the test pyramid falls a little short if you take a closer look. Some argue that either the naming or some conceptual aspects of Mike Cohn's test pyramid are not ideal, and I have to agree. From a modern point of view the test pyramid seems overly simplistic and can therefore be misleading.

不幸的是，如果您仔細觀察，測試金字塔的概念會有些不足。有些人認為 Mike Cohn 測試金字塔的命名或某些概念方面並不理想，我必須同意。從現代的角度來看，測試金字塔似乎過於簡化，因此可能會產生誤導。

Still, due to its simplicity the essence of the test pyramid serves as a good rule of thumb when it comes to establishing your own test suite. Your best bet is to remember two things from Cohn's original test pyramid:

儘管如此，由於其簡單性，測試金字塔的本質可以作為建立您自己的測試套件時的一個良好經驗法則。您最好記住 Cohn 最初的測試金字塔中的兩件事：

1.  Write tests with different granularity 編寫具有不同粒度的測試
2.  The more high-level you get the fewer tests you should have 層級越高，您應該擁有的測試越少

Stick to the pyramid shape to come up with a healthy, fast and maintainable test suite: Write _lots_ of small and fast _unit tests_. Write _some_ more coarse-grained tests and _very few_ high-level tests that test your application from end to end. Watch out that you don't end up with a [test ice-cream cone](https://watirmelon.blog/testing-pyramids/) that will be a nightmare to maintain and takes way too long to run.

堅持金字塔形狀，以產生一個健康、快速且可維護的測試套件：編寫大量小型且快速的單元測試。編寫一些更粗粒度的測試和非常少的高階測試，這些測試會從頭到尾測試您的應用程式。請注意不要最終得到一個[測試冰淇淋甜筒](https://www.google.com/url?sa=E&q=https%3A%2F%2Fwatirmelon.blog%2Ftesting-pyramids%2F)，這將是一場維護噩夢，並且運行時間太長。

Don't become too attached to the names of the individual layers in Cohn's test pyramid. In fact they can be quite misleading: _service test_ is a term that is hard to grasp (Cohn himself talks about the observation that [a lot of developers completely ignore this layer](https://www.mountaingoatsoftware.com/blog/the-forgotten-layer-of-the-test-automation-pyramid)). In the days of single page application frameworks like react, angular, ember.js and others it becomes apparent that _UI tests_ don't have to be on the highest level of your pyramid - you're perfectly able to unit test your UI in all of these frameworks.

不要過於執著於 Cohn 測試金字塔中各個層級的名稱。事實上，它們可能會非常具有誤導性：服務測試是一個難以掌握的術語（Cohn 本人也談到了[很多開發人員完全忽略這一層](https://www.google.com/url?sa=E&q=https%3A%2F%2Fwww.mountaingoatsoftware.com%2Fblog%2Fthe-forgotten-layer-of-the-test-automation-pyramid)的觀察）。在 react、angular、ember.js 等單頁應用程式框架的時代，很明顯 UI 測試 不必位於金字塔的最高層級——您完全可以在所有這些框架中對您的 UI 進行單元測試。

Given the shortcomings of the original names it's totally okay to come up with other names for your test layers, as long as you keep it consistent within your codebase and your team's discussions.

鑑於原始名稱的缺點，為您的測試層級想出其他名稱完全沒問題，只要您在程式碼庫和團隊的討論中保持一致即可。
## Tools and Libraries We'll Look at

## The Sample Application

I've written a [simple microservice](https://github.com/hamvocke/spring-testing) including a test suite with tests for the different layers of the test pyramid.

我編寫了一個[簡單的微服務](https://www.google.com/url?sa=E&q=https%3A%2F%2Fgithub.com%2Fhamvocke%2Fspring-testing)，其中包括一個測試套件，其中包含針對測試金字塔不同層級的測試。

The sample application shows traits of a typical microservice. It provides a REST interface, talks to a database and fetches information from a third-party REST service. It's implemented in [Spring Boot](https://projects.spring.io/spring-boot/) and should be understandable even if you've never worked with Spring Boot before.

該範例應用程式展示了一個典型微服務的特徵。它提供了一個 REST 介面，與資料庫通訊，並從第三方 REST 服務取得資訊。它以 [Spring Boot](https://www.google.com/url?sa=E&q=https%3A%2F%2Fprojects.spring.io%2Fspring-boot%2F) 實作，即使您以前從未接觸過 Spring Boot，也應該能夠理解。

Make sure to check out [the code on Github](https://github.com/hamvocke/spring-testing). The readme contains instructions you need to run the application and its automated tests on your machine.

請務必查看 [Github 上的程式碼](https://www.google.com/url?sa=E&q=https%3A%2F%2Fgithub.com%2Fhamvocke%2Fspring-testing)。readme 包含您需要在您的機器上執行應用程式及其自動化測試的說明。

### Functionality

The application's functionality is simple. It provides a REST interface with three endpoints:

該應用程式的功能很簡單。它提供了一個具有三個端點的 REST 介面：

<table><tbody><tr><td>GET /hello</td><td>Returns <i>“Hello World”</i>. Always.</td></tr><tr><td>GET /hello/{lastname}</td><td>Looks up the person with the provided last name. If the person is known, returns <i>“Hello {Firstname} {Lastname}”</i>.</td></tr><tr><td>GET /weather</td><td>Returns the current weather conditions for <i>Hamburg, Germany</i>.</td></tr></tbody></table>

### High-level Structure

On a high-level the system has the following structure:

在高階層面，系統具有以下架構：

![](https://martinfowler.com/articles/practical-test-pyramid/testService.png)

Figure 3: the high level structure of our microservice system 我們的微服務系統的高階架構

Our microservice provides a REST interface that can be called via HTTP. For some endpoints the service will fetch information from a database. In other cases the service will call an external [weather API](https://darksky.net/) via HTTP to fetch and display current weather conditions.

我們的微服務提供了一個可以透過 HTTP 呼叫的 REST 介面。對於某些端點，服務將從資料庫中提取資訊。在其他情況下，服務將透過 HTTP 呼叫外部 [天氣 API](https://www.google.com/url?sa=E&q=https%3A%2F%2Fdarksky.net%2F) 以提取和顯示目前的天氣狀況。

### Internal Architecture

Internally, the Spring Service has a Spring-typical architecture:

在內部，Spring 服務具有 Spring 典型的架構：


![](https://martinfowler.com/articles/practical-test-pyramid/testArchitecture.png)

Figure 4: the internal structure of our microservice 我們的微服務的內部架構

-   `Controller` classes provide _REST_ endpoints and deal with _HTTP_ requests and responses
-   `Repository` classes interface with the _database_ and take care of writing and reading data to/from persistent storage
-   `Client` classes talk to other APIs, in our case it fetches _JSON_ via _HTTPS_ from the darksky.net weather API
-   `Domain` classes capture our [domain model](https://en.wikipedia.org/wiki/Domain_model) including the domain logic (which, to be fair, is quite trivial in our case).

- Controller 類別提供 REST 端點並處理 HTTP 請求和回應
- Repository 類別與 資料庫 介接，並負責將資料寫入和讀取到/從永久儲存裝置
- Client 類別與其他 API 通訊，在我們的例子中，它透過 HTTPS 從 darksky.net 天氣 API 提取 JSON
- Domain 類別捕捉我們的 [領域模型](https://www.google.com/url?sa=E&q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FDomain_model)，包括領域邏輯（公平地說，在我們的例子中，這非常簡單）。

Experienced Spring developers might notice that a frequently used layer is missing here: Inspired by [Domain-Driven Design](https://en.wikipedia.org/wiki/Domain-driven_design) a lot of developers build a _service layer_ consisting of _service_ classes. I decided not to include a service layer in this application. One reason is that our application is simple enough, a service layer would have been an unnecessary level of indirection. The other one is that I think people overdo it with service layers. I often encounter codebases where the entire business logic is captured within service classes. The domain model becomes merely a layer for data, not for behaviour (an [Anemic Domain Model](https://en.wikipedia.org/wiki/Anemic_domain_model)). For every non-trivial application this wastes a lot of potential to keep your code well-structured and testable and does not fully utilise the power of object orientation.

經驗豐富的 Spring 開發人員可能會注意到，這裡缺少一個經常使用的層：受到 [領域驅動設計](https://www.google.com/url?sa=E&q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FDomain-driven_design) 的啟發，許多開發人員建立一個由 服務 類別組成的 服務層。我決定不在這個應用程式中包含服務層。一個原因是我們的應用程式足夠簡單，服務層將是一個不必要的間接層級。另一個原因是，我認為人們過度使用服務層。我經常遇到程式碼庫，其中整個業務邏輯都捕捉在服務類別中。領域模型僅僅成為資料的一層，而不是行為（一個 [貧血領域模型](https://www.google.com/url?sa=E&q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FAnemic_domain_model)）。對於每個非平凡的應用程式，這都浪費了許多潛力，無法保持您的程式碼結構良好且可測試，並且沒有充分利用物件導向的力量。

Our repositories are straightforward and provide simple CRUD functionality. To keep the code simple I used [Spring Data](http://projects.spring.io/spring-data/). Spring Data gives us a simple and generic CRUD repository implementation that we can use instead of rolling our own. It also takes care of spinning up an in-memory database for our tests instead of using a real PostgreSQL database as it would in production.

我們的 repositories 很簡單，並提供簡單的 CRUD 功能。為了使程式碼簡單，我使用了 [Spring Data](https://www.google.com/url?sa=E&q=http%3A%2F%2Fprojects.spring.io%2Fspring-data%2F)。 Spring Data 為我們提供了一個簡單且通用的 CRUD repository 實作，我們可以使用它來代替我們自己的實作。它還負責為我們的測試啟動一個記憶體資料庫，而不是像在生產環境中那樣使用真正的 PostgreSQL 資料庫。

Take a look at the codebase and make yourself familiar with the internal structure. It will be useful for our next step: Testing the application!

看看程式碼庫，讓自己熟悉內部架構。這對我們的下一步很有用：測試應用程式！

## Unit tests

The foundation of your test suite will be made up of unit tests. Your unit tests make sure that a certain unit (your _subject under test_) of your codebase works as intended. Unit tests have the narrowest scope of all the tests in your test suite. The number of unit tests in your test suite will largely outnumber any other type of test.

您的測試套件的基礎將由單元測試組成。您的單元測試可確保程式碼庫的特定單元（您的 測試對象）按預期工作。單元測試在您的測試套件中具有最小的範圍。您測試套件中單元測試的數量將大大超過任何其他類型的測試。

![](https://martinfowler.com/articles/practical-test-pyramid/unitTest.png)

Figure 5: A unit test typically replaces external collaborators with test doubles 單元測試通常會使用測試替身替換外部協作者

### What's a Unit?

If you ask three different people what _“unit”_ means in the context of unit tests, you'll probably receive four different, slightly nuanced answers. To a certain extent it's a matter of your own definition and it's okay to have no canonical answer.

如果您詢問三個人，單元測試的上下文中 “單元” 的含義是什麼，您可能會收到四個不同的、稍微細微的答案。在一定程度上，這是您自己的定義問題，並且沒有標準答案是可以接受的。

If you're working in a functional language a _unit_ will most likely be a single function. Your unit tests will call a function with different parameters and ensure that it returns the expected values. In an object-oriented language a unit can range from a single method to an entire class.

如果您使用函數式語言，則 單元 很可能是一個單個函數。您的單元測試將使用不同的參數呼叫一個函數，並確保它傳回預期的值。在物件導向語言中，單元的範圍可以從單個方法到整個類別。

### Sociable and Solitary

Some argue that all collaborators (e.g. other classes that are called by your class under test) of your subject under test should be substituted with _mocks_ or _stubs_ to come up with perfect isolation and to avoid side-effects and a complicated test setup. Others argue that only collaborators that are slow or have bigger side effects (e.g. classes that access databases or make network calls) should be stubbed or mocked.

有些人認為，您的測試對象的所有協作者（例如，由您的測試類別呼叫的其他類別）都應替換為 mocks 或 stubs，以實現完美的隔離，並避免副作用和複雜的測試設定。另一些人則認為，只有緩慢或具有較大副作用的協作者（例如，存取資料庫或進行網路呼叫的類別）才應被 stub 或 mock。

[Occasionally](https://martinfowler.com/bliki/UnitTest.html) people label these two sorts of tests as **solitary unit tests** for tests that stub all collaborators and **sociable unit tests** for tests that allow talking to real collaborators (Jay Fields' [Working Effectively with Unit Tests](https://leanpub.com/wewut) coined these terms). If you have some spare time you can go down the rabbit hole and [read more about the pros and cons](https://martinfowler.com/articles/mocksArentStubs.html) of the different schools of thought.

[有時](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Fbliki%2FUnitTest.html) 人們將這兩種測試標記為 **孤立型單元測試**，用於 stub 所有協作者的測試，並標記為 **社交型單元測試**，用於允許與真實協作者通訊的測試（Jay Fields 的 [Working Effectively with Unit Tests](https://www.google.com/url?sa=E&q=https%3A%2F%2Fleanpub.com%2Fwewut) 創造了這些術語）。如果您有空閒時間，可以深入研究並[閱讀更多關於不同思想流派的優缺點](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Farticles%2FmocksArentStubs.html)。

At the end of the day it's not important to decide if you go for solitary or sociable unit tests. Writing automated tests is what's important. Personally, I find myself using both approaches all the time. If it becomes awkward to use real collaborators I will use mocks and stubs generously. If I feel like involving the real collaborator gives me more confidence in a test I'll only stub the outermost parts of my service.

歸根究柢，決定您選擇孤立型還是社交型單元測試並不重要。編寫自動化測試才是重要的。就個人而言，我發現自己一直在使用這兩種方法。如果使用真實協作者變得尷尬，我會慷慨地使用 mocks 和 stubs。如果我覺得涉及真實協作者讓我在測試中更有信心，我只會 stub 我服務的最外層部分。

### Mocking and Stubbing

Mocks and Stubs are two different kinds of [Test Doubles](https://martinfowler.com/bliki/TestDouble.html) (there are more than these two). A lot of people use the terms Mock and Stub interchangeably. I think it's good to be precise and keep their specific properties in mind. You can use test doubles to replace objects you'd use in production with an implementation that helps you with testing.

Mocks 和 Stubs 是兩種不同類型的 [測試替身](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Fbliki%2FTestDouble.html)（還有更多）。許多人可以互換使用 Mock 和 Stub 術語。我認為精確並牢記它們的特定屬性是很好的。您可以使用測試替身替換您在生產環境中使用的物件，並使用有助於測試的實作。

In plain words it means that you replace a real thing (e.g. a class, module or function) with a fake version of that thing. The fake version looks and acts like the real thing (answers to the same method calls) but answers with canned responses that you define yourself at the beginning of your unit test.

用簡單的話來說，這意味著您將真實事物（例如，類別、模組或函數）替換為該事物的假版本。假版本看起來和行為都像真實事物（回答相同的方法呼叫），但會回答您在單元測試開始時自己定義的 canned 回應。

Using test doubles is not specific to unit testing. More elaborate test doubles can be used to simulate entire parts of your system in a controlled way. However, in unit testing you're most likely to encounter a lot of mocks and stubs (depending of whether you're the sociable or solitary kind of developer), simply because lots of modern languages and libraries make it easy and comfortable to set up mocks and stubs.

使用測試替身並非單元測試所特有。更精細的測試替身可用於以受控方式模擬您系統的整個部分。但是，在單元測試中，您最有可能遇到大量的 mocks 和 stubs（取決於您是社交型還是孤立型的開發人員），僅僅因為許多現代語言和函式庫使設定 mocks 和 stubs 變得容易和舒適。

Regardless of your technology choice, there's a good chance that either your language's standard library or some popular third-party library will provide you with elegant ways to set up mocks. And even writing your own mocks from scratch is only a matter of writing a fake class/module/function with the same signature as the real one and setting up the fake in your test.

無論您選擇哪種技術，您的語言的標準函式庫或某些流行的第三方函式庫都很可能會為您提供設定 mocks 的優雅方法。甚至從頭開始編寫您自己的 mocks 也只是編寫一個具有與真實事物相同簽名的假類別/模組/函數，並在您的測試中設定假事物。

Your unit tests will run very fast. On a decent machine you can expect to run thousands of unit tests within a few minutes. Test small pieces of your codebase in isolation and avoid hitting databases, the filesystem or firing HTTP queries (by using mocks and stubs for these parts) to keep your tests fast.

您的單元測試將執行得非常快。在功能良好的機器上，您可以在幾分鐘內執行數千個單元測試。隔離測試程式碼庫的小部分，並避免使用資料庫、檔案系統或觸發 HTTP 查詢（透過為這些部分使用 mocks 和 stubs）以保持測試快速。

Once you got a hang of writing unit tests you will become more and more fluent in writing them. Stub out external collaborators, set up some input data, call your subject under test and check that the returned value is what you expected. Look into [Test-Driven Development](https://en.wikipedia.org/wiki/Test-driven_development) and let your unit tests guide your development; if applied correctly it can help you get into a great flow and come up with a good and maintainable design while automatically producing a comprehensive and fully automated test suite. Still, it's no silver bullet. Go ahead, give it a real chance and see if it feels right for you.

一旦您掌握了編寫單元測試的訣竅，您將越來越熟練地編寫它們。Stub 外部協作者，設定一些輸入資料，呼叫您的測試對象，並檢查傳回的值是否是您所期望的。研究 [測試驅動開發](https://www.google.com/url?sa=E&q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FTest-driven_development)，讓您的單元測試指導您的開發；如果應用正確，它可以幫助您進入一個很棒的流程，並提出一個良好且可維護的設計，同時自動產生一個全面且完全自動化的測試套件。儘管如此，它並非萬靈丹。繼續前進，給它一個真正的機會，看看它是否適合您。

### What to Test?

The good thing about unit tests is that you can write them for all your production code classes, regardless of their functionality or which layer in your internal structure they belong to. You can unit tests controllers just like you can unit test repositories, domain classes or file readers. Simply stick to the **one test class per production class** rule of thumb and you're off to a good start.

單元測試的好處是您可以為所有生產程式碼類別編寫它們，無論它們的功能如何或它們屬於您內部架構的哪一層。您可以單元測試控制器，就像您可以單元測試 repositories、領域類別或檔案讀取器一樣。只需堅持 **每個生產類別一個測試類別** 的經驗法則，您就可以很好地開始了。

A unit test class should at least **test the _public_ interface of the class**. Private methods can't be tested anyways since you simply can't call them from a different test class. _Protected_ or _package-private_ are accessible from a test class (given the package structure of your test class is the same as with the production class) but testing these methods could already go too far.

單元測試類別至少應該 **測試類別的 公共 介面**。無論如何，無法測試私有方法，因為您根本無法從不同的測試類別呼叫它們。受保護 或 套件私有 方法可以從測試類別存取（假設您的測試類別的套件結構與生產類別相同），但測試這些方法可能已經走得太遠了。

There's a fine line when it comes to writing unit tests: They should ensure that all your non-trivial code paths are tested (including happy path and edge cases). At the same time they shouldn't be tied to your implementation too closely.

在編寫單元測試方面存在一條細線：它們應該確保測試您所有非平凡的程式碼路徑（包括 happy path 和邊緣情況）。同時，它們不應過於緊密地與您的實作綁定。

Why's that? 為什麼？

Tests that are too close to the production code quickly become annoying. As soon as you refactor your production code (quick recap: refactoring means changing the internal structure of your code without changing the externally visible behaviour) your unit tests will break.

過於接近生產程式碼的測試很快就會變得令人惱火。只要您重構生產程式碼（快速回顧：重構意味著在不改變外部可見行為的情況下改變程式碼的內部架構），您的單元測試就會中斷。

This way you lose one big benefit of unit tests: acting as a safety net for code changes. You rather become fed up with those stupid tests failing every time you refactor, causing more work than being helpful; and whose idea was this stupid testing stuff anyways?

這樣您就失去了單元測試的一個大好處：充當程式碼變更的保護網。您寧願厭倦那些每次重構時都會失敗的愚蠢測試，導致更多的工作而不是有幫助；而且這個愚蠢的測試東西是誰的主意？

What do you do instead? Don't reflect your internal code structure within your unit tests. Test for observable behaviour instead. Think about

您該怎麼做？不要在您的單元測試中反映您的內部程式碼架構。而是測試可觀察的行為。考慮一下

if I enter values `x` and `y`, will the result be `z`?

如果我輸入值 x 和 y，結果會是 z 嗎？

instead of

而不是

if I enter `x` and `y`, will the method call class A first, then call class B and then return the result of class A plus the result of class B?

如果我輸入 x 和 y，方法會先呼叫類別 A，然後呼叫類別 B，然後傳回類別 A 的結果加上類別 B 的結果嗎？

Private methods should generally be considered an implementation detail. That's why you shouldn't even have the urge to test them.

私有方法通常應被視為實作細節。這就是為什麼您甚至不應該有測試它們的衝動。

I often hear opponents of unit testing (or TDD ) arguing that writing unit tests becomes pointless work where you have to test all your methods in order to come up with a high test coverage. They often cite scenarios where an overly eager team lead forced them to write unit tests for getters and setters and all other sorts of trivial code in order to come up with 100% test coverage.

我經常聽到單元測試（或 TDD ）的反對者爭辯說，編寫單元測試變得毫無意義，您必須測試所有方法才能獲得高測試覆蓋率。他們經常引用這樣的情境：一位過於熱心的團隊負責人強迫他們為 getters 和 setters 以及所有其他瑣碎的程式碼編寫單元測試，以便獲得 100% 的測試覆蓋率。

There's so much wrong with that. 這有很多錯誤。

Yes, you should _test the public interface_. More importantly, however, you **don't test trivial code**. Don't worry, [Kent Beck said it's ok](https://stackoverflow.com/questions/153234/how-deep-are-your-unit-tests/). You won't gain anything from testing simple _getters_ or _setters_ or other trivial implementations (e.g. without any conditional logic). Save the time, that's one more meeting you can attend, hooray!

是的，您應該 測試公共介面。但更重要的是，您 **不要測試瑣碎的程式碼**。別擔心，[Kent Beck 說沒關係](https://www.google.com/url?sa=E&q=https%3A%2F%2Fstackoverflow.com%2Fquestions%2F153234%2Fhow-deep-are-your-unit-tests%2F)。從測試簡單的 getters 或 setters 或其他瑣碎的實作（例如，沒有任何條件邏輯）中您不會獲得任何收益。節省時間，您可以參加更多會議，萬歲！

### Test Structure

A good structure for all your tests (this is not limited to unit tests) is this one:

適用於您所有測試（不限於單元測試）的一個良好結構是這樣的：

1.  Set up the test data 設定測試資料
2.  Call your method under test 呼叫您的測試方法
3.  Assert that the expected results are returned 斷言傳回預期的結果

There's a nice mnemonic to remember this structure: [_“Arrange, Act, Assert”_](https://xp123.com/articles/3a-arrange-act-assert/). Another one that you can use takes inspiration from BDD . It's the [_“given”_, _“when”_, _“then”_](https://martinfowler.com/bliki/GivenWhenThen.html) triad, where _given_ reflects the setup, _when_ the method call and _then_ the assertion part.

有一個不錯的助記符來記住這個結構：[“Arrange, Act, Assert”](https://www.google.com/url?sa=E&q=https%3A%2F%2Fxp123.com%2Farticles%2F3a-arrange-act-assert%2F)。另一個可以使用的助記符從 BDD 中獲得靈感。它是 [“given”，“when”，“then”](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Fbliki%2FGivenWhenThen.html) 三位一體，其中 given 反映設定，when 方法呼叫，而 then 斷言部分。

This pattern can be applied to other, more high-level tests as well. In every case they ensure that your tests remain easy and consistent to read. On top of that tests written with this structure in mind tend to be shorter and more expressive.

這個模式也可以應用於其他更高層級的測試。在每種情況下，它們都確保您的測試保持易於閱讀且一致。最重要的是，以這種結構編寫的測試往往更短且更具表現力。

### Implementing a Unit Test

Now that we know what to test and how to structure our unit tests we can finally see a real example.

現在我們知道要測試什麼以及如何構建我們的單元測試，我們終於可以看到一個真實的例子。

Let's take a simplified version of the `ExampleController` class:

讓我們採用 ExampleController 類別的簡化版本：

```java
@RestController
public class ExampleController {

    private final PersonRepository personRepo;

    @Autowired
    public ExampleController(final PersonRepository personRepo) {
        this.personRepo = personRepo;
    }

    @GetMapping("/hello/{lastName}")
    public String hello(@PathVariable final String lastName) {
        Optional&lt;Person&gt; foundPerson = personRepo.findByLastName(lastName);

        return foundPerson
                .map(person -&gt; String.format("Hello %s %s!",
                        person.getFirstName(),
                        person.getLastName()))
                .orElse(String.format("Who is this '%s' you're talking about?",
                        lastName));
    }
}
```

A unit test for the `hello(lastname)` method could look like this:

hello(lastname) 方法的單元測試可能如下所示：

```java
public class ExampleControllerTest {

    private ExampleController subject;

    @Mock
    private PersonRepository personRepo;

    @Before
    public void setUp() throws Exception {
        initMocks(this);
        subject = new ExampleController(personRepo);
    }

    @Test
    public void shouldReturnFullNameOfAPerson() throws Exception {
        Person peter = new Person("Peter", "Pan");
        given(personRepo.findByLastName("Pan"))
            .willReturn(Optional.of(peter));

        String greeting = subject.hello("Pan");

        assertThat(greeting, is("Hello Peter Pan!"));
    }

    @Test
    public void shouldTellIfPersonIsUnknown() throws Exception {
        given(personRepo.findByLastName(anyString()))
            .willReturn(Optional.empty());

        String greeting = subject.hello("Pan");

        assertThat(greeting, is("Who is this 'Pan' you're talking about?"));
    }
}
```

We're writing the unit tests using [JUnit](http://junit.org/), the de-facto standard testing framework for Java. We use [Mockito](http://site.mockito.org/) to replace the real `PersonRepository` class with a stub for our test. This stub allows us to define canned responses the stubbed method should return in this test. Stubbing makes our test more simple, predictable and allows us to easily setup test data.

我們使用 [JUnit](https://www.google.com/url?sa=E&q=http%3A%2F%2Fjunit.org%2F) 編寫單元測試，JUnit 是 Java 事實上的標準測試框架。我們使用 [Mockito](https://www.google.com/url?sa=E&q=http%3A%2F%2Fsite.mockito.org%2F) 將真實的 PersonRepository 類別替換為我們測試的 stub。這個 stub 允許我們定義 stub 方法在此測試中應傳回的 canned 回應。 Stubbing 使我們的測試更簡單、更可預測，並允許我們輕鬆設定測試資料。

Following the _arrange, act, assert_ structure, we write two unit tests - a positive case and a case where the searched person cannot be found. The first, positive test case creates a new person object and tells the mocked repository to return this object when it's called with _“Pan”_ as the value for the `lastName` parameter. The test then goes on to call the method that should be tested. Finally it asserts that the response is equal to the expected response.

遵循 arrange, act, assert 結構，我們編寫兩個單元測試 - 一個正面案例和一個找不到搜尋對象的案例。第一個正面測試案例建立一個新的人員物件，並告訴 mocked 的 repository 在以 “Pan” 作為 lastName 參數的值呼叫時，傳回這個物件。然後測試繼續呼叫應該測試的方法。最後，它斷言回應等於預期的回應。

The second test works similarly but tests the scenario where the tested method does not find a person for the given parameter.

第二個測試的工作方式類似，但測試了測試方法找不到給定參數的人員的情況。

## Integration Tests

All non-trivial applications will integrate with some other parts (databases, filesystems, network calls to other applications). When writing unit tests these are usually the parts you leave out in order to come up with better isolation and faster tests. Still, your application will interact with other parts and this needs to be tested. **[Integration Tests](https://martinfowler.com/bliki/IntegrationTest.html)** are there to help. They test the integration of your application with all the parts that live outside of your application.

所有非平凡的應用程式都將與其他部分整合（資料庫、檔案系統、對其他應用程式的網路呼叫）。在編寫單元測試時，通常會省略這些部分，以便實現更好的隔離和更快的測試。儘管如此，您的應用程式仍將與其他部分互動，這需要進行測試。 **[整合測試](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Fbliki%2FIntegrationTest.html)** 的作用就在於此。它們測試您的應用程式與應用程式外部的所有部分的整合。

For your automated tests this means you don't just need to run your own application but also the component you're integrating with. If you're testing the integration with a database you need to run a database when running your tests. For testing that you can read files from a disk you need to save a file to your disk and load it in your integration test.

對於您的自動化測試，這意味著您不僅需要執行自己的應用程式，還需要執行您正在整合的元件。如果您正在測試與資料庫的整合，則在執行測試時需要執行資料庫。為了測試您可以從磁碟讀取檔案，您需要將檔案儲存到磁碟上，並在整合測試中載入它。

I mentioned before that “unit tests” is a vague term, this is even more true for “integration tests”. For some people integration testing means to test through the entire stack of your application connected to other applications within your system. I like to treat integration testing more narrowly and test one integration point at a time by replacing separate services and databases with test doubles. Together with contract testing and running contract tests against test doubles as well as the real implementations you can come up with integration tests that are faster, more independent and usually easier to reason about.

我之前提到過，“單元測試”是一個模糊的術語，對於“整合測試”而言更是如此。對於某些人來說，整合測試意味著測試整個應用程式堆疊，該堆疊連接到系統中的其他應用程式。我喜歡更狹隘地處理整合測試，並透過用測試替身替換單獨的服務和資料庫來一次測試一個整合點。結合合約測試，以及針對測試替身和真實實作執行合約測試，您可以提出更快、更獨立且通常更容易推理的整合測試。

Narrow integration tests live at the boundary of your service. Conceptually they're always about triggering an action that leads to integrating with the outside part (filesystem, database, separate service). A database integration test would look like this:

狹義的整合測試位於您服務的邊界。從概念上講，它們始終是關於觸發一個導致與外部部分（檔案系統、資料庫、單獨的服務）整合的動作。資料庫整合測試如下所示：


![](https://martinfowler.com/articles/practical-test-pyramid/dbIntegrationTest.png)

Figure 6: A database integration test integrates your code with a real database 資料庫整合測試將您的程式碼與真實資料庫整合

1.  start a database 啟動資料庫
2.  connect your application to the database 將您的應用程式連接到資料庫
3.  trigger a function within your code that writes data to the database 觸發程式碼中的一個函數，該函數將資料寫入資料庫
4.  check that the expected data has been written to the database by reading the data from the database 透過從資料庫讀取資料來檢查預期的資料是否已寫入資料庫

Another example, testing that your service integrates with a separate service via a REST API could look like this:

另一個例子，測試您的服務是否透過 REST API 與單獨的服務整合，可能如下所示：


![](https://martinfowler.com/articles/practical-test-pyramid/httpIntegrationTest.png)

Figure 7: This kind of integration test checks that your application can communicate with a separate service correctly 這種類型的整合測試檢查您的應用程式是否可以與單獨的服務正確通訊

1.  start your application 啟動您的應用程式
2.  start an instance of the separate service (or a test double with the same interface)  啟動單獨服務的實例（或具有相同介面的測試替身）
3.  trigger a function within your code that reads from the separate service's API 觸發程式碼中的一個函數，該函數從單獨服務的 API 讀取資料
4.  check that your application can parse the response correctly 檢查您的應用程式是否可以正確解析回應

Your integration tests - like unit tests - can be fairly whitebox. Some frameworks allow you to start your application while still being able to mock some other parts of your application so that you can check that the correct interactions have happened.

您的整合測試（如單元測試）可能相當於白盒。某些框架允許您在啟動應用程式的同時仍然能夠模擬應用程式的其他一些部分，以便您可以檢查是否發生了正確的互動。

Write integration tests for all pieces of code where you either _serialize_ or _deserialize_ data. This happens more often than you might think. Think about:

為您 序列化 或 還原序列化 資料的所有程式碼片段編寫整合測試。這種情況發生的頻率可能比您想像的要高。考慮：

-   Calls to your services' REST API 對您服務的 REST API 的呼叫
-   Reading from and writing to databases 從資料庫讀取和寫入資料
-   Calling other application's APIs 呼叫其他應用程式的 API
-   Reading from and writing to queues 從佇列讀取和寫入資料
-   Writing to the filesystem 寫入檔案系統

Writing integration tests around these boundaries ensures that writing data to and reading data from these external collaborators works fine.

圍繞這些邊界編寫整合測試可確保將資料寫入和從這些外部協作者讀取資料的過程正常工作。

When writing _narrow integration tests_ you should aim to run your external dependencies locally: spin up a local MySQL database, test against a local ext4 filesystem. If you're integrating with a separate service either run an instance of that service locally or build and run a fake version that mimics the behaviour of the real service.

在編寫 狹義整合測試 時，您應該努力在本地執行您的外部依賴項：啟動本地 MySQL 資料庫，針對本地 ext4 檔案系統進行測試。如果您正在與單獨的服務整合，請在本地執行該服務的實例，或建置並執行一個模擬真實服務行為的假版本。

If there's no way to run a third-party service locally you should opt for running a dedicated test instance and point at this test instance when running your integration tests. Avoid integrating with the real production system in your automated tests. Blasting thousands of test requests against a production system is a surefire way to get people angry because you're cluttering their logs (in the best case) or even DoS 'ing their service (in the worst case). Integrating with a service over the network is a typical characteristic of a _broad integration test_ and makes your tests slower and usually harder to write.

如果無法在本地執行第三方服務，則應選擇執行專用測試實例，並在執行整合測試時指向此測試實例。避免在您的自動化測試中與真實的生產系統整合。針對生產系統發送數千個測試請求肯定會激怒人們，因為您正在整理他們的日誌（在最好的情況下），或者甚至 DoS 他們的服務（在最壞的情況下）。透過網路與服務整合是 廣義整合測試 的典型特徵，並使您的測試速度變慢且通常更難編寫。

With regards to the test pyramid, integration tests are on a higher level than your unit tests. Integrating slow parts like filesystems and databases tends to be much slower than running unit tests with these parts stubbed out. They can also be harder to write than small and isolated unit tests, after all you have to take care of spinning up an external part as part of your tests. Still, they have the advantage of giving you the confidence that your application can correctly work with all the external parts it needs to talk to. Unit tests can't help you with that.

關於測試金字塔，整合測試比單元測試層級更高。整合檔案系統和資料庫等緩慢的部分往往比執行具有這些部分 stubbed 的單元測試慢得多。它們也可能比小型且隔離的單元測試更難編寫，畢竟您必須負責啟動外部部分作為測試的一部分。儘管如此，它們的優點是讓您確信您的應用程式可以正確地與它需要通訊的所有外部部分一起工作。單元測試無法幫助您做到這一點。

### Database Integration

The `PersonRepository` is the only repository class in the codebase. It relies on _Spring Data_ and has no actual implementation. It just extends the `CrudRepository` interface and provides a single method header. The rest is Spring magic.

PersonRepository 是程式碼庫中唯一的 repository 類別。它依賴於 Spring Data 並且沒有實際實作。它只是擴展了 CrudRepository 介面並提供了一個方法標頭。其餘的是 Spring 的魔力。

```java
public interface PersonRepository extends CrudRepository<Person, String> { 
	Optional<Person> findByLastName(String lastName); 
}
```

With the `CrudRepository` interface Spring Boot offers a fully functional CRUD repository with `findOne`, `findAll`, `save`, `update` and `delete` methods. Our custom method definition (`findByLastName()`) extends this basic functionality and gives us a way to fetch `Person`s by their last name. Spring Data analyses the return type of the method and its method name and checks the method name against a naming convention to figure out what it should do.

透過 CrudRepository 介面，Spring Boot 提供了一個功能齊全的 CRUD repository，具有 findOne、findAll、save、update 和 delete 方法。我們的自訂方法定義 (findByLastName()) 擴展了這個基本功能，並為我們提供了一種按姓氏提取 Person 的方法。 Spring Data 分析方法的傳回類型及其方法名稱，並根據命名約定檢查方法名稱，以確定它應該做什麼。

Although Spring Data does the heavy lifting of implementing database repositories I still wrote a database integration test. You might argue that this is _testing the framework_ and something that I should avoid as it's not our code that we're testing. Still, I believe having at least one integration test here is crucial. First it tests that our custom `findByLastName` method actually behaves as expected. Secondly it proves that our repository used Spring's wiring correctly and can connect to the database.

儘管 Spring Data 完成了實作資料庫 repositories 的繁重工作，但我仍然編寫了一個資料庫整合測試。您可能會爭辯說，這是 測試框架，是我應該避免的事情，因為我們測試的不是我們的程式碼。儘管如此，我相信在這裡至少有一個整合測試至關重要。首先，它測試了我們的自訂 findByLastName 方法實際上是否按預期執行。其次，它證明了我們的 repository 正確使用了 Spring 的配置，並且可以連接到資料庫。

To make it easier for you to run the tests on your machine (without having to install a PostgreSQL database) our test connects to an in-memory _H2_ database.

為了讓您更容易在您的機器上執行測試（而不必安裝 PostgreSQL 資料庫），我們的測試連接到一個記憶體 H2 資料庫。

I've defined H2 as a test dependency in the `build.gradle` file. The `application.properties` in the test directory doesn't define any `spring.datasource` properties. This tells Spring Data to use an in-memory database. As it finds H2 on the classpath it simply uses H2 when running our tests.

我在 build.gradle 檔案中將 H2 定義為測試依賴項。測試目錄中的 application.properties 沒有定義任何 spring.datasource 屬性。這告訴 Spring Data 使用記憶體資料庫。當它在類別路徑中找到 H2 時，它會在執行我們的測試時簡單地使用 H2。

When running the real application with the `int` profile (e.g. by setting `SPRING_PROFILES_ACTIVE=int` as environment variable) it connects to a PostgreSQL database as defined in the `application-int.properties`.

當使用 int 配置檔案執行真實應用程式時（例如，透過將 SPRING_PROFILES_ACTIVE=int 設定為環境變數），它會連接到 application-int.properties 中定義的 PostgreSQL 資料庫。

I know, that's an awful lot of Spring specifics to know and understand. To get there, you'll have to sift through [a lot of documentation](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-sql.html#boot-features-embedded-database-support). The resulting code is easy on the eye but hard to understand if you don't know the fine details of Spring.

我知道，要了解和理解這些需要了解很多 Spring 的細節。要達到這個目標，您必須篩選[大量文件](https://www.google.com/url?sa=E&q=https%3A%2F%2Fdocs.spring.io%2Fspring-boot%2Fdocs%2Fcurrent%2Freference%2Fhtml%2Fboot-features-sql.html%23boot-features-embedded-database-support)。產生的程式碼看起來很簡單，但如果您不知道 Spring 的細節，則很難理解。

On top of that going with an in-memory database is risky business. After all, our integration tests run against a different type of database than they would in production. Go ahead and decide for yourself if you prefer Spring magic and simple code over an explicit yet more verbose implementation.

最重要的是，使用記憶體資料庫是有風險的。畢竟，我們的整合測試運行在與生產環境中不同的資料庫類型上。繼續前進，自己決定如果您更喜歡 Spring 的魔力和簡單的程式碼，而不是更明確但更冗長的實作。

Enough explanation already, here's a simple integration test that saves a Person to the database and finds it by its last name:

解釋已經夠多了，這是一個簡單的整合測試，它將 Person 儲存到資料庫，並按其姓氏找到它：

```java
@RunWith(SpringRunner.class)
@DataJpaTest
public class PersonRepositoryIntegrationTest {
    @Autowired
    private PersonRepository subject;

    @After
    public void tearDown() throws Exception {
        subject.deleteAll();
    }

    @Test
    public void shouldSaveAndFetchPerson() throws Exception {
        Person peter = new Person("Peter", "Pan");
        subject.save(peter);

        Optional<Person>; maybePeter = subject.findByLastName("Pan");

        assertThat(maybePeter, is(Optional.of(peter)));
    }
}
```

You can see that our integration test follows the same _arrange, act, assert_ structure as the unit tests. Told you that this was a universal concept!

您可以看到我們的整合測試遵循與單元測試相同的 arrange, act, assert 結構。告訴過您這是一個通用概念！

### Integration With Separate Services

Our microservice talks to [darksky.net](https://darksky.net/), a weather REST API. Of course we want to ensure that our service sends requests and parses the responses correctly.

我們的微服務與 [darksky.net](https://www.google.com/url?sa=E&q=https%3A%2F%2Fdarksky.net%2F) 通訊，這是一個天氣 REST API。當然，我們希望確保我們的服務發送請求並正確解析回應。

We want to avoid hitting the real _darksky_ servers when running automated tests. Quota limits of our free plan are only part of the reason. The real reason is _decoupling_. Our tests should run independently of whatever the lovely people at darksky.net are doing. Even when your machine can't access the _darksky_ servers or the darksky servers are down for maintenance.

我們希望在運行自動化測試時避免訪問真實的 darksky 伺服器。我們的免費方案的配額限制只是部分原因。真正的原因是 解耦。我們的測試應該獨立於 darksky.net 的可愛的人們正在做的事情運行。即使您的機器無法存取 darksky 伺服器或 darksky 伺服器因維護而關閉也是如此。

We can avoid hitting the real _darksky_ servers by running our own, fake _darksky_ server while running our integration tests. This might sound like a huge task. Thanks to tools like [Wiremock](http://wiremock.org/) it's easy peasy. Watch this:

我們可以透過在運行我們的整合測試時運行我們自己的、假的 darksky 伺服器來避免訪問真實的 darksky 伺服器。這聽起來可能是一項艱鉅的任務。由於像 [Wiremock](https://www.google.com/url?sa=E&q=http%3A%2F%2Fwiremock.org%2F) 這樣的工具，它很容易。看看這個：

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class WeatherClientIntegrationTest {

    @Autowired
    private WeatherClient subject;

    @Rule
    public WireMockRule wireMockRule = new WireMockRule(8089);

    @Test
    public void shouldCallWeatherService() throws Exception {
        wireMockRule.stubFor(get(urlPathEqualTo("/some-test-api-key/53.5511,9.9937"))
                .willReturn(aResponse()
                        .withBody(FileLoader.read("classpath:weatherApiResponse.json"))
                        .withHeader(CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE)
                        .withStatus(200)));

        Optional&lt;WeatherResponse&gt; weatherResponse = subject.fetchWeather();

        Optional&lt;WeatherResponse&gt; expectedResponse = Optional.of(new WeatherResponse("Rain"));
        assertThat(weatherResponse, is(expectedResponse));
    }
}
```

To use Wiremock we instantiate a `WireMockRule` on a fixed port (`8089`). Using the DSL we can set up the Wiremock server, define the endpoints it should listen on and set canned responses it should respond with.

為了使用 Wiremock，我們在一個固定端口 (8089) 上實例化一個 WireMockRule。 使用 DSL，我們可以設定 Wiremock 伺服器，定義它應該監聽的端點，並設定它應該回應的 canned 回應。

Next we call the method we want to test, the one that calls the third-party service and check if the result is parsed correctly.

接下來，我們呼叫我們要測試的方法，即呼叫第三方服務的方法，並檢查是否正確解析了結果。

It's important to understand how the test knows that it should call the fake Wiremock server instead of the real _darksky_ API. The secret is in our `application.properties` file contained in `src/test/resources`. This is the properties file Spring loads when running tests. In this file we override configuration like API keys and URLs with values that are suitable for our testing purposes, e.g. calling the fake Wiremock server instead of the real one:

重要的是要理解測試如何知道它應該呼叫假的 Wiremock 伺服器而不是真實的 darksky API。 秘密在於我們包含在 src/test/resources 中的 application.properties 檔案中。 這是 Spring 在執行測試時載入的屬性檔案。 在這個檔案中，我們使用適合我們測試目的的值（例如，呼叫假的 Wiremock 伺服器而不是真實的伺服器）覆蓋 API 密鑰和 URL 等配置：

```
weather.url = http://localhost:8089
```

Note that the port defined here has to be the same we define when instantiating the `WireMockRule` in our test. Replacing the real weather API's URL with a fake one in our tests is made possible by injecting the URL in our `WeatherClient` class' constructor:

請注意，此處定義的端口必須與我們在測試中實例化 WireMockRule 時定義的端口相同。 透過在我們的 WeatherClient 類別的構造函數中注入 URL，可以將真實天氣 API 的 URL 替換為測試中的假 URL：

```java
@Autowired
public WeatherClient(final RestTemplate restTemplate,
                     @Value("${weather.url}") final String weatherServiceUrl,
                     @Value("${weather.api_key}") final String weatherServiceApiKey) {
    this.restTemplate = restTemplate;
    this.weatherServiceUrl = weatherServiceUrl;
    this.weatherServiceApiKey = weatherServiceApiKey;
}
```

This way we tell our `WeatherClient` to read the `weatherUrl` parameter's value from the `weather.url` property we define in our application properties.

透過這種方式，我們告訴我們的 WeatherClient 從我們在應用程式屬性中定義的 weather.url 屬性讀取 weatherUrl 參數的值。

Writing _narrow integration tests_ for a separate service is quite easy with tools like Wiremock. Unfortunately there's a downside to this approach: How can we ensure that the fake server we set up behaves like the real server? With the current implementation, the separate service could change its API and our tests would still pass. Right now we're merely testing that our `WeatherClient` can parse the responses that the fake server sends. That's a start but it's very brittle. Using _end-to-end tests_ and running the tests against a test instance of the real service instead of using a fake service would solve this problem but would make us reliant on the availability of the test service. Fortunately, there's a better solution to this dilemma: Running contract tests against the fake and the real server ensures that the fake we use in our integration tests is a faithful test double. Let's see how this works next.

使用 Wiremock 等工具為單獨的服務編寫 狹義整合測試 相當容易。 不幸的是，這種方法有一個缺點：我們如何確保我們設定的假伺服器的行為與真實伺服器相同？ 在目前的實作中，單獨的服務可以更改其 API，而我們的測試仍然會通過。 現在我們僅僅測試我們的 WeatherClient 是否可以解析假伺服器發送的回應。 這是一個好的開始，但它非常脆弱。 使用 端到端測試 並針對真實服務的測試實例運行測試，而不是使用假服務，可以解決這個問題，但會使我們依賴於測試服務的可用性。 幸運的是，對於這個兩難困境有一個更好的解決方案：針對假伺服器和真實伺服器運行合約測試可確保我們在整合測試中使用的假伺服器是一個忠實的測試替身。 讓我們看看接下來這是如何運作的。

## Contract Tests

More modern software development organisations have found ways of scaling their development efforts by spreading the development of a system across different teams. Individual teams build individual, loosely coupled services without stepping on each others toes and integrate these services into a big, cohesive system. The more recent buzz around microservices focuses on exactly that.

更現代化的軟體開發組織已經找到方法來擴展其開發工作，方法是將系統的開發分散到不同的團隊中。 各個團隊建構單獨的、鬆散耦合的服務，而不會互相干擾，並將這些服務整合到一個大的、有凝聚力的系統中。最近關於微服務的熱議正是關注於此。

Splitting your system into many small services often means that these services need to communicate with each other via certain (hopefully well-defined, sometimes accidentally grown) interfaces.

將您的系統分成許多小型服務通常意味著這些服務需要透過某些（希望定義良好，有時意外成長）介面相互通訊。

Interfaces between different applications can come in different shapes and technologies. Common ones are

不同應用程式之間的介面可以有多種形式和技術。常見的有

-   REST and JSON via HTTPS 透過 HTTPS 的 REST 和 JSON
-   RPC using something like [gRPC](https://grpc.io/) 使用 [gRPC](https://www.google.com/url?sa=E&q=https%3A%2F%2Fgrpc.io%2F) 之類的 RPC
-   building an event-driven architecture using queues 使用佇列建構事件驅動架構

For each interface there are two parties involved: the provider and the consumer. The **provider** serves data to consumers. The **consumer** processes data obtained from a provider. In a REST world a provider builds a REST API with all required endpoints; a consumer makes calls to this REST API to fetch data or trigger changes in the other service. In an asynchronous, event-driven world, a provider (often rather called **publisher**) publishes data to a queue; a consumer (often called **subscriber**) subscribes to these queues and reads and processes data.

對於每個介面，都涉及兩個參與方：提供者和消費者。 **提供者** 向消費者提供資料。 **消費者** 處理從提供者那裡獲得的資料。 在 REST 世界中，提供者建構具有所有必需端點的 REST API； 消費者呼叫此 REST API 以提取資料或觸發另一個服務中的變更。 在非同步、事件驅動的世界中，提供者（通常稱為 **發布者**）將資料發布到佇列； 消費者（通常稱為 **訂閱者**）訂閱這些佇列並讀取和處理資料。


![](https://martinfowler.com/articles/practical-test-pyramid/contract_tests.png)

Figure 8: Each interface has a providing (or publishing) and a consuming (or subscribing) party. The specification of an interface can be considered a contract. 每個介面都有一個提供（或發布）方和一個消費（或訂閱）方。 介面的規格可以被認為是一份合約。

As you often spread the consuming and providing services across different teams you find yourself in the situation where you have to clearly specify the interface between these services (the so called **contract**). Traditionally companies have approached this problem in the following way:

由於您通常將消費和提供服務分散在不同的團隊中，因此您會發現自己處於必須清楚地指定這些服務之間的介面（所謂的 **合約**）的情況。 傳統上，公司已經以以下方式處理這個問題：

-   Write a long and detailed interface specification (the _contract_) 編寫一個長而詳細的介面規格（合約）
-   Implement the providing service according to the defined contract 根據定義的合約實作提供服務
-   Throw the interface specification over the fence to the consuming team 將介面規格丟給消費團隊
-   Wait until they implement their part of consuming the interface 等到他們實作他們消費介面的部分
-   Run some large-scale manual system test to see if everything works 執行一些大規模的手動系統測試，以查看一切是否正常
-   Hope that both teams stick to the interface definition forever and don't screw up 希望兩個團隊永遠堅持介面定義，並且不要搞砸

More modern software development teams have replaced steps 5. and 6. with something more automated: Automated [contract tests](https://martinfowler.com/bliki/ContractTest.html) make sure that the implementations on the consumer and provider side still stick to the defined contract. They serve as a good regression test suite and make sure that deviations from the contract will be noticed early.

更現代化的軟體開發團隊已經用更自動化的東西取代了步驟 5 和 6：自動化的 [合約測試](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Fbliki%2FContractTest.html) 確保消費者和提供者端的實作仍然堅持定義的合約。 它們可以作為良好的迴歸測試套件，並確保及早發現與合約的偏差。

In a more agile organisation you should take the more efficient and less wasteful route. You build your applications within the same organisation. It really shouldn't be too hard to talk to the developers of the other services directly instead of throwing overly detailed documentation over the fence. After all they're your co-workers and not a third-party vendor that you could only talk to via customer support or legally bulletproof contracts.

在更敏捷的組織中，您應該採取更有效率且浪費更少的途徑。 您在同一個組織內建構您的應用程式。 與其將過於詳細的文件丟過圍欄，不如直接與其他服務的開發人員交談，這真的不應該太難。 畢竟，他們是您的同事，而不是您只能透過客戶支援或法律上萬無一失的合約交談的第三方供應商。

**Consumer-Driven Contract tests** (CDC tests) let the [consumers drive the implementation of a contract](https://martinfowler.com/articles/consumerDrivenContracts.html). Using CDC, consumers of an interface write tests that check the interface for all data they need from that interface. The consuming team then publishes these tests so that the publishing team can fetch and execute these tests easily. The providing team can now develop their API by running the CDC tests. Once all tests pass they know they have implemented everything the consuming team needs.

**消費者驅動合約測試** (CDC 測試) 讓 [消費者驅動合約的實作](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Farticles%2FconsumerDrivenContracts.html)。 透過使用 CDC，介面的消費者編寫測試，檢查介面的所有資料是否需要從該介面獲取。 然後，消費團隊發布這些測試，以便發布團隊可以輕鬆提取和執行這些測試。 提供團隊現在可以透過運行 CDC 測試來開發他們的 API。 一旦所有測試都通過，他們就知道他們已經實作了消費團隊需要的所有內容。

![](https://martinfowler.com/articles/practical-test-pyramid/cdc_tests.png)

Figure 9: Contract tests ensure that the provider and all consumers of an interface stick to the defined interface contract. With CDC tests consumers of an interface publish their requirements in the form of automated tests; the providers fetch and execute these tests continuously 合約測試確保介面的提供者和所有消費者都堅持定義的介面合約。 透過 CDC 測試，介面的消費者以自動化測試的形式發布他們的需求； 提供者不斷提取和執行這些測試

This approach allows the providing team to implement only what's really necessary (keeping things simple, YAGNI and all that). The team providing the interface should fetch and run these CDC tests continuously (in their build pipeline) to spot any breaking changes immediately. If they break the interface their CDC tests will fail, preventing breaking changes to go live. As long as the tests stay green the team can make any changes they like without having to worry about other teams. The Consumer-Driven Contract approach would leave you with a process looking like this:

這種方法允許提供團隊僅實作真正需要的東西（保持簡單，YAGNI 等等）。 提供介面的團隊應不斷提取和運行這些 CDC 測試（在其建置管道中），以立即發現任何重大變更。 如果他們破壞了介面，他們的 CDC 測試將會失敗，從而阻止重大變更上線。 只要測試保持綠色，團隊就可以進行任何他們喜歡的變更，而不必擔心其他團隊。 消費者驅動合約方法會讓您得到如下所示的流程：

-   The consuming team writes automated tests with all consumer expectations 消費團隊使用所有消費者期望編寫自動化測試
-   They publish the tests for the providing team 他們為提供團隊發布測試
-   The providing team runs the CDC tests continuously and keeps them green 提供團隊不斷運行 CDC 測試並保持它們為綠色
-   Both teams talk to each other once the CDC tests break 一旦 CDC 測試中斷，兩個團隊就會相互交談

If your organisation adopts a microservices approach, having CDC tests is a big step towards establishing autonomous teams. CDC tests are an automated way to foster team communication. They ensure that interfaces between teams are working at any time. Failing CDC tests are a good indicator that you should walk over to the affected team, have a chat about any upcoming API changes and figure out how you want to move forward.

如果您的組織採用微服務方法，那麼擁有 CDC 測試是邁向建立自主團隊的一大步。 CDC 測試是促進團隊溝通的一種自動化方式。 它們確保團隊之間的介面始終有效。 失敗的 CDC 測試是一個很好的指標，表明您應該走到受影響的團隊，就任何即將發生的 API 變更進行聊天，並弄清楚您想如何前進。

A naive implementation of CDC tests can be as simple as firing requests against an API and assert that the responses contain everything you need. You then package these tests as an executable (.gem, .jar, .sh) and upload it somewhere the other team can fetch it (e.g. an artifact repository like [Artifactory](https://www.jfrog.com/artifactory/)).

CDC 測試的簡單實作可以簡單到向 API 發送請求並斷言回應包含您需要的所有內容。 然後，您可以將這些測試打包為可執行檔（.gem、.jar、.sh）並將其上傳到其他團隊可以提取它的地方（例如，像 [Artifactory](https://www.google.com/url?sa=E&q=https%3A%2F%2Fwww.jfrog.com%2Fartifactory%2F) 這樣的工件庫）。

Over the last couple of years the CDC approach has become more and more popular and several tools been build to make writing and exchanging them easier.

在過去幾年中，CDC 方法變得越來越流行，並且已經建立了幾個工具來使編寫和交換它們更容易。

[Pact](https://github.com/realestate-com-au/pact) is probably the most prominent one these days. It has a sophisticated approach of writing tests for the consumer and the provider side, gives you stubs for separate services out of the box and allows you to exchange CDC tests with other teams. Pact has been ported to a lot of platforms and can be used with JVM languages, Ruby, .NET, JavaScript and many more.

[Pact](https://www.google.com/url?sa=E&q=https%3A%2F%2Fgithub.com%2Frealestate-com-au%2Fpact) 可能是目前最突出的工具之一。 它具有為消費者和提供者端編寫測試的複雜方法，開箱即用地為您提供單獨服務的 stubs，並允許您與其他團隊交換 CDC 測試。 Pact 已被移植到許多平台，並且可以與 JVM 語言、Ruby、.NET、JavaScript 等一起使用。

If you want to get started with CDCs and don't know how, Pact can be a sane choice. The [documentation](https://docs.pact.io/) can be overwhelming at first. Be patient and work through it. It helps to get a firm understanding for CDCs which in turn makes it easier for you to advocate for the use of CDCs when working with other teams.

如果您想開始使用 CDC 並且不知道如何做，那麼 Pact 可能是一個合理的選擇。 [文檔](https://www.google.com/url?sa=E&q=https%3A%2F%2Fdocs.pact.io%2F) 一開始可能會讓人不知所措。 請耐心並完成它。 它有助於牢固地理解 CDC，這反過來又使您在與其他團隊合作時更容易提倡使用 CDC。

Consumer-Driven Contract tests can be a real game changer to establish autonomous teams that can move fast and with confidence. Do yourself a favor, read up on that concept and give it a try. A solid suite of CDC tests is invaluable for being able to move fast without breaking other services and cause a lot of frustration with other teams.

消費者驅動合約測試可能是建立可以快速且自信地行動的自主團隊的一個真正的遊戲規則改變者。 幫自己一個忙，閱讀一下這個概念並嘗試一下。 一套可靠的 CDC 測試對於能夠快速行動而不會破壞其他服務並導致其他團隊感到沮喪來說是無價的。

### Consumer Test (our team)

Our microservice consumes the weather API. So it's our responsibility to write a **consumer test** that defines our expectations for the contract (the API) between our microservice and the weather service.

我們的微服務消費天氣 API。 因此，我們有責任編寫一個 **消費者測試**，該測試定義了我們對微服務和天氣服務之間合約（API）的期望。

First we include a library for writing pact consumer tests in our `build.gradle`:

首先，我們在 build.gradle 中包含一個用於編寫 pact 消費者測試的函式庫：

```
testCompile('au.com.dius:pact-jvm-consumer-junit_2.11:3.5.5')
```

Thanks to this library we can implement a consumer test and use pact's mock services:

由於這個函式庫，我們可以實作一個消費者測試並使用 pact 的模擬服務：

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class WeatherClientConsumerTest {

    @Autowired
    private WeatherClient weatherClient;

    @Rule
    public PactProviderRuleMk2 weatherProvider =
            new PactProviderRuleMk2("weather_provider", "localhost", 8089, this);

    @Pact(consumer="test_consumer")
    public RequestResponsePact createPact(PactDslWithProvider builder) throws IOException {
        return builder
                .given("weather forecast data")
                .uponReceiving("a request for a weather request for Hamburg")
                    .path("/some-test-api-key/53.5511,9.9937")
                    .method("GET")
                .willRespondWith()
                    .status(200)
                    .body(FileLoader.read("classpath:weatherApiResponse.json"),
                            ContentType.APPLICATION_JSON)
                .toPact();
    }

    @Test
    @PactVerification("weather_provider")
    public void shouldFetchWeatherInformation() throws Exception {
        Optional&lt;WeatherResponse&gt; weatherResponse = weatherClient.fetchWeather();
        assertThat(weatherResponse.isPresent(), is(true));
        assertThat(weatherResponse.get().getSummary(), is("Rain"));
    }
}
```

If you look closely, you'll see that the `WeatherClientConsumerTest` is very similar to the `WeatherClientIntegrationTest`. Instead of using Wiremock for the server stub we use Pact this time. In fact the consumer test works exactly as the integration test, we replace the real third-party server with a stub, define the expected response and check that our client can parse the response correctly. In this sense the `WeatherClientConsumerTest` is a narrow integration test itself. The advantage over the wiremock-based test is that this test generates a _pact file_ (found in `target/pacts/&pact-name>.json`) each time it runs. This pact file describes our expectations for the contract in a special JSON format. This pact file can then be used to verify that our stub server behaves like the real server. We can take the pact file and hand it to the team providing the interface. They take this pact file and write a provider test using the expectations defined in there. This way they test if their API fulfils all our expectations.

如果您仔細觀察，您會發現 WeatherClientConsumerTest 與 WeatherClientIntegrationTest 非常相似。 這次我們使用 Pact 而不是使用 Wiremock 作為伺服器 stub。 實際上，消費者測試的工作方式與整合測試完全相同，我們使用 stub 替換真實的第三方伺服器，定義預期的回應，並檢查我們的用戶端是否可以正確解析回應。 從這個意義上講，WeatherClientConsumerTest 本身就是一個狹義的整合測試。 優於基於 wiremock 的測試的優點是，每次運行時，此測試都會產生一個 pact 檔案（位於 target/pacts/&pact-name>.json 中）。 此 pact 檔案以特殊的 JSON 格式描述了我們對合約的期望。 然後可以使用此 pact 檔案來驗證我們的 stub 伺服器的行為是否與真實伺服器相同。 我們可以採用 pact 檔案並將其交給提供介面的團隊。 他們採用此 pact 檔案並使用其中定義的期望編寫提供者測試。 透過這種方式，他們測試他們的 API 是否滿足我們所有的期望。

You see that this is where the _consumer-driven_ part of CDC comes from. The consumer drives the implementation of the interface by describing their expectations. The provider has to make sure that they fulfil all expectations and they're done. No gold-plating, no YAGNI and stuff.

您會看到，這就是 CDC 的 消費者驅動 部分的來源。 消費者透過描述他們的期望來驅動介面的實作。 提供者必須確保他們滿足所有期望並且他們已完成。 沒有鍍金，沒有 YAGNI 之類的東西。

Getting the pact file to the providing team can happen in multiple ways. A simple one is to check them into version control and tell the provider team to always fetch the latest version of the pact file. A more advances one is to use an artifact repository, a service like Amazon's S3 or the pact broker. Start simple and grow as you need.

可以透過多種方式將 pact 檔案提供給提供團隊。 一個簡單的方法是將它們檢入版本控制系統，並告訴提供團隊始終獲取最新版本的 pact 檔案。 更高級的方法是使用工件庫、像 Amazon 的 S3 這樣的服務或 pact 代理。 從簡單開始並根據需要成長。

In your real-world application you don't need both, an _integration test_ and a _consumer test_ for a client class. The sample codebase contains both to show you how to use either one. If you want to write CDC tests using pact I recommend sticking to the latter. The effort of writing the tests is the same. Using pact has the benefit that you automatically get a pact file with the expectations to the contract that other teams can use to easily implement their provider tests. Of course this only makes sense if you can convince the other team to use pact as well. If this doesn't work, using the _integration test_ and Wiremock combination is a decent plan b.

在您的真實應用程式中，您不需要一個用戶端類別的 整合測試 和 消費者測試。 範例程式碼庫包含兩者，以向您展示如何使用其中一種。 如果您想使用 pact 編寫 CDC 測試，我建議您堅持使用後者。 編寫測試的工作量是相同的。 使用 pact 的好處是，您可以自動獲得一個 pact 檔案，其中包含合約的期望，其他團隊可以使用該檔案輕鬆地實作他們的提供者測試。 當然，只有在您能說服其他團隊也使用 pact 時，這才有意義。 如果這不起作用，那麼使用 整合測試 和 Wiremock 組合是一個不錯的 B 計劃。

### Provider Test (the other team)

The provider test has to be implemented by the people providing the weather API. We're consuming a public API provided by darksky.net. In theory the darksky team would implement the provider test on their end to check that they're not breaking the contract between their application and our service.

提供者測試必須由提供天氣 API 的人員實作。 我們正在使用 darksky.net 提供的公共 API。 理論上，darksky 團隊會在他們的端實作提供者測試，以檢查他們是否沒有破壞他們的應用程式和我們的服務之間的合約。

Obviously they don't care about our meager sample application and won't implement a CDC test for us. That's the big difference between a public-facing API and an organisation adopting microservices. Public-facing APIs can't consider every single consumer out there or they'd become unable to move forward. Within your own organisation, you can — and should. Your app will most likely serve a handful, maybe a couple dozen of consumers max. You'll be fine writing provider tests for these interfaces in order to keep a stable system.

顯然，他們不在乎我們可憐的範例應用程式，也不會為我們實作 CDC 測試。 這是面向公眾的 API 和採用微服務的組織之間的最大區別。 面向公眾的 API 無法考慮那裡的每個消費者，否則他們將無法前進。 在您自己的組織中，您可以——並且應該。 您的應用程式很可能會為少數幾個，最多幾十個消費者提供服務。 編寫這些介面的提供者測試以保持系統穩定，您會沒事的。

The providing team gets the pact file and runs it against their providing service. To do so they implement a provider test that reads the pact file, stubs out some test data and runs the expectations defined in the pact file against their service.

提供團隊獲取 pact 檔案並針對其提供服務運行它。 為此，他們實作一個提供者測試，該測試讀取 pact 檔案，stub 一些測試資料，並根據他們的服務運行 pact 檔案中定義的期望。

The pact folks have written several libraries for implementing provider tests. Their main [GitHub repo](https://github.com/DiUS/pact-jvm) gives you a nice overview which consumer and which provider libraries are available. Pick the one that best matches your tech stack.

pact 人員編寫了幾個用於實作提供者測試的函式庫。 他們的主要 [GitHub 倉庫](https://www.google.com/url?sa=E&q=https%3A%2F%2Fgithub.com%2FDiUS%2Fpact-jvm) 為您提供了哪些消費者和哪些提供者函式庫可用的簡潔概述。 選擇最符合您的技術堆疊的那個。

For simplicity let's assume that the darksky API is implemented in Spring Boot as well. In this case they could use the [Spring pact provider](https://github.com/DiUS/pact-jvm/tree/master/pact-jvm-provider-spring) which hooks nicely into Spring's MockMVC mechanisms. A hypothetical provider test that the darksky.net team would implement could look like this:

為了簡單起見，讓我們假設 darksky API 也以 Spring Boot 實作。 在這種情況下，他們可以使用 [Spring pact 提供者](https://www.google.com/url?sa=E&q=https%3A%2F%2Fgithub.com%2FDiUS%2Fpact-jvm%2Ftree%2Fmaster%2Fpact-jvm-provider-spring)，它可以很好地與 Spring 的 MockMVC 機制掛鉤。 darksky.net 團隊將實作的假設提供者測試可能如下所示：

```java
@RunWith(RestPactRunner.class)
@Provider("weather_provider") // same as the "provider_name" in our clientConsumerTest
@PactFolder("target/pacts") // tells pact where to load the pact files from
public class WeatherProviderTest {
    @InjectMocks
    private ForecastController forecastController = new ForecastController();

    @Mock
    private ForecastService forecastService;

    @TestTarget
    public final MockMvcTarget target = new MockMvcTarget();

    @Before
    public void before() {
        initMocks(this);
        target.setControllers(forecastController);
    }

    @State("weather forecast data") // same as the "given()" in our clientConsumerTest
    public void weatherForecastData() {
        when(forecastService.fetchForecastFor(any(String.class), any(String.class)))
                .thenReturn(weatherForecast("Rain"));
    }
}
```

You see that all the provider test has to do is to load a pact file (e.g. by using the `@PactFolder` annotation to load previously downloaded pact files) and then define how test data for pre-defined states should be provided (e.g. using Mockito mocks). There's no custom test to be implemented. These are all derived from the pact file. It's important that the provider test has matching counterparts to the _provider name_ and _state_ declared in the consumer test.

您會看到，提供者測試所要做的就是載入 pact 檔案（例如，透過使用 @PactFolder 註解載入先前下載的 pact 檔案），然後定義應如何提供預定義狀態的測試資料（例如，使用 Mockito mocks）。 沒有要實作的自訂測試。 這些都是從 pact 檔案中派生的。 重要的是，提供者測試具有與消費者測試中聲明的 提供者名稱 和 狀態 相對應的匹配項。

### Provider Test (our team)

We've seen how to test the contract between our service and the weather provider. With this interface our service acts as consumer, the weather service acts as provider. Thinking a little further we'll see that our service also acts as a provider for others: We provide a REST API that offers a couple of endpoints ready to be consumed by others.

我們已經看到了如何測試我們的服務和天氣提供者之間的合約。 透過此介面，我們的服務充當消費者，天氣服務充當提供者。 再稍微思考一下，我們會發現我們的服務也充當其他人的提供者：我們提供一個 REST API，該 API 提供了幾個可供其他人使用的端點。

As we've just learned that contract tests are all the rage, we of course write a contract test for this contract as well. Luckily we're using consumer-driven contracts so there's all the consuming teams sending us their Pacts that we can use to implement our provider tests for our REST API.

正如我們剛才了解到的，合約測試非常流行，因此我們當然也為此合約編寫了一個合約測試。 幸運的是，我們正在使用消費者驅動的合約，因此所有消費團隊都向我們發送了我們可以使用的 Pact，以便為我們的 REST API 實作我們的提供者測試。

Let's first add the Pact provider library for Spring to our project:

讓我們首先將用於 Spring 的 Pact 提供者函式庫新增到我們的專案中：

```
testCompile('au.com.dius:pact-jvm-provider-spring_2.12:3.5.5')
```

Implementing the provider test follows the same pattern as described before. For the sake of simplicity I simply checked the pact file from our [simple consumer](https://github.com/hamvocke/spring-testing-consumer) into our service's repository. This makes it easier for our purpose, in a real-life scenario you're probably going to use a more sophisticated mechanism to distribute your pact files.

實作提供者測試遵循與之前描述的相同的模式。 為了簡單起見，我只是將來自我們 [簡單消費者](https://www.google.com/url?sa=E&q=https%3A%2F%2Fgithub.com%2Fhamvocke%2Fspring-testing-consumer) 的 pact 檔案檢入到我們服務的倉庫中。 這使我們的目的更容易實現，在真實的場景中，您可能會使用更複雜的機制來分發您的 pact 檔案。

```java
@RunWith(RestPactRunner.class)
@Provider("person_provider")// same as in the "provider_name" part in our pact file
@PactFolder("target/pacts") // tells pact where to load the pact files from
public class ExampleProviderTest {

    @Mock
    private PersonRepository personRepository;

    @Mock
    private WeatherClient weatherClient;

    private ExampleController exampleController;

    @TestTarget
    public final MockMvcTarget target = new MockMvcTarget();

    @Before
    public void before() {
        initMocks(this);
        exampleController = new ExampleController(personRepository, weatherClient);
        target.setControllers(exampleController);
    }

    @State("person data") // same as the "given()" part in our consumer test
    public void personData() {
        Person peterPan = new Person("Peter", "Pan");
        when(personRepository.findByLastName("Pan")).thenReturn(Optional.of
                (peterPan));
    }
}
```

The shown `ExampleProviderTest` needs to provide state according to the pact file we're given, that's it. Once we run the provider test, Pact will pick up the pact file and fire HTTP request against our service that then responds according to the state we've set up.

顯示的 ExampleProviderTest 需要根據我們給定的 pact 檔案提供狀態，就是這樣。 一旦我們運行提供者測試，Pact 將會拾取 pact 檔案，並針對我們的服務觸發 HTTP 請求，然後根據我們設定的狀態做出回應。

## UI Tests

Most applications have some sort of user interface. Typically we're talking about a web interface in the context of web applications. People often forget that a REST API or a command line interface is as much of a user interface as a fancy web user interface.

大多數應用程式都有某種使用者介面。 通常，我們在 Web 應用程式的上下文中談論的是 Web 介面。 人們經常忘記，REST API 或命令列介面與精美的 Web 使用者介面一樣都是使用者介面。

_UI tests_ test that the user interface of your application works correctly. User input should trigger the right actions, data should be presented to the user, the UI state should change as expected.

UI 測試 測試您應用程式的使用者介面是否正常運作。 使用者輸入應觸發正確的動作，資料應呈現給使用者，UI 狀態應按預期變更。


![](https://martinfowler.com/articles/practical-test-pyramid/ui_tests.png)

UI Tests and end-to-end tests are sometimes (as in Mike Cohn's case) said to be the same thing. For me this conflates two things that are rather orthogonal concepts.

UI 測試和端到端測試有時（如在 Mike Cohn 的案例中）被認為是同一件事。 對我來說，這混淆了兩個相當正交的概念。

Yes, testing your application end-to-end often means driving your tests through the user interface. The inverse, however, is not true.

是的，端到端地測試您的應用程式通常意味著透過使用者介面驅動您的測試。 但是，反之則不成立。

Testing your user interface doesn't have to be done in an end-to-end fashion. Depending on the technology you use, testing your user interface can be as simple as writing some unit tests for your frontend javascript code with your backend stubbed out.

測試您的使用者介面不必以端到端的方式完成。 根據您使用的技術，測試您的使用者介面可以像為您的前端 javascript 程式碼編寫一些單元測試一樣簡單，您的後端 stubbed 出來。

With traditional web applications testing the user interface can be achieved with tools like [Selenium](http://docs.seleniumhq.org/). If you consider a REST API to be your user interface you should have everything you need by writing proper integration tests around your API.

對於傳統的 Web 應用程式，可以使用像 [Selenium](https://www.google.com/url?sa=E&q=http%3A%2F%2Fdocs.seleniumhq.org%2F) 這樣的工具來實現測試使用者介面。 如果您認為 REST API 是您的使用者介面，那麼您應該擁有通過圍繞您的 API 編寫適當的整合測試來獲得所需的一切。

With web interfaces there's multiple aspects that you probably want to test around your UI: behaviour, layout, usability or adherence to your corporate design are only a few.

對於 Web 介面，您可能想要測試 UI 的多個方面：行為、版面配置、可用性或對公司設計的遵守只是其中的幾個。

Fortunately, testing the _behaviour_ of your user interface is pretty simple. You click here, enter data there and want the state of the user interface to change accordingly. Modern single page application frameworks ([react](https://facebook.github.io/react/), [vue.js](https://vuejs.org/), [Angular](https://angular.io/) and the like) often come with their own tools and helpers that allow you to thoroughly test these interactions in a pretty low-level (unit test) fashion. Even if you roll your own frontend implementation using vanilla javascript you can use your regular testing tools like [Jasmine](https://jasmine.github.io/) or [Mocha](http://mochajs.org/). With a more traditional, server-side rendered application, Selenium-based tests will be your best choice.

幸運的是，測試您使用者介面的 行為 非常簡單。 您在這裡點擊，在那裡輸入資料，並希望使用者介面的狀態相應地變更。 現代單頁應用程式框架（[react](https://www.google.com/url?sa=E&q=https%3A%2F%2Ffacebook.github.io%2Freact%2F)、[vue.js](https://www.google.com/url?sa=E&q=https%3A%2F%2Fvuejs.org%2F)、[Angular](https://www.google.com/url?sa=E&q=https%3A%2F%2Fangular.io%2F) 等）通常配備了自己的工具和助手，使您能夠以相當低層級（單元測試）的方式徹底測試這些互動。 即使您使用 vanilla javascript 推出自己的前端實作，您也可以使用您的常規測試工具，如 [Jasmine](https://www.google.com/url?sa=E&q=https%3A%2F%2Fjasmine.github.io%2F) 或 [Mocha](https://www.google.com/url?sa=E&q=http%3A%2F%2Fmochajs.org%2F)。 對於更傳統的、伺服器端呈現的應用程式，基於 Selenium 的測試將是您最好的選擇。

Testing that your web application's _layout_ remains intact is a little harder. Depending on your application and your users' needs you may want to make sure that code changes don't break the website's layout by accident.

測試您 Web 應用程式的 版面配置 是否保持完整有點困難。 根據您的應用程式和使用者的需求，您可能希望確保程式碼變更不會意外地破壞網站的版面配置。

The problem is that computers are notoriously bad at checking if something “looks good” (maybe some clever machine learning algorithm can change that in the future).

問題在於，電腦在檢查某個東西是否“看起來不錯”方面臭名昭著地糟糕（也許一些聰明的機器學習演算法可以在未來改變這一點）。

There are some tools to try if you want to automatically check your web application's design in your build pipeline. Most of these tools utilise Selenium to open your web application in different browsers and formats, take screenshots and compare these to previously taken screenshots. If the old and new screenshots differ in an unexpected way, the tool will let you know.

如果您想在您的建置管道中自動檢查您 Web 應用程式的設計，有一些工具可以嘗試。 這些工具中的大多數都利用 Selenium 在不同的瀏覽器和格式中開啟您的 Web 應用程式，截取螢幕截圖並將其與先前截取的螢幕截圖進行比較。 如果舊螢幕截圖和新螢幕截圖以非預期的方式不同，該工具將會通知您。

[Galen](http://galenframework.com/) is one of these tools. But even rolling your own solution isn't too hard if you have special requirements. Some teams I've worked with built [lineup](https://github.com/otto-de/lineup) and its Java-based cousin [jlineup](https://github.com/otto-de/jlineup) to achieve something similar. Both tools take the same Selenium-based approach I described before.

[Galen](https://www.google.com/url?sa=E&q=http%3A%2F%2Fgalenframework.com%2F) 就是其中一種工具。 但是，如果您有特殊要求，推出您自己的解決方案也不是太難。 我合作過的一些團隊建構了 [lineup](https://www.google.com/url?sa=E&q=https%3A%2F%2Fgithub.com%2Fotto-de%2Flineup) 及其基於 Java 的表親 [jlineup](https://www.google.com/url?sa=E&q=https%3A%2F%2Fgithub.com%2Fotto-de%2Fjlineup) 來實現類似的東西。 這兩種工具都採用了我之前描述的基於 Selenium 的相同方法。

Once you want to test for _usability_ and a “looks good” factor you leave the realms of automated testing. This is the area where you should rely on [exploratory testing](https://en.wikipedia.org/wiki/Exploratory_testing), usability testing (this can even be as simple as [hallway testing](https://en.wikipedia.org/wiki/Usability_testing#Hallway_testing)) and showcases with your users to see if they like using your product and can use all features without getting frustrated or annoyed.

一旦您想要測試 可用性 和“看起來不錯”的因素，您就會離開自動化測試的領域。 這是您應該依靠 [探索性測試](https://www.google.com/url?sa=E&q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FExploratory_testing)、可用性測試（這甚至可以像 [走廊測試](https://www.google.com/url?sa=E&q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FUsability_testing%23Hallway_testing) 一樣簡單）和與您的使用者展示以查看他們是否喜歡使用您的產品並且可以使用所有功能而不會感到沮喪或惱火的區域。

## End-to-End Tests

Testing your deployed application via its user interface is the most end-to-end way you could test your application. The previously described, webdriver driven UI tests are a good example of end-to-end tests.

透過其使用者介面測試您部署的應用程式是您可以測試您的應用程式的最端到端的方式。 先前描述的、webdriver 驅動的 UI 測試是端到端測試的一個很好的例子。


![](https://martinfowler.com/articles/practical-test-pyramid/e2etests.png)

Figure 11: End-to-end tests test your entire, completely integrated system 端到端測試測試您的整個、完全整合的系統

End-to-end tests (also called [Broad Stack Tests](https://martinfowler.com/bliki/BroadStackTest.html)) give you the biggest confidence when you need to decide if your software is working or not. [Selenium](http://docs.seleniumhq.org/) and the [WebDriver Protocol](https://www.w3.org/TR/webdriver/) allow you to automate your tests by automatically driving a (headless) browser against your deployed services, performing clicks, entering data and checking the state of your user interface. You can use Selenium directly or use tools that are build on top of it, [Nightwatch](http://nightwatchjs.org/) being one of them.

當您需要決定您的軟體是否正常運作時，端到端測試（也稱為 [廣堆疊測試](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Fbliki%2FBroadStackTest.html)）給您最大的信心。 [Selenium](https://www.google.com/url?sa=E&q=http%3A%2F%2Fdocs.seleniumhq.org%2F) 和 [WebDriver 協議](https://www.google.com/url?sa=E&q=https%3A%2F%2Fwww.w3.org%2FTR%2Fwebdriver%2F) 允許您透過自動驅動一個（無頭）瀏覽器針對您部署的服務來自動化您的測試，執行點擊，輸入資料和檢查您使用者介面的狀態。 您可以直接使用 Selenium，也可以使用建立在其之上的工具，[Nightwatch](https://www.google.com/url?sa=E&q=http%3A%2F%2Fnightwatchjs.org%2F) 就是其中之一。

End-to-End tests come with their own kind of problems. They are notoriously flaky and often fail for unexpected and unforeseeable reasons. Quite often their failure is a false positive. The more sophisticated your user interface, the more flaky the tests tend to become. Browser quirks, timing issues, animations and unexpected popup dialogs are only some of the reasons that got me spending more of my time with debugging than I'd like to admit.

端到端測試有它們自己種類的問題。 它們臭名昭著地不穩定，並且經常由於意外和無法預見的原因而失敗。 很多時候它們的失敗是誤報。 您的使用者介面越複雜，測試往往變得越不穩定。 瀏覽器怪癖、時序問題、動畫和意外的彈出對話方塊只是讓我花費更多時間進行除錯的一些原因，而不是我願意承認的。

In a microservices world there's also the big question of who's in charge of writing these tests. Since they span multiple services (your entire system) there's no single team responsible for writing end-to-end tests

在微服務世界中，還有一個大問題是誰負責編寫這些測試。 由於它們跨越多個服務（您的整個系統），因此沒有一個團隊負責編寫端到端測試。

If you have a centralised _quality assurance_ team they look like a good fit. Then again having a centralised QA team is a big anti-pattern and shouldn't have a place in a DevOps world where your teams are meant to be truly cross-functional. There's no easy answer who should own end-to-end tests. Maybe your organisation has a community of practice or a _quality guild_ that can take care of these. Finding the correct answer highly depends on your organisation.

如果您有一個集中的 品質保證 團隊，它們看起來是一個很好的選擇。 再說一次，擁有一個集中的 QA 團隊是一個很大的反模式，並且不應該在一個團隊旨在真正跨職能的 DevOps 世界中佔有一席之地。 誰應該擁有端到端測試沒有簡單的答案。 也許您的組織有一個實踐社群或一個 品質公會 可以負責這些。 找到正確的答案在很大程度上取決於您的組織。

Furthermore, end-to-end tests require a lot of maintenance and run pretty slowly. Thinking about a landscape with more than a couple of microservices in place you won't even be able to run your end-to-end tests locally — as this would require to start all your microservices locally as well. Good luck spinning up hundreds of applications on your development machine without frying your RAM.

此外，端到端測試需要大量維護並且運行速度非常慢。 考慮到一個有幾個以上微服務的場景，您甚至無法在本地運行您的端到端測試——因為這也需要在本地啟動您的所有微服務。 祝您好運，在您的開發機器上啟動數百個應用程式而不會燒毀您的 RAM。

Due to their high maintenance cost you should aim to reduce the number of end-to-end tests to a bare minimum.

由於它們的高維護成本，您應該旨在將端到端測試的數量減少到最少。

Think about the high-value interactions users will have with your application. Try to come up with user journeys that define the core value of your product and translate the most important steps of these user journeys into automated end-to-end tests.

考慮一下使用者將與您的應用程式進行的有價值的互動。 嘗試提出定義您產品核心價值的用戶旅程，並將這些用戶旅程的最重要步驟轉換為自動化的端到端測試。

If you're building an e-commerce site your most valuable customer journey could be a user searching for a product, putting it in the shopping basket and doing a checkout. That's it. As long as this journey still works you shouldn't be in too much trouble. Maybe you'll find one or two more crucial user journeys that you can translate into end-to-end tests. Everything more than that will likely be more painful than helpful.

如果您正在建構一個電子商務網站，您最有價值的客戶旅程可能是使用者搜尋產品、將其放入購物籃中並進行結帳。 就是這樣。 只要這個旅程仍然有效，您就不應該遇到太多麻煩。 也許您會找到一個或兩個您可以轉換為端到端測試的更關鍵的用戶旅程。 超過這些的任何東西都可能比有幫助的更痛苦。

Remember: you have lots of lower levels in your test pyramid where you already tested all sorts of edge cases and integrations with other parts of the system. There's no need to repeat these tests on a higher level. High maintenance effort and lots of false positives will slow you down and cause you to lose trust in your tests, sooner rather than later.

請記住：您的測試金字塔中有很多較低的層級，您已經在其中測試了各種邊緣情況以及與系統其他部分的整合。 無需在更高的層級重複這些測試。 高維護成本和大量的誤報會減慢您的速度，並導致您或遲或早地失去對測試的信任。

### User Interface End-to-End Test

For end-to-end tests [Selenium](http://docs.seleniumhq.org/) and the [WebDriver](https://www.w3.org/TR/webdriver/) protocol are the tool of choice for many developers. With Selenium you can pick a browser you like and let it automatically call your website, click here and there, enter data and check that stuff changes in the user interface.

對於端到端測試，[Selenium](https://www.google.com/url?sa=E&q=http%3A%2F%2Fdocs.seleniumhq.org%2F) 和 [WebDriver](https://www.google.com/url?sa=E&q=https%3A%2F%2Fwww.w3.org%2FTR%2Fwebdriver%2F) 協議是許多開發人員的首選工具。 透過 Selenium，您可以選擇一個您喜歡的瀏覽器，並讓它自動呼叫您的網站，在這裡點擊，在那裡點擊，輸入資料並檢查使用者介面中的內容是否變更。

Selenium needs a browser that it can start and use for running its tests. There are multiple so-called _'drivers'_ for different browsers that you could use. [Pick one](https://www.mvnrepository.com/search?q=selenium+driver) (or multiple) and add it to your `build.gradle`. Whatever browser you choose, you need to make sure that all devs in your team and your CI server have installed the correct version of the browser locally. This can be pretty painful to keep in sync. For Java, there's a nice little library called [webdrivermanager](https://github.com/bonigarcia/webdrivermanager) that can automate downloading and setting up the correct version of the browser you want to use. Add these two dependencies to your `build.gradle` and you're good to go:

Selenium 需要一個可以啟動並用於運行其測試的瀏覽器。 對於您可以使用的不同瀏覽器，有多個所謂的 '驅動程式'。 [選擇一個](https://www.google.com/url?sa=E&q=https%3A%2F%2Fwww.mvnrepository.com%2Fsearch%3Fq%3Dselenium%2Bdriver)（或多個）並將其新增到您的 build.gradle 中。 無論您選擇哪個瀏覽器，您都需要確保您團隊中的所有開發人員和您的 CI 伺服器都在本地安裝了正確版本的瀏覽器。 保持同步可能非常痛苦。 對於 Java，有一個不錯的小函式庫，稱為 [webdrivermanager](https://www.google.com/url?sa=E&q=https%3A%2F%2Fgithub.com%2Fbonigarcia%2Fwebdrivermanager)，可以自動下載和設定您想要使用的瀏覽器的正確版本。 將這兩個依賴項新增到您的 build.gradle 中，您就可以開始了：

```
testCompile('org.seleniumhq.selenium:selenium-chrome-driver:2.53.1')
testCompile('io.github.bonigarcia:webdrivermanager:1.7.2')
```

Running a fully-fledged browser in your test suite can be a hassle. Especially when using continuous delivery the server running your pipeline might not be able to spin up a browser including a user interface (e.g. because there's no X-Server available). You can take a workaround for this problem by starting a virtual X-Server like [xvfb](https://en.wikipedia.org/wiki/Xvfb).

在您的測試套件中運行一個功能齊全的瀏覽器可能很麻煩。 尤其是當使用持續交付時，運行您的管道的伺服器可能無法啟動一個包含使用者介面的瀏覽器（例如，因為沒有可用的 X-Server）。 您可以透過啟動一個虛擬 X-Server（如 [xvfb](https://www.google.com/url?sa=E&q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FXvfb)）來解決這個問題。

A more recent approach is to use a _headless_ browser (i.e. a browser that doesn't have a user interface) to run your webdriver tests. Until recently [PhantomJS](http://phantomjs.org/) was the leading headless browser used for browser automation. Ever since both [Chromium](https://developers.google.com/web/updates/2017/04/headless-chrome) and [Firefox](https://developer.mozilla.org/en-US/Firefox/Headless_mode) announced that they've implemented a headless mode in their browsers PhantomJS all of a sudden became obsolete. After all it's better to test your website with a browser that your users actually use (like Firefox and Chrome) instead of using an artificial browser just because it's convenient for you as a developer.

最近的一種方法是使用 無頭 瀏覽器（即沒有使用者介面的瀏覽器）來運行您的 webdriver 測試。 直到最近，[PhantomJS](https://www.google.com/url?sa=E&q=http%3A%2F%2Fphantomjs.org%2F) 還是用於瀏覽器自動化的領先無頭瀏覽器。 自從 [Chromium](https://www.google.com/url?sa=E&q=https%3A%2F%2Fdevelopers.google.com%2Fweb%2Fupdates%2F2017%2F04%2Fheadless-chrome) 和 [Firefox](https://www.google.com/url?sa=E&q=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2FFirefox%2FHeadless_mode) 宣布它們已在其瀏覽器中實作無頭模式以來，PhantomJS 突然變得過時了。 畢竟，最好使用您的使用者實際使用的瀏覽器（如 Firefox 和 Chrome）測試您的網站，而不是僅僅因為它作為開發人員對您來說很方便而使用人工瀏覽器。

Both, headless Firefox and Chrome, are brand new and yet to be widely adopted for implementing webdriver tests. We want to keep things simple. Instead of fiddling around to use the bleeding edge headless modes let's stick to the classic way using Selenium and a regular browser. A simple end-to-end test that fires up Chrome, navigates to our service and checks the content of the website looks like this:

無頭 Firefox 和 Chrome 都是全新的，尚未被廣泛採用來實作 webdriver 測試。 我們希望讓事情簡單化。 與其四處摸索以使用最前沿的無頭模式，不如讓我們堅持使用 Selenium 和常規瀏覽器的經典方法。 一個啟動 Chrome、導航到我們服務並檢查網站內容的簡單端到端測試如下所示：

```java
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class HelloE2ESeleniumTest {

    private WebDriver driver;

    @LocalServerPort
    private int port;

    @BeforeClass
    public static void setUpClass() throws Exception {
        ChromeDriverManager.getInstance().setup();
    }

    @Before
    public void setUp() throws Exception {
        driver = new ChromeDriver();
    }

    @After
    public void tearDown() {
        driver.close();
    }

    @Test
    public void helloPageHasTextHelloWorld() {
        driver.get(String.format("http://127.0.0.1:%s/hello", port));

        assertThat(driver.findElement(By.tagName("body")).getText(), containsString("Hello World!"));
    }
}
```

Note that this test will only run on your system if you have Chrome installed on the system you run this test on (your local machine, your CI server).

請注意，只有在您在運行此測試的系統（您的本地機器、您的 CI 伺服器）上安裝了 Chrome 後，此測試才能在您的系統上運行。

The test is straightforward. It spins up the entire Spring application on a random port using `@SpringBootTest`. We then instantiate a new Chrome webdriver, tell it to go navigate to the `/hello` endpoint of our microservice and check that it prints “Hello World!” on the browser window. Cool stuff!

該測試很簡單。 它使用 @SpringBootTest 在隨機端口上啟動整個 Spring 應用程式。 然後我們實例化一個新的 Chrome webdriver，告訴它導航到我們微服務的 /hello 端點，並檢查它是否在瀏覽器視窗上列印“Hello World！”。 酷的東西！

### REST API End-to-End Test

Avoiding a graphical user interface when testing your application can be a good idea to come up with tests that are less flaky than full end-to-end tests while still covering a broad part of your application's stack. This can come in handy when testing through the web interface of your application is particularly hard. Maybe you don't even have a web UI but serve a REST API instead (because you have a single page application somewhere talking to that API, or simply because you despise everything that's nice and shiny). Either way, a [Subcutaneous Test](https://martinfowler.com/bliki/SubcutaneousTest.html) that tests just beneath the graphical user interface and can get you really far without compromising on confidence too much. Just the right thing if you're serving a REST API like we do in our example code:

在測試應用程式時避免使用圖形使用者介面可能是一個好主意，可以提出比完整端到端測試更穩定的測試，同時仍然涵蓋應用程式堆疊的廣泛部分。 當透過應用程式的 Web 介面進行測試特別困難時，這可能會派上用場。 也許您甚至沒有 Web UI，而是提供 REST API（因為您在某處有一個與該 API 通訊的單頁應用程式，或者只是因為您鄙視所有美好而有光澤的東西）。 無論如何，一個 [皮下測試](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Fbliki%2FSubcutaneousTest.html) 僅在圖形使用者介面下方進行測試，並且可以在不太多地損害信心的情況下讓您走得很遠。 如果您像我們在範例程式碼中所做的那樣提供 REST API，那就太棒了：

```java
@RestController
public class ExampleController {
    private final PersonRepository personRepository;

    // shortened for clarity

    @GetMapping("/hello/{lastName}")
    public String hello(@PathVariable final String lastName) {
        Optional&lt;Person&gt; foundPerson = personRepository.findByLastName(lastName);

        return foundPerson
             .map(person -&gt; String.format("Hello %s %s!",
                     person.getFirstName(),
                     person.getLastName()))
             .orElse(String.format("Who is this '%s' you're talking about?",
                     lastName));
    }
}
```

Let me show you one more library that comes in handy when testing a service that provides a REST API. [REST-assured](https://github.com/rest-assured/rest-assured) is a library that gives you a nice DSL for firing real HTTP requests against an API and evaluating the responses you receive.

讓我向您展示另一個函式庫，它在測試提供 REST API 的服務時會派上用場。 [REST-assured](https://www.google.com/url?sa=E&q=https%3A%2F%2Fgithub.com%2Frest-assured%2Frest-assured) 是一個函式庫，它為您提供了一個不錯的 DSL，用於針對 API 發送真實的 HTTP 請求並評估您收到的回應。

First things first: Add the dependency to your `build.gradle`.

首先：將依賴項新增到您的 build.gradle 中。

```
testCompile('io.rest-assured:rest-assured:3.0.3')
```

With this library at our hands we can implement an end-to-end test for our REST API:

有了這個函式庫，我們可以為我們的 REST API 實作一個端到端測試：

```
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class HelloE2ERestTest {

    @Autowired
    private PersonRepository personRepository;

    @LocalServerPort
    private int port;

    @After
    public void tearDown() throws Exception {
        personRepository.deleteAll();
    }

    @Test
    public void shouldReturnGreeting() throws Exception {
        Person peter = new Person("Peter", "Pan");
        personRepository.save(peter);

        when()
                .get(String.format("http://localhost:%s/hello/Pan", port))
        .then()
                .statusCode(is(200))
                .body(containsString("Hello Peter Pan!"));
    }
}
```

Again, we start the entire Spring application using `@SpringBootTest`. In this case we `@Autowire` the `PersonRepository` so that we can write test data into our database easily. When we now ask the REST API to say “hello” to our friend “Mr Pan” we're being presented with a nice greeting. Amazing! And more than enough of an end-to-end test if you don't even sport a web interface.

再次，我們使用 @SpringBootTest 啟動整個 Spring 應用程式。 在這種情況下，我們 @Autowire PersonRepository，以便我們可以輕鬆地將測試資料寫入我們的資料庫。 當我們現在要求 REST API 向我們的朋友“Pan 先生”說“hello”時，我們將會收到一個友好的問候語。 驚人的！ 如果您甚至沒有 Web 介面，這就足以進行端到端測試了。

## Acceptance Tests — Do Your Features Work Correctly?

The higher you move up in your test pyramid the more likely you enter the realms of testing whether the features you're building work correctly from a user's perspective. You can treat your application as a black box and shift the focus in your tests from

您在測試金字塔中移動得越高，就越有可能進入從使用者的角度測試您正在建構的功能是否正常運作的領域。 您可以將您的應用程式視為一個黑盒，並將測試重點從

when I enter the values `x` and `y`, the return value should be `z`
當我輸入值 x 和 y 時，傳回值應為 z

towards
轉向

_given_ there's a logged in user
given 有一個已登入的使用者

_and_ there's an article “bicycle”
and 有一篇文章“bicycle”

_when_ the user navigates to the “bicycle” article's detail page
when 使用者導航到“bicycle”文章的詳細資訊頁面

_and_ clicks the “add to basket” button
and 點擊“新增到購物籃”按鈕

_then_ the article “bicycle” should be in their shopping basket
then 文章“bicycle”應該在他們的購物籃中

Sometimes you'll hear the terms [**functional test**](https://en.wikipedia.org/wiki/Functional_testing) or [**acceptance test**](https://en.wikipedia.org/wiki/Acceptance_testing#Acceptance_testing_in_extreme_programming) for these kinds of tests. Sometimes people will tell you that functional and acceptance tests are different things. Sometimes the terms are conflated. Sometimes people will argue endlessly about wording and definitions. Often this discussion is a pretty big source of confusion.

有時您會聽到 [**功能測試**](https://www.google.com/url?sa=E&q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FFunctional_testing) 或 [**驗收測試**](https://www.google.com/url?sa=E&q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FAcceptance_testing%23Acceptance_testing_in_extreme_programming) 這些術語用於此類測試。 有時人們會告訴您功能測試和驗收測試是不同的東西。 有時這些術語會被混淆。 有時人們會無休止地爭論措辭和定義。 通常這種討論是一個相當大的混亂來源。

Here's the thing: At one point you should make sure to test that your software works correctly from a _user's_ perspective, not just from a technical perspective. What you call these tests is really not that important. Having these tests, however, is. Pick a term, stick to it, and write those tests.

事情是這樣的：在某個時候，您應該確保從 使用者 的角度而不是僅從技術的角度來測試您的軟體是否正常運作。 您如何稱呼這些測試實際上並不重要。 但是，擁有這些測試很重要。 選擇一個術語，堅持使用它，然後編寫這些測試。

This is also the moment where people talk about BDD and tools that allow you to implement tests in a BDD fashion. BDD or a BDD-style way of writing tests can be a nice trick to shift your mindset from implementation details towards the users' needs. Go ahead and give it a try.

這也是人們談論 BDD 和允許您以 BDD 方式實作測試的時刻。 BDD 或 BDD 風格的編寫測試的方式可能是一個不錯的技巧，可以將您的心態從實作細節轉向使用者的需求。 繼續前進並嘗試一下。

You don't even need to adopt full-blown BDD tools like [Cucumber](https://cucumber.io/) (though you can). Some assertion libraries (like [chai.js](http://chaijs.com/guide/styles/#should) allow you to write assertions with `should`\-style keywords that can make your tests read more BDD-like. And even if you don't use a library that provides this notation, clever and well-factored code will allow you to write user behaviour focused tests. Some helper methods/functions can get you a very long way:

您甚至不需要採用像 [Cucumber](https://www.google.com/url?sa=E&q=https%3A%2F%2Fcucumber.io%2F) 這樣成熟的 BDD 工具（儘管您可以）。 一些斷言函式庫（如 [chai.js](https://www.google.com/url?sa=E&q=http%3A%2F%2Fchaijs.com%2Fguide%2Fstyles%2F%23should) 允許您使用 should 風格的關鍵字編寫斷言，這可以使您的測試讀起來更像 BDD。 即使您不使用提供此表示法的函式庫，聰明且經過良好分解的程式碼也可以讓您編寫以使用者行為為中心的測試。 一些助手方法/函式可以讓您走得很遠：

```java
# a sample acceptance test in Python

def test_add_to_basket():
    # given
    user = a_user_with_empty_basket()
    user.login()
    bicycle = article(name="bicycle", price=100)

    # when
    article_page.add_to_.basket(bicycle)

    # then
    assert user.basket.contains(bicycle)
```

Acceptance tests can come in different levels of granularity. Most of the time they will be rather high-level and test your service through the user interface. However, it's good to understand that there's technically no need to write acceptance tests at the highest level of your test pyramid. If your application design and your scenario at hand permits that you write an acceptance test at a lower level, go for it. Having a low-level test is better than having a high-level test. The concept of acceptance tests - proving that your features work correctly for the user - is completely orthogonal to your test pyramid.

驗收測試可以有不同的粒度層級。 大多數時候，它們將會相當高層級，並透過使用者介面測試您的服務。 但是，很高興了解從技術上講，沒有必要在測試金字塔的最高層級編寫驗收測試。 如果您的應用程式設計和手頭的場景允許您在較低的層級編寫驗收測試，那就去做吧。 擁有低層級測試比擁有高層級測試更好。 驗收測試的概念（證明您的功能對使用者來說正常運作）與您的測試金字塔完全正交。

## Exploratory Testing

Even the most diligent test automation efforts are not perfect. Sometimes you miss certain edge cases in your automated tests. Sometimes it's nearly impossible to detect a particular bug by writing a unit test. Certain quality issues don't even become apparent within your automated tests (think about design or usability). Despite your best intentions with regards to test automation, manual testing of some sorts is still a good idea.

即使是最勤奮的測試自動化工作也不是完美的。 有時您會在自動化測試中遺漏某些邊緣情況。 有時幾乎不可能透過編寫單元測試來檢測特定錯誤。 某些品質問題甚至不會在您的自動化測試中變得明顯（考慮設計或可用性）。 儘管您對測試自動化抱有最好的意圖，但某些形式的手動測試仍然是一個好主意。


![](https://martinfowler.com/articles/practical-test-pyramid/exploratoryTesting.png)

Figure 12: Use exploratory testing to spot all quality issues that your build pipeline didn't spot 使用探索性測試來發現您的建置管道沒有發現的所有品質問題

Include [Exploratory Testing](https://en.wikipedia.org/wiki/Exploratory_testing) in your testing portfolio. It is a manual testing approach that emphasises the tester's freedom and creativity to spot quality issues in a running system. Simply take some time on a regular schedule, roll up your sleeves and try to break your application. Use a destructive mindset and come up with ways to provoke issues and errors in your application. Document everything you find for later. Watch out for bugs, design issues, slow response times, missing or misleading error messages and everything else that would annoy you as a user of your software.

將 [探索性測試](https://www.google.com/url?sa=E&q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FExploratory_testing) 納入您的測試組合中。 這是一種手動測試方法，強調測試人員的自由和創造力，以發現運行系統中的品質問題。 只需定期花一些時間，捲起袖子並嘗試破壞您的應用程式。 使用破壞性的心態，並提出在您的應用程式中引發問題和錯誤的方法。 記錄您稍後發現的所有內容。 注意錯誤、設計問題、響應時間慢、遺失或具有誤導性的錯誤訊息以及任何其他會讓您作為軟體使用者感到惱火的事情。

The good news is that you can happily automate most of your findings with automated tests. Writing automated tests for the bugs you spot makes sure there won't be any regressions of that bug in the future. Plus it helps you narrowing down the root cause of that issue during bugfixing.

好消息是您可以很高興地透過自動化測試來自動化您的大部分發現。 為您發現的錯誤編寫自動化測試可確保將來不會再出現該錯誤的回歸。 此外，它還有助於您在錯誤修復期間縮小該問題的根本原因。

During exploratory testing you will spot problems that slipped through your build pipeline unnoticed. Don't be frustrated. This is great feedback on the maturity of your build pipeline. As with any feedback, make sure to act on it: Think about what you can do to avoid these kinds of problems in the future. Maybe you're missing out on a certain set of automated tests. Maybe you have just been sloppy with your automated tests in this iteration and need to test more thoroughly in the future. Maybe there's a shiny new tool or approach that you could use in your pipeline to avoid these issues in the future. Make sure to act on it so your pipeline and your entire software delivery will grow more mature the longer you go.

在探索性測試期間，您會發現沒有被您的建置管道注意到的問題。 不要感到沮喪。 這是對您的建置管道成熟度的好反饋。 與任何反饋一樣，請務必採取行動：考慮您可以做些什麼來避免將來出現這些類型的問題。 也許您缺少一組特定的自動化測試。 也許您只是在這次迭代中對您的自動化測試馬虎了，並且需要在將來更徹底地測試。 也許有一個閃亮的新工具或方法，您可以在您的管道中使用，以避免將來出現這些問題。 務必採取行動，以便您的管道和您的整個軟體交付將隨著時間的推移而變得更加成熟。

## The Confusion About Testing Terminology

Talking about different test classifications is always difficult. What I mean when I talk about _unit tests_ can be slightly different from your understanding. With integration tests it's even worse. For some people integration testing is a very broad activity that tests through a lot of different parts of your entire system. For me it's a rather narrow thing, only testing the integration with one external part at a time. Some call them _integration tests_, some refer to them as _component tests_, some prefer the term _service test_. Even others will argue, that all of these three terms are totally different things. There's no right or wrong. The software development community simply hasn't managed to settle on well-defined terms around testing.

談論不同的測試分類總是困難的。 當我談論 單元測試 時的含義可能與您的理解略有不同。 對於整合測試來說，情況甚至更糟。 對於某些人來說，整合測試是一項非常廣泛的活動，它透過您的整個系統的許多不同部分進行測試。 對我來說，這是一個相當狹隘的事情，一次只測試與一個外部部分的整合。 有些人稱它們為 整合測試，有些人將它們稱為 元件測試，有些人則更喜歡術語 服務測試。 甚至其他人會爭辯說，所有這三個術語都是完全不同的東西。 沒有對與錯。 軟體開發社群只是沒有設法就圍繞測試的定義明確的術語達成一致。

Don't get too hung up on sticking to ambiguous terms. It doesn't matter if you call it end-to-end or broad stack test or functional test. It doesn't matter if your integration tests mean something different to you than to the folks at another company. Yes, it would be really nice if our profession could settle on some well-defined terms and all stick to it. Unfortunately this hasn't happened yet. And since there are many nuances when it comes to writing tests it's really more of a spectrum than a bunch of discrete buckets anyways, which makes consistent naming even harder.

不要過於執著於堅持使用含糊不清的術語。 您是否將其稱為端到端或廣堆疊測試或功能測試都無關緊要。 您的整合測試對您來說是否與另一家公司的人員對您來說意味著不同的意義也無關緊要。 是的，如果我們的專業能夠就一些定義明確的術語達成一致，並且我們都堅持使用它們，那將會非常好。 不幸的是，這種情況尚未發生。 並且由於在編寫測試方面存在許多細微的差別，因此無論如何它實際上更多的是一個頻譜而不是一堆離散的桶，這使得一致的命名更加困難。

The important takeaway is that you should find terms that work for you and your team. Be clear about the different types of tests that you want to write. Agree on the naming in your team and find consensus on the scope of each type of test. If you get this consistent within your team (or maybe even within your organisation) that's really all you should care about. [Simon Stewart](https://testing.googleblog.com/2010/12/test-sizes.html) summed this up very nicely when he described the approach they use at Google. And I think it shows perfectly how getting too hung up on names and naming conventions just isn't worth the hassle.

重要的收穫是您應該找到適用於您和您的團隊的術語。 清楚您要編寫的不同類型的測試。 在您的團隊中就命名達成一致，並就每種類型測試的範圍達成共識。 如果您在您的團隊（或者甚至在您的組織中）內保持一致，那麼這就是您真正應該關心的全部。 [Simon Stewart](https://www.google.com/url?sa=E&q=https%3A%2F%2Ftesting.googleblog.com%2F2010%2F12%2Ftest-sizes.html) 在他描述了他們在 Google 使用的方法時，非常出色地總結了這一點。 而且我認為這完美地展示了過於執著於名稱和命名約定根本不值得麻煩。

## Putting Tests Into Your Deployment Pipeline

If you're using Continuous Integration or Continuous Delivery, you'll have a [Deployment Pipeline](https://martinfowler.com/bliki/DeploymentPipeline.html) in place that will run automated tests every time you make a change to your software. Usually this pipeline is split into several stages that gradually give you more confidence that your software is ready to be deployed to production. Hearing about all these different kinds of tests you're probably wondering how you should place them within your deployment pipeline. To answer this you should just think about one of the very foundational values of Continuous Delivery (indeed one of the core [values of Extreme Programming](http://www.extremeprogramming.org/values.html) and agile software development): **Fast Feedback**.

如果您使用持續整合或持續交付，那麼您將有一個 [部署管道](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2Fbliki%2FDeploymentPipeline.html)，每次您對您的軟體進行變更時，它都會運行自動化測試。 通常，這個管道被分成幾個階段，逐漸讓您更有信心您的軟體已準備好部署到生產環境。 聽到所有這些不同種類的測試，您可能想知道應該如何將它們放入您的部署管道中。 要回答這個問題，您應該只考慮持續交付的最基本價值觀之一（實際上是 [極限編程](https://www.google.com/url?sa=E&q=http%3A%2F%2Fwww.extremeprogramming.org%2Fvalues.html) 和敏捷軟體開發的核心 [價值觀](https://www.google.com/url?sa=E&q=http%3A%2F%2Fwww.extremeprogramming.org%2Fvalues.html) 之一）：**快速反饋**。

A good build pipeline tells you that you messed up as quick as possible. You don't want to wait an hour just to find out that your latest change broke some simple unit tests. Chances are that you've probably gone home already if your pipeline takes that long to give you that feedback. You could get this information within a matter of seconds, maybe a few minutes by putting the fast running tests in the earlier stages of your pipeline. Conversely you put the longer running tests - usually the ones with a broader scope - in the later stages to not defer the feedback from the fast-running tests. You see that defining the stages of your deployment pipeline is not driven by the types of tests but rather by their speed and scope. With that in mind it can be a very reasonable decision to put some of the really narrowly-scoped and fast-running integration tests in the same stage as your unit tests - simply because they give you faster feedback and not because you want to draw the line along the formal type of your tests.

一個好的建置管道會盡快告訴您您搞砸了。 您不想等待一個小時才發現您最新的變更破壞了一些簡單的單元測試。 如果您的管道需要這麼長時間才能給您那種反饋，那麼您很可能已經回家了。 透過將快速運行的測試放在管道的早期階段，您可以在幾秒鐘內，也許幾分鐘內獲得此資訊。 相反，您將運行時間更長的測試（通常是範圍更廣的測試）放在後面的階段，以免延遲來自快速運行測試的反饋。 您會看到，定義部署管道的階段不是由測試的類型驅動的，而是由它們的速度和範圍驅動的。 牢記這一點，將一些範圍非常狹窄且快速運行的整合測試與單元測試放在同一個階段可能是一個非常合理的決定，僅僅因為它們給您更快的反饋，而不是因為您想根據測試的正式類型劃清界線。

## Avoid Test Duplication

Now that you know that you should write different types of tests there's one more pitfall to avoid: duplicating tests throughout the different layers of the pyramid. While your gut feeling might say that there's no such thing as too many tests let me assure you, there is. Every single test in your test suite is additional baggage and doesn't come for free. Writing and maintaining tests takes time. Reading and understanding other people's test takes time. And of course, running tests takes time.

現在您知道您應該編寫不同類型的測試，還有一個要避免的陷阱：在金字塔的不同層級重複測試。 儘管您的直覺可能會說沒有太多的測試，但請讓我向您保證，確實有。 您測試套件中的每個測試都是額外的負擔，而且不是免費的。 編寫和維護測試需要時間。 閱讀和理解其他人的測試需要時間。 當然，運行測試需要時間。

As with production code you should strive for simplicity and avoid duplication. In the context of implementing your test pyramid you should keep two rules of thumb in mind:

與生產程式碼一樣，您應該努力實現簡單並避免重複。 在實作您的測試金字塔的上下文中，您應該牢記兩個經驗法則：

1.  If a higher-level test spots an error and there's no lower-level test failing, you need to write a lower-level test 如果一個較高層級的測試發現了一個錯誤並且沒有較低層級的測試失敗，那麼您需要編寫一個較低層級的測試
2.  Push your tests as far down the test pyramid as you can 將您的測試盡可能地推到測試金字塔的底部

The first rule is important because lower-level tests allow you to better narrow down errors and replicate them in an isolated way. They'll run faster and will be less bloated when you're debugging the issue at hand. And they will serve as a good regression test for the future. The second rule is important to keep your test suite fast. If you have tested all conditions confidently on a lower-level test, there's no need to keep a higher-level test in your test suite. It just doesn't add more confidence that everything's working. Having redundant tests will become annoying in your daily work. Your test suite will be slower and you need to change more tests when you change the behaviour of your code.

第一個規則很重要，因為較低層級的測試允許您更好地縮小錯誤範圍，並以隔離的方式複製它們。 當您除錯手頭的問題時，它們將運行得更快並且膨脹更少。 它們將作為將來的良好迴歸測試。 第二個規則對於保持您的測試套件快速很重要。 如果您已經在較低層級的測試中自信地測試了所有條件，則無需在您的測試套件中保留較高層級的測試。 它只是沒有增加更多的信心一切正常。 在您的日常工作中，擁有冗餘測試會變得令人惱火。 您的測試套件會變慢，並且當您更改程式碼的行為時，您需要更改更多測試。

Let's phrase this differently: If a higher-level test gives you more confidence that your application works correctly, you should have it. Writing a unit test for a `Controller` class helps to test the logic within the Controller itself. Still, this won't tell you whether the REST endpoint this Controller provides actually responds to HTTP requests. So you move up the test pyramid and add a test that checks for exactly that - but nothing more. You don't test all the conditional logic and edge cases that your lower-level tests already cover in the higher-level test again. Make sure that the higher-level test focuses on the part that the lower-level tests couldn't cover.

讓我們以不同的方式表達這一點：如果一個較高層級的測試讓您更有信心您的應用程式正常運作，那麼您應該擁有它。 為 Controller 類別編寫單元測試有助於測試 Controller 本身中的邏輯。 儘管如此，這不會告訴您此 Controller 提供的 REST 端點是否實際響應 HTTP 請求。 因此，您向上移動測試金字塔並新增一個測試，以檢查確切的內容 - 但僅此而已。 您不會再次在較高層級的測試中測試您的較低層級測試已經涵蓋的所有條件邏輯和邊緣情況。 確保較高層級的測試側重於較低層級測試無法涵蓋的部分。

I'm rigorous when it comes to eliminating tests that don't provide any value. I delete high-level tests that are already covered on a lower level (given they don't provide extra value). I replace higher-level tests with lower-level tests if possible. Sometimes that's hard, especially if you know that coming up with a test was hard work. Beware of the sunk cost fallacy and hit the delete key. There's no reason to waste more precious time on a test that ceased to provide value.

在消除不提供任何價值的測試方面，我很嚴格。 我刪除了較低層級已經涵蓋的高層級測試（假設它們沒有提供額外的價值）。 如果可能，我用較低層級的測試替換較高層級的測試。 有時這很困難，特別是如果您知道提出一個測試是一項艱苦的工作。 請注意沉沒成本謬誤並按下刪除鍵。 沒有理由在不再提供價值的測試上浪費更多寶貴的時間。

## Writing Clean Test Code

As with writing code in general, coming up with good and clean test code takes great care. Here are some more hints for coming up with maintainable test code before you go ahead and hack away on your automated test suite:

與編寫程式碼一樣，提出良好而乾淨的測試程式碼需要非常小心。 在您繼續破解您的自動化測試套件之前，這裡有一些關於提出可維護測試程式碼的更多提示：

1.  Test code is as important as production code. Give it the same level of care and attention. _“this is only test code”_ is not a valid excuse to justify sloppy code 測試程式碼與生產程式碼一樣重要。 給予它相同的關心和關注。 “這只是測試程式碼” 不是為草率的程式碼辯解的有效藉口
2.  Test one condition per test. This helps you to keep your tests short and easy to reason about 每個測試測試一個條件。 這有助於您保持測試簡短且易於推理
3.  _“arrange, act, assert”_ or _“given, when, then”_ are good mnemonics to keep your tests well-structured "arrange, act, assert" 或 "given, when, then" 是保持您的測試結構良好的好助記符
4.  Readability matters. Don't try to be overly DRY . Duplication is okay, if it improves readability. Try to find a balance between [DRY and DAMP](https://stackoverflow.com/questions/6453235/what-does-damp-not-dry-mean-when-talking-about-unit-tests) code 可讀性很重要。 不要試圖過於 DRY 。 如果它提高了可讀性，那麼重複是可以的。 嘗試在 [DRY 和 DAMP](https://www.google.com/url?sa=E&q=https%3A%2F%2Fstackoverflow.com%2Fquestions%2F6453235%2Fwhat-does-damp-not-dry-mean-when-talking-about-unit-tests) 程式碼之間找到平衡
5.  When in doubt use the [Rule of Three](https://blog.codinghorror.com/rule-of-three/) to decide when to refactor. _Use before reuse_ 如有疑問，請使用 [三法則](https://www.google.com/url?sa=E&q=https%3A%2F%2Fblog.codinghorror.com%2Frule-of-three%2F) 來決定何時重構。 先使用後重用

## Conclusion

That's it! I know this was a long and tough read to explain why and how you should test your software. The great news is that this information is pretty timeless and independent of what kind of software you're building. It doesn't matter if you're working on a microservices landscape, IoT devices, mobile apps or web applications, the lessons from this article can be applied to all of these.

就是這樣！ 我知道這是一篇冗長而艱難的閱讀，解釋了為什麼以及如何測試您的軟體。 好消息是，這些資訊是相當永恆的，並且獨立於您正在建構的軟體的種類。 無論您是在微服務環境、物聯網設備、行動應用程式還是 Web 應用程式上工作，本文中的經驗都可以應用於所有這些。

I hope that there's something useful in this article. Now go ahead and check out the [sample code](https://github.com/hamvocke/spring-testing) and get some of the concepts explained here into your testing portfolio. Having a solid test portfolio takes some effort. It will pay off in the longer term and it will make your live as a developer more peaceful, trust me.

我希望這篇文章中有些有用的東西。 現在繼續檢查 [範例程式碼](https://www.google.com/url?sa=E&q=https%3A%2F%2Fgithub.com%2Fhamvocke%2Fspring-testing)，並將本文中解釋的一些概念納入您的測試組合中。 擁有可靠的測試組合需要付出一些努力。 從長遠來看，它將會得到回報，並且它會使您作為開發人員的生活更加平靜，請相信我。
