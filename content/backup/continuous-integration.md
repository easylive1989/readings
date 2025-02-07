---
date: '2025-02-07T17:04:50+08:00'
title: 'Continuous Integration'
tags: ["Marting Fowler"]
---

[Source](https://www.martinfowler.com/articles/continuousIntegration.html)

I vividly remember one of my first sightings of a large software project. I was taking a summer internship at a large English electronics company. My manager, part of the QA group, gave me a tour of a site and we entered a huge, depressing, windowless warehouse full of people working in cubicles. I was told that these programmers had been writing code for this software for a couple of years, and while they were done programming, their separate units were now being integrated together, and they had been integrating for several months. My guide told me that nobody really knew how long it would take to finish integrating. From this I learned a common story of software projects: integrating the work 
of multiple developers is a long and unpredictable process.

我清楚地記得第一次目睹一個大型軟件項目的情景。當時，我在英國一家大型電子公司實習，負責質量保證（QA）部門的經理帶我參觀了一個現場。我們走進了一個巨大的、令人沮喪的無窗倉庫，裡面全是工作在隔間裡的程序員。經理告訴我，這些程序員已經編寫這個軟件的代碼好幾年了，雖然他們的編程工作已經完成，但現在他們各自的單元正在被集成，而且已經集成了好幾個月。我們的導遊還說，沒有人真正知道這個集成工作還需要多久才能完成。從這次經歷中，我學到了一個軟件項目中的常見問題：多名開發人員的工作集成是一個漫長且不可預測的過程。

I haven't heard of a team trapped in such a long integration like this for many years, but that doesn't mean that integration is a painless process. A developer may have been working for several days on a new feature, regularly pulling changes from a common main branch into her feature branch. Just before she's ready to push her changes, a big change lands on main, one that alters some code that she's interacting with. She has to change from finishing off her feature to figuring out how to integrate her work with this change, which while better for her colleague, doesn't work so well for her. Hopefully the complexities of the change will be in merging the source code, not an insidious fault that only shows when she runs the application, forcing her to debug unfamiliar code.

我已經很多年沒聽說過有團隊被困在如此漫長的集成過程中了，但這並不意味著集成就沒有痛點。假設有位開發人員已經花了幾天時間在開發一個新功能，期間她經常從公共主分支拉取變更到自己的功能分支。在她準備推送自己的變更之前，主分支上發生了一個大的變動，這個變動改動了她正在使用的一些代碼。她不得不從完成功能轉向解決如何將她的工作與這個變更集成，雖然這對她的同事來說是個改進，但對她卻不那麼友好。希望這次變更的複雜性只在合併源代碼上，而不是在運行應用程式時出現隱藏的錯誤，否則她將不得不調試不熟悉的代碼。

At least in that scenario, she gets to find out before she submits her pull request. Pull requests can be fraught enough while waiting for someone to review a change. The review can take time, forcing her to context-switch from her next feature. A difficult integration during that period can be very disconcerting, dragging out the review process even longer. And that may not even the be the end of story, since integration tests are often only run after the pull request is merged.

至少在那種情況下，她能在提交 pull request 之前發現問題。等待其他人審查變更的過程中，pull request 本身就可能充滿挑戰。審查需要時間，迫使她在下個功能的開發過程中進行上下文切換。如果在這段時間內發生困難的集成問題，可能會讓審查過程拖得更長。而這還不一定是故事的結局，因為集成測試通常只有在 pull request 合併後才會進行。

In time, this team may learn that making significant changes to core code causes this kind of problem, and thus stops doing it. But that, by preventing regular refactoring, ends up allowing cruft to grow throughout the codebase. Folks who encounter a crufty code base wonder how it got into such a state, and often the answer lies in an integration process with so much friction that it discourages people from removing that cruft.

隨著時間推移，這個團隊可能會意識到對核心代碼進行重大更改會導致這類問題，因此會停止這樣做。然而，這樣的做法會阻止定期重構，最終導致代碼庫中的冗餘和混亂逐漸累積。遇到這種混亂的代碼庫時，人們往往會想知道它是怎麼變成這樣的，通常答案就在於集成過程中存在太多阻力，讓開發者不願意清理這些混亂的代碼。

But this needn't be the way. Most projects done by my colleagues at Thoughtworks, and by many others around the world, treat integration as a non-event. Any individual developer's work is only a few hours away from a shared project state and can be integrated back into that state in minutes. Any integration errors are found rapidly and can be fixed rapidly.

但事情不一定非得如此。我的 Thoughtworks 同事們以及世界各地的許多其他團隊所執行的項目，都將集成視為一件不值一提的小事。任何開發者的工作只需幾個小時就能與共享的項目狀態同步，並且可以在幾分鐘內重新集成回那個狀態。集成錯誤能夠被迅速發現並迅速修復。

This contrast isn't the result of an expensive and complex tool. The essence of it lies in the simple practice of everyone on the team integrating frequently, at least daily, against a controlled source code repository. This practice is called “Continuous Integration” (or [in some circles](https://www.martinfowler.com/articles/continuousIntegration.html#trunk-based) it’s called “Trunk-Based Development”).

這種對比並不是來自於昂貴且複雜的工具。其核心在於團隊中每個人經常集成他們的工作，至少每天一次，並與受控的源代碼倉庫進行同步。這種做法被稱為「持續集成」（在某些圈子裡也被稱為「主幹式開發」）。

In this article, I explain what Continuous Integration is and how to do it well. I've written it for two reasons. Firstly there are always new people coming into the profession and I want to show them how they can avoid that depressing warehouse. But secondly this topic needs clarity because Continuous Integration is a much misunderstood concept. There are many people who say that they are doing Continuous Integration, but once they describe their workflow, it becomes clear that they are missing important pieces. A clear understanding of Continuous Integration helps us communicate, so we know what to expect when we describe our way of working. It also helps folks realize that there are further things they can do to improve their experience.

在這篇文章中，我解釋了什麼是持續集成以及如何做好它。我寫這篇文章有兩個原因。首先，總是有新的人進入這個行業，我想向他們展示如何避免那種令人沮喪的「倉庫」情景。其次，這個話題需要澄清，因為持續集成是一個被廣泛誤解的概念。有很多人聲稱他們在進行持續集成，但當他們描述他們的工作流程時，很明顯他們缺少了一些重要的環節。對持續集成的清晰理解有助於我們溝通，確保我們能夠理解彼此的工作方式，並且讓人們意識到還有許多可以改進的地方，以提升他們的工作體驗。

I originally wrote this article in 2001, with an update in 2006. Since then much has changed in usual expectations of software development teams. The many-month integration that I saw in the 1980s is a distant memory, technologies such as version control and build scripts have become commonplace. I rewrote this article again in 2023 to better address the development teams of that time, with twenty years of experience to confirm the value of Continuous Integration.

我最初在2001年撰寫了這篇文章，並於2006年進行了一次更新。自那時以來，軟體開發團隊的常規期望發生了許多變化。我在1980年代見到的長達數月的整合過程已成為遙遠的記憶，版本控制和構建腳本等技術已經變得司空見慣。我在2023年再次重寫這篇文章，以更好地針對當時的開發團隊，並用這二十年來的經驗來證實持續集成的價值。

## Building a Feature with Continuous Integration

The easiest way for me to explain what Continuous Integration is and how it works is to show a quick example of how it works with the development of a small feature. I'm currently working with a major manufacturer of magic potions, we are extending their product quality system to calculate how long the potion's effect will last. We already have a dozen potions supported in the system, and we need to extend the logic for flying potions. (We've learned that having them wear off too early severely impacts customer retention.) Flying potions introduce a few new factors to take care of, one of which is the moon phase during secondary mixing.

對我來說，最簡單解釋什麼是持續集成以及它如何運作的方法，就是通過開發一個小功能的實例來展示它的工作方式。目前我正在與一家大型魔藥製造商合作，我們正在擴展他們的產品質量系統，以計算魔藥效果持續的時間。我們的系統已經支持了十幾種魔藥，現在需要擴展飛行魔藥的邏輯。（我們發現，飛行魔藥效果過早失效會嚴重影響客戶留存率。）飛行魔藥引入了一些新因素需要考慮，其中之一是在二次混合時的月相。

I begin by taking a copy of the latest product sources onto my local development environment. I do this by checking out the current mainline from the central repository with `git pull`.

我首先將最新的產品源代碼複製到本地開發環境中。我通過使用 `git pull` 從中央代碼庫檢出當前的主線來完成這一步。

Once the source is in my environment, I execute a command to build the product. This command checks that my environment is set up correctly, does any compilation of the sources into an executable product, starts the product, and runs a comprehensive suite of tests against it. This should take only a few minutes, while I start poking around the code to decide how to begin adding the new feature. This build hardly ever fails, but I do it just in case, because if it does fail, I want to know before I start making changes. If I make changes on top of a failing build, I'll get confused thinking it was my changes that caused the failure.

一旦源代碼在我的環境中，我會執行一個命令來構建產品。這個命令會檢查我的環境是否設置正確，將源代碼編譯成可執行的產品，啟動產品，並對其運行一個全面的測試套件。這應該只需要幾分鐘的時間，這期間我會瀏覽代碼，決定如何開始添加新功能。這個構建過程很少會失敗，但我還是會進行這一步，以防萬一，因為如果構建失敗，我希望在開始修改之前就知道原因。如果我在一個已經失敗的構建上進行更改，可能會混淆，以為是我的更改導致了失敗。

Now I take my working copy and do whatever I need to do to deal with the moon phases. This will consist of both altering the product code, and also adding or changing some of the automated tests. During that time I run the automated build and tests frequently. After an hour or so I have the moon logic incorporated and tests updated.

接下來，我在我的工作副本中進行所需的更改，以處理月相問題。這包括修改產品代碼以及添加或更改一些自動化測試。在此過程中，我會頻繁地運行自動化構建和測試。經過大約一小時後，我已將月相邏輯整合進去並更新了測試。

I'm now ready to integrate my changes back into the central repository. My first step for this is to pull again, because it's possible, indeed likely, that my colleagues will have pushed changes into the mainline while I've been working. Indeed there are a couple of such changes, which I pull into my working copy. I combine my changes on top of them and run the build again. Usually this feels superfluous, but this time a test fails. The test gives me some clue about what's gone wrong, but I find it more useful to look at the commits that I pulled to see what changed. It seems that someone has made an adjustment to a function, moving some of its logic out into its callers. They fixed all the callers in the mainline code, but I added a new call in my changes that, of course, they couldn't see yet. I make the same adjustment and rerun the build, which passes this time.

現在我準備將我的更改整合回中央儲存庫。第一步是再次進行拉取操作，因為在我工作的期間，同事們很可能已經將變更推送到了主分支。事實上，確實有幾個變更，所以我將它們拉取到我的工作副本中，並將我的更改疊加在這些變更之上，再次執行構建。通常這樣做感覺有些多餘，但這次卻出現了測試失敗。測試給了一些關於問題的線索，但我發現查看我拉取的提交變更更為有用，以了解發生了哪些更改。看來有人對一個函數進行了調整，將其中一些邏輯移到了呼叫端。他們已經修正了主分支代碼中的所有呼叫端，但我在我的變更中添加了一個新的呼叫端，他們當然無法預見這一點。我進行相同的調整後再次運行構建，這次通過了。

Since I was a few minutes sorting that out, I pull again, and again there's a new commit. However the build works fine with this one, so I'm able to `git push` my change up to the central repository.

由於剛才花了幾分鐘處理問題，我再次執行拉取操作，這次又出現了一個新提交。不過這次構建一切正常，所以我可以順利地將我的更改使用 `git push` 推送到中央儲存庫。

However my push doesn't mean I'm done. Once I've pushed to the mainline a Continuous Integration Service notices my commit, checks out the changed code onto a CI agent, and builds it there. Since the build was fine in my environment I don't expect it to fail on the CI Service, but there is a reason that “works on my machine” is a well-known phrase in programmer circles. It's rare that something gets missed that causes the CI Services build to fail, but rare is not the same as never.

然而，我的推送並不意味著我已經完成。一旦我將更改推送到主線，持續集成服務會偵測到我的提交，將更改過的代碼檢出至 CI 執行代理並在那裡進行構建。由於在我的環境中構建成功，因此我不預期在 CI 服務上會失敗，但「在我機器上可以運行」這句話在程序員圈子中廣為流傳是有原因的。雖然很少出現某些細節被忽略而導致 CI 服務構建失敗的情況，但少發生並不代表從未發生。

The integration machine's build doesn't take long, but it's long enough that an eager developer would be starting to think about the next step in calculating flight time. But I'm an old guy, so enjoy a few minutes to stretch my legs and read an email. I soon get a notification from the CI service that all is well, so I start the process again for the next part of the change.

整合機器的構建時間不長，但足夠讓一個急於求成的開發者開始思考下一步如何計算飛行時間。不過我是一個老手，因此會利用這幾分鐘活動一下，或者讀讀郵件。很快，我就收到了 CI 服務的通知，一切運行良好，因此我可以開始進行下一部分的變更了。

## Practices of Continuous Integration

The story above is an illustration of Continuous Integration that hopefully gives you a feel of what it's like for an ordinary programmer to work with. But, as with anything, there's quite a few things to sort out when doing this in daily work. So now we'll go through the key practices that we need to do.

上面的故事展示了持續整合的一個例子，希望能讓你感受到普通程序員的工作情況。不過，和任何事情一樣，在日常工作中實行這個流程時有很多細節需要處理。現在，我們來探討一下需要採取的關鍵實踐。

### Put everything in a version controlled mainline

These days almost every software team keeps their source code in a version control system, so that every developer can easily find not just the current state of the product, but all the changes that have been made to the product. Version control tools allow a system to be rolled back to any point in its development, which can be very helpful to understand the history of the system, using Diff Debugging to find bugs. As I write this, the dominant version control system is git.

如今，幾乎每個軟體團隊都會使用版本控制系統來管理源代碼，這樣每位開發人員都可以輕鬆查看產品的當前狀態以及所有更改記錄。版本控制工具允許系統回溯到開發過程中的任何時間點，這對於了解系統歷史很有幫助，並且可以利用「差異調試」來查找錯誤。目前，主流的版本控制系統是 Git。

But while version control is commonplace, some teams fail to take full advantage of version control. My test for full version control is that I should be able to walk up with a very minimally configured environment - say a laptop with no more than the vanilla operating system installed - and be able to easily build, and run the product after cloning the repository. This means the repository should reliably return product source code, tests, database schema, test data, configuration files, IDE configurations, install scripts, third-party libraries, and any tools required to build the software.

然而，儘管版本控制系統普遍存在，一些團隊卻未能充分利用它。對我來說，衡量版本控制是否完善的標準是：**我應該能在一個幾乎沒有配置的環境下操作——比如一台只有基本操作系統的筆電——然後在克隆代碼庫後，能夠輕鬆地構建和運行產品**。這意味著，代碼庫應該可靠地包含產品的源代碼、測試、資料庫架構、測試資料、配置文件、IDE 配置、安裝腳本、第三方庫，以及構建軟體所需的任何工具。

You might notice I said that the repository should _return_ all of these elements, which isn't the same as storing them. We don't have to store the compiler in the repository, but we need to be able to get at the right compiler. If I check out last year's product sources, I may need to be able to build them with the compiler I was using last year, not the version I'm using now. The repository can do this by storing a link to immutable asset storage - immutable in the sense that once an asset is stored with an id, I'll always get exactly that asset back again. I can also do this with library code, providing I both trust the asset storage and always reference a particular version, never “the latest version”.

你可能注意到我說代碼庫應該「返回」所有這些元素，這與「存儲」它們並不完全相同。我們不必在代碼庫中存儲編譯器，但我們需要能夠獲取正確的編譯器。如果我檢出去年的產品源代碼，可能需要使用當時的編譯器版本來構建它，而不是現在使用的版本。代碼庫可以通過存儲指向不可變資產存儲的鏈接來實現這一點——不可變的意思是，一旦一個資產被存儲並賦予 ID，我將始終獲得相同的資產。對於庫代碼也是如此，前提是我信任這個資產存儲，並且總是引用特定版本，而不是「最新版本」。

Similar asset storage schemes can be used for anything too large, such as videos. Cloning a repository often means grabbing everything, even if it's not needed. By using references to an asset store, the build scripts can choose to download only what's needed for a particular build.

類似的資產存儲方案也可以用於過大的內容，比如視頻。克隆代碼庫通常意味著獲取所有東西，即使它並非都需要。通過使用對資產存儲的引用，構建腳本可以選擇僅下載特定構建所需的內容。

In general we should store in source control everything we need to build anything, but nothing that we actually build. Some people do keep the build products in source control, but I consider that to be a smell - an indication of a deeper problem, usually an inability to reliably recreate builds. It can be useful to cache build products, but they should always be treated as disposable, and it's usually good to then ensure they are removed promptly so that people don't rely on them when they shouldn't.

一般來說，我們應該在版本控制系統中保存構建任何東西所需的所有內容，但不應該保存任何實際構建出來的東西。有些人確實會把構建產物保存在版本控制中，但我認為這是一種壞味道 - 這通常表明存在更深層次的問題，最常見的是無法可靠地重現構建。緩存構建產物可能會有用，但應該始終將它們視為可丟棄的，而且通常最好確保及時刪除它們，這樣人們就不會在不應該依賴它們的時候依賴它們。

A second element of this principle is that it should be easy to find the code for a given piece of work. Part of this is clear names and URL schemes, both within the repository and within the broader enterprise. It also means not having to spend time figuring out which branch within the version control system to use. Continuous Integration relies on having a clear mainline - a single, shared, branch that acts as the current state of the product. This is the next version that will be deployed to production.

這個原則的第二個要素是：應該能夠輕鬆找到特定工作項目的代碼。這部分包括清晰的命名和 URL 方案，不論是在倉庫內部還是在更廣泛的企業環境中都是如此。這也意味著不必浪費時間來確定該使用版本控制系統中的哪個分支。持續整合依賴於有一個清晰的主線 - 一個單一的、共享的分支，它代表著產品的當前狀態。這就是下一個將要部署到生產環境的版本。

Teams that use git mostly use the name “main” for the mainline branch, but we also sometimes see “trunk” or the old default of “master”. The mainline is that branch on the central repository, so to add a commit to a mainline called `main` I need to first commit to my local copy of `main` and then push that commit to the central server. The tracking branch (called something like `origin/main`) is a copy of the mainline on my local machine. However it may be out of date, since in a Continuous Integration environment there are many commits pushed into mainline every day.

使用 git 的團隊通常使用 "main" 作為主線分支的名稱，但我們有時也會看到 "trunk" 或舊的默認名稱 "master"。主線是指中央倉庫上的那個分支，所以要向名為 `main` 的主線添加提交，我需要先提交到我本地的 `main` 分支，然後將該提交推送到中央伺服器。跟蹤分支（通常叫做類似 `origin/main` 的名稱）是主線在我本地機器上的一個副本。但是它可能已經過時了，因為在持續整合環境中，每天都會有許多提交被推送到主線中。

As much as possible, we should use text files to define the product and its environment. I say this because, although version-control systems can store and track non-text files, they don't usually provide any facility to easily see the difference between versions. This makes it much harder to understand what change was made. It's possible that in the future we'll see more storage formats having the facility to create meaningful diffs, but at the moment clear diffs are almost entirely reserved for text formats. Even there we need to use text formats that will produce comprehensible diffs.

我們應該盡可能使用文本文件來定義產品及其環境。我這麼說是因為，儘管版本控制系統可以存儲和跟蹤非文本文件，但它們通常無法提供輕鬆查看版本之間差異的功能。這使得理解做出了什麼變更變得更加困難。雖然將來我們可能會看到更多存儲格式具備創建有意義的差異對比（diff）的功能，但目前來說，清晰的差異對比幾乎完全僅限於文本格式。即便是文本格式，我們也需要使用能夠產生易於理解的差異對比的文本格式。

### Automate the Build

Turning the source code into a running system can often be a complicated process involving compilation, moving files around, loading schemas into databases, and so on. However like most tasks in this part of software development it can be automated - and as a result should be automated. Asking people to type in strange commands or clicking through dialog boxes is a waste of time and a breeding ground for mistakes.

將源代碼轉換成運行中的系統通常可能是一個複雜的過程，包括編譯、移動文件、將架構（schema）加載到數據庫等等。然而，像軟件開發這部分的大多數任務一樣，這個過程是可以自動化的 - 因此也應該被自動化。要求人們輸入奇怪的命令或點擊對話框是在浪費時間，而且容易滋生錯誤。

Most modern programming environments include tooling for automating builds, and such tools have been around for a long time. I first encountered them with [make](https://en.wikipedia.org/wiki/Make_(software)), one of the earliest Unix tools.

大多數現代編程環境都包含了用於自動化構建的工具，而且這類工具已經存在很長時間了。我最早接觸到的是 make，它是最早的 Unix 工具之一。

Any instructions for the build need to be stored in the repository, in practice this means that we must use text representations. That way we can easily inspect them to see how they work, and crucially, see diffs when they change. Thus teams using Continuous Integration avoid tools that require clicking around in UIs to perform a build or to configure an environment.

任何構建的指令都需要存儲在代碼倉庫中，這實際上意味著我們必須使用文本形式來表示這些指令。這樣我們就可以輕鬆檢查它們是如何工作的，更重要的是，當它們發生變更時可以看到差異對比（diff）。因此，使用持續整合的團隊會避免使用那些需要在用戶界面中點來點去來執行構建或配置環境的工具。

It's possible to use a regular programming language to automate builds, indeed simple builds are often captured as shell scripts. But as builds get more complicated it's better to use a tool that's designed with build automation in mind. Partly this is because such tools will have built-in functions for common build tasks. But the main reason is that build tools work best with a particular way to organize their logic - an alternative computational model that I refer to as a [Dependency Network](https://www.martinfowler.com/dslCatalog/dependencyNetwork.html). A dependency network organizes its logic into tasks which are structured as a graph of dependencies.

使用常規編程語言來自動化構建是可行的，實際上簡單的構建通常會被寫成 shell 腳本。但是當構建變得更複雜時，最好使用專門為構建自動化設計的工具。部分原因是這類工具會為常見的構建任務提供內置函數。但主要原因是構建工具在特定的邏輯組織方式下工作得最好 - 這是一種我稱之為「依賴網絡」的另類計算模型。依賴網絡將其邏輯組織成任務，這些任務被組織成一個依賴關係圖。

A trivially simple dependency network might say that the “test” task is dependent upon the “compile” task. If I invoke the test task, it will look to see if the compile task needs to be run and if so invoke it first. Should the compile task itself have dependencies, the network will look to see if it needs to invoke them first, and so on backwards along the dependency chain. A dependency network like this is useful for build scripts because often tasks take a long time, which is wasted if they aren't needed. If nobody has changed any source files since I last ran the tests, then I can save doing a potentially long compilation.

一個極其簡單的依賴網絡可能會表明「測試」任務依賴於「編譯」任務。如果我執行測試任務，它會先檢查是否需要運行編譯任務，如果需要的話就會先執行編譯任務。如果編譯任務本身也有依賴項，那麼網絡會檢查是否需要先執行這些依賴項，以此類推，沿著依賴鏈往回檢查。這樣的依賴網絡對於構建腳本來說很有用，因為任務通常需要很長時間，如果不需要執行就會造成時間浪費。如果自從我上次運行測試以來沒有人修改過任何源文件，那麼我就可以省去可能需要很長時間的編譯過程。

To tell if a task needs to be run, the most common and straightforward way is to look at the modification times of files. If any of the input files to the compilation have been modified later than the output, then we know the compilation needs to be executed if that task is invoked.

要判斷是否需要執行某個任務，最常見和最直接的方法是查看文件的修改時間。如果任何輸入文件的修改時間晚於輸出文件，那麼我們就知道如果調用該任務，就需要執行編譯。

A common mistake is not to include everything in the automated build. The build should include getting the database schema out of the repository and firing it up in the execution environment. I'll elaborate my earlier rule of thumb: anyone should be able to bring in a clean machine, check the sources out of the repository, issue a single command, and have a running system on their own environment.

一個常見的錯誤是沒有把所有東西都包含在自動化構建中。構建過程應該包括從代碼倉庫中獲取數據庫架構（schema），並在執行環境中啟動它。我要詳細說明一下之前提到的經驗法則：任何人都應該能夠拿一台全新的機器，從代碼倉庫中檢出源代碼，執行一個單一的命令，就能在他們自己的環境中得到一個運行中的系統。

While a simple program may only need a line or two of script file to build, complex systems often have a large graph of dependencies, finely tuned to minimize the amount of time required to build things. This website, for example, has over a thousand web pages. My build system knows that should I alter the source for this page, I only have to build this one page. But should I alter a core file in the publication tool chain, then it needs to rebuild them all. Either way, I invoke the same command in my editor, and the build system figures out how much to do.

雖然一個簡單的程序可能只需要一兩行腳本就能構建，但複雜的系統通常會有一個龐大的依賴關係圖，經過精心調整以最小化構建所需的時間。舉例來說，這個網站有超過一千個網頁。我的構建系統知道，如果我修改了這個頁面的源文件，我只需要構建這一個頁面。但如果我修改了發布工具鏈中的一個核心文件，那麼系統就需要重新構建所有頁面。無論是哪種情況，我在編輯器中執行的都是相同的命令，而構建系統會自行計算需要完成多少工作。

Depending on what we need, we may need different kinds of things to be built. We can build a system with or without test code, or with different sets of tests. Some components can be built stand-alone. A build script should allow us to build alternative targets for different cases.

根據我們的需求，我們可能需要構建不同類型的項目。我們可以構建一個包含或不包含測試代碼的系統，或者使用不同組合的測試。某些組件可以單獨構建。構建腳本應允許我們為不同情況構建備選目標。

### Make the Build Self-Testing

Traditionally a build meant compiling, linking, and all the additional stuff required to get a program to execute. A program may run, but that doesn't mean it does the right thing. Modern statically typed languages can catch many bugs, but far more slip through that net. This is a critical issue if we want to integrate as frequently as Continuous Integration demands. If bugs make their way into the product, then we are faced with the daunting task of performing bug fixes on a rapidly-changing code base. Manual testing is too slow to cope with the frequency of change.

傳統上，構建意味著編譯、鏈接以及所有其他讓程序能夠執行的必要操作。一個程序可能可以運行，但這並不意味著它的行為是正確的。現代靜態類型語言可以捕捉到許多錯誤，但仍有更多的錯誤會漏網而出。這在我們希望像持續集成那樣頻繁集成時是一個關鍵問題。如果錯誤進入了產品中，我們就需要面對在快速變動的代碼庫中修復錯誤的艱鉅任務。手動測試的速度過於緩慢，無法應對如此頻繁的變更。

Faced with this, we need to ensure that bugs don't get into the product in the first place. The main technique to do this is a comprehensive test suite, one that is run before each integration to flush out as many bugs as possible. Testing isn't perfect, of course, but it can catch a lot of bugs - enough to be useful. Early computers I used did a visible memory self-test when they were booting up, which led me referring to this as [Self Testing Code](https://www.martinfowler.com/bliki/SelfTestingCode.html).

為了應對這種情況，我們需要確保錯誤不會一開始就進入產品中。實現這一點的主要方法是使用全面的測試套件，在每次集成前執行測試，盡可能排除錯誤。測試並非完美，當然，但它可以捕捉到大量錯誤，足以發揮其作用。我用過的早期計算機在啟動時會執行顯示的內存自測，因此我稱這種方法為「自測代碼」。

Writing self-testing code affects a programmer's workflow. Any programming task combines both modifying the functionality of the program, and also augmenting the test suite to verify this changed behavior. A programmer's job isn't done merely when the new feature is working, but also when they have automated tests to prove it.

撰寫自測代碼會影響程序員的工作流程。任何編程任務都需要同時修改程序的功能，並增強測試套件以驗證此更改行為。程序員的工作並不僅僅是在新功能正常運行時完成，還需要有自動化測試來證明功能的正確性。

Over the two decades since the first version of this article, I've seen programming environments increasingly embrace the need to provide the tools for programmers to build such test suites. The biggest push for this was JUnit, originally written by Kent Beck and Erich Gamma, which had a marked impact on the Java community in the late 1990s. This inspired similar testing frameworks for other languages, often referred to as [Xunit](https://www.martinfowler.com/bliki/Xunit.html) frameworks. These stressed a light-weight, programmer-friendly mechanics that allowed a programmer to easily build tests in concert with the product code. Often these tools have some kind of graphical progress bar that is green if the tests pass, but turns red should any fail - leading to phrases like “green build”, or “red-bar”.

在本文的最初版本發表後的二十多年裡，我看到編程環境越來越重視為程序員提供構建測試套件的工具。最大的推動力是由 Kent Beck 和 Erich Gamma 創建的 JUnit，這對 1990 年代後期的 Java 社區產生了顯著影響。這啟發了其他語言的類似測試框架，通常被稱為 Xunit 框架。這些框架強調輕量、程序員友好的機制，使程序員能夠輕鬆地與產品代碼一同構建測試。這些工具通常具有某種圖形進度條，測試通過時顯示為綠色，若測試失敗則顯示為紅色，因此出現了“綠色構建”或“紅色條”的說法。

The test of such a test suite is that we should be confident that if the tests are green, then no significant bugs are in the product. I like to imagine a mischievous imp that is able to make simple modifications to the product code, such as commenting out lines, or reversing conditionals, but is not able to change the tests. A sound test suite would never allow the imp to do any damage without a test turning red. And any test failing is enough to fail the build, 99.9% green is still red.

這樣的測試套件的衡量標準是：當測試顯示綠色時，我們應該有信心產品中沒有重大錯誤。我喜歡想像一個淘氣的小鬼可以對產品代碼進行簡單修改，例如注釋掉某些行或反轉條件判斷，但無法更改測試。健全的測試套件不會允許小鬼做任何破壞而不導致測試變紅。只要有一個測試失敗，整個構建就算失敗，99.9% 綠色依然視為紅色。

Self-testing code is so important to Continuous Integration that it is a necessary prerequisite. Often the biggest barrier to implementing Continuous Integration is insufficient skill at testing.

自測代碼對於持續整合至關重要，以至於它成為必須的先決條件。實施持續整合的最大障礙通常是缺乏足夠的測試技能。

That self-testing code and Continuous Integration are so tied together is no surprise. Continuous Integration was originally developed as part of [Extreme Programming](https://www.martinfowler.com/bliki/ExtremeProgramming.html) and testing has always been a core practice of Extreme Programming. This testing is often done in the form of [Test Driven Development](https://www.martinfowler.com/bliki/TestDrivenDevelopment.html) (TDD), a practice that instructs us to never write new code unless it fixes a test that we've written just before. TDD isn't essential for Continuous Integration, as tests can be written after production code as long as they are done before integration. But I do find that, most of the time, TDD is the best way to write self-testing code.

自測代碼與持續整合的緊密聯繫並不令人意外。持續整合最初是在極限編程（Extreme Programming）中發展出來的，而測試一直是極限編程的核心實踐。這種測試通常以測試驅動開發（TDD）的形式進行，這種方法要求我們不編寫任何新代碼，除非它解決了我們剛剛編寫的測試。儘管持續整合不必依賴於 TDD，只要測試在集成之前完成，就可以在生產代碼之後撰寫。但我發現，在大多數情況下，TDD 是撰寫自測代碼的最佳方法。

The tests act as an automated check of the health of the code base, and while tests are the key element of such an automated verification of the code, many programming environments provide additional verification tools. Linters can detect poor programming practices, and ensure code follows a team's preferred formatting style, vulnerability scanners can find security weaknesses. Teams should evaluate these tools to include them in the verification process.

測試充當了代碼庫健康狀況的自動檢查工具，儘管測試是代碼自動驗證的關鍵元素，許多開發環境還提供其他驗證工具。靜態分析工具（Linters）可以檢測出不良的編程習慣，並確保代碼遵循團隊偏好的格式風格，而漏洞掃描工具則可以發現安全弱點。團隊應當評估這些工具並將它們納入驗證過程中。

Of course we can't count on tests to find everything. As it's often been said: tests don't prove the absence of bugs. However perfection isn't the only point at which we get payback for a self-testing build. Imperfect tests, run frequently, are much better than perfect tests that are never written at all.

當然，我們無法指望測試發現所有問題。正如人們常說的：測試無法證明不存在缺陷。然而，達到完美並不是我們從自測構建中獲得回報的唯一標準。不完美的測試，但經常執行，比從未寫出的完美測試要好得多。

### Everyone Pushes Commits To the Mainline Every Day

Integration is primarily about communication. Integration allows developers to tell other developers about the changes they have made. Frequent communication allows people to know quickly as changes develop.

集成主要是關於溝通。集成允許開發人員告知其他開發人員他們所做的更改。頻繁的溝通可以使人們能夠快速了解正在發生的變化。

The one prerequisite for a developer committing to the mainline is that they can correctly build their code. This, of course, includes passing the build tests. As with any commit cycle the developer first updates their working copy to match the mainline, resolves any conflicts with the mainline, then builds on their local machine. If the build passes, then they are free to push to the mainline.

開發人員提交到主線的唯一先決條件是他們能夠正確地構建他們的代碼。這當然包括通過構建測試。與任何提交週期一樣,開發人員首先會更新他們的工作副本以匹配主線,解決與主線的任何衝突,然後在本地機器上構建。如果構建通過,那麼他們就可以自由地推送到主線。

If everyone pushes to the mainline frequently, developers quickly find out if there's a conflict between two developers. The key to fixing problems quickly is finding them quickly. With developers committing every few hours a conflict can be detected within a few hours of it occurring, at that point not much has happened and it's easy to resolve. Conflicts that stay undetected for weeks can be very hard to resolve.

如果每個人都頻繁地向主線推送,開發人員很快就會發現兩個開發人員之間是否存在衝突。快速解決問題的關鍵是能夠及時發現問題。當開發人員每隔幾個小時就提交一次,衝突就會在發生後的幾個小時內被發現。在這個時候,還沒有發生太多變化,很容易解決。但如果衝突被忽略數週,就會變得非常難以解決。

Conflicts in the codebase come in different forms. The easiest to find and resolve are textual conflicts, often called “merge conflicts”, when two developers edit the same fragment of code in different ways. Version-control tools detect these easily once the second developer pulls the updated mainline into their working copy. The harder problem are [Semantic Conflicts](https://www.martinfowler.com/bliki/SemanticConflict.html). If my colleague changes the name of a function and I call that function in my newly added code, the version-control system can't help us. In a statically typed language we get a compilation failure, which is pretty easy to detect, but in a dynamic language we get no such help. And even statically-typed compilation doesn't help us when a colleague makes a change to the body of a function that I call, making a subtle change to what it does. This is why it's so important to have self-testing code.

代碼庫中的衝突有不同的形式。最容易發現和解決的是文本衝突,通常被稱為"合併衝突",這是當兩個開發人員以不同的方式編輯相同的代碼片段時發生的。一旦第二個開發人員拉取了更新的主線到他們的工作副本,版本控制工具就很容易檢測到這些衝突。更難解決的是語義衝突。如果我的同事更改了一個函數的名稱,而我在新添加的代碼中調用了該函數,版本控制系統無法幫助我們。在靜態類型語言中,我們會遇到編譯失敗,這很容易發現,但在動態語言中我們卻得不到這樣的幫助。即使是靜態類型的編譯也無法解決當同事更改了我調用的函數的內部實現,從而微妙地改變了它的行為這種情況。這就是為什麼擁有自我測試的代碼如此重要。

A test failure alerts that there's a conflict between changes, but we still have to figure out what the conflict is and how to resolve it. Since there's only a few hours of changes between commits, there's only so many places where the problem could be hiding. Furthermore since not much has changed we can use [Diff Debugging](https://www.martinfowler.com/bliki/DiffDebugging.html) to help us find the bug.

測試失敗意味著存在變更之間的衝突,但我們仍然需要弄清楚具體是什麼衝突,以及如何解決它。由於提交之間只有幾個小時的變更,問題可能隱藏的地方也不多。此外,由於變更不多,我們可以使用差異調試來幫助我們找到bug。

My general rule of thumb is that every developer should commit to the mainline every day. In practice, those experienced with Continuous Integration integrate more frequently than that. The more frequently we integrate, the less places we have to look for conflict errors, and the more rapidly we fix conflicts.

我的一般經驗法則是,每個開發人員應該每天都提交到主線。實際上,那些有持續集成經驗的開發人員集成的頻率更高。我們集成的頻率越高,需要查找衝突錯誤的地方就越少,解決衝突的速度也越快。

Frequent commits encourage developers to break down their work into small chunks of a few hours each. This helps track progress and provides a sense of progress. Often people initially feel they can't do something meaningful in just a few hours, but we've found that mentoring and practice helps us learn.

頻繁的提交鼓勵開發人員將工作分解為每幾小時一個的小塊。這有助於追蹤進度，並提供成就感。人們常常最初會覺得在短短幾小時內難以完成有意義的工作，但我們發現通過指導和練習可以幫助我們學習如何達到這一點。

### Every Push to Mainline Should Trigger a Build

If everyone on the team integrates at least daily, this ought to mean that the mainline stays in a healthy state. In practice, however, things still do go wrong. This may be due to lapses in discipline, neglecting to update and build before a push, there may also be environmental differences between developer workspaces.

如果團隊中的每個人至少每天進行一次集成，理論上主線應該會保持在健康狀態。然而，實際上，問題還是會發生。這可能是因為紀律鬆懈，例如在推送之前忘記更新和構建，或是開發者工作環境之間存在差異。

We thus need to ensure that every commit is verified in a reference environment. The usual way to do this is with a **Continuous Integration Service (CI Service)** that monitors the mainline. (Examples of CI Services are tools like Jenkins, GitHub Actions, Circle CI etc.) Every time the mainline receives a commit, the CI service checks out the head of the mainline into an integration environment and performs a full build. Only once this integration build is green can the developer consider the integration to be complete. By ensuring we have a build with every push, should we get a failure, we know that the fault lies in that latest push, narrowing down where have to look to fix it.

因此，我們需要確保每次提交都在參考環境中進行驗證。通常的做法是使用持續集成服務（CI 服務）來監控主線（例如 Jenkins、GitHub Actions、Circle CI 等工具）。每當主線收到提交時，CI 服務就會將主線的最新版本檢出到集成環境中並執行完整構建。只有當這個集成構建通過後，開發者才能認為集成完成。透過在每次推送時進行構建，一旦出現失敗，我們就知道問題出在最新推送，縮小了需要排查的範圍。

I want to stress here that when we use a CI Service, we only use it on the mainline, which is the main branch on the reference instance of the version control system. It's common to use a CI service to monitor and build from multiple branches, but the whole point of integration is to have all commits coexisting on a _single_ branch. While it may be useful to use CI service to do an automated build for different branches, that's not the same as Continuous Integration, and teams using Continuous Integration will only need the CI service to monitor a single branch of the product.

在此我想強調，當我們使用 CI 服務時，僅使用於主線，也就是版本控制系統中參考實例的主分支。雖然 CI 服務常用於監控和構建多個分支，但整合的核心意圖是讓所有提交共存於單一分支。儘管在不同分支上執行自動化構建可能有所助益，但這與持續整合不同。真正實行持續整合的團隊僅需讓 CI 服務監控產品的單一主分支。

While almost all teams use CI Services these days, it is [perfectly possible](https://www.jamesshore.com/v2/blog/2006/continuous-integration-on-a-dollar-a-day) to do Continuous Integration without one. Team members can manually check out the head on the mainline onto an integration machine and perform a build to verify the integration. But there's little point in a manual process when automation is so freely available.

儘管如今幾乎所有團隊都使用 CI 服務來執行持續整合，但其實並不一定非得使用 CI 服務。團隊成員可以手動檢出主線的最新提交，然後在整合機器上進行構建以驗證整合的成果。然而，既然自動化工具如此容易取得，依賴手動流程的意義就不大了。

(This is an appropriate point to mention that my colleagues at Thoughtworks, have contributed a lot of open-source tooling for Continuous Integration, in particular Cruise Control - the first CI Service.)

(這裡也適合提到，我在 Thoughtworks 的同事們對持續整合的開源工具做出了許多貢獻，尤其是 Cruise Control——首個 CI 服務。)

### Fix Broken Builds Immediately

Continuous Integration can only work if the mainline is kept in a healthy state. Should the integration build fail, then it needs to be fixed right away. As Kent Beck puts it: “nobody has a higher priority task than fixing the build”. This doesn't mean that everyone on the team has to stop what they are doing in order to fix the build, usually it only needs a couple of people to get things working again. It does mean a conscious prioritization of a build fix as an urgent, high priority task.

持續整合的成功取決於主線保持在穩定健康的狀態。一旦集成構建失敗，必須立刻修復它。正如 Kent Beck 所說：「沒有人有比修復構建更高優先級的任務。」這並不意味著團隊中的每個人都必須暫停手頭的工作來修復構建，通常只需幾個人來解決問題即可。然而，這確實需要有意識地將修復構建視為一項緊急的高優先級任務。

Usually the best way to fix the build is to revert the latest commit from the mainline, taking the system back to the last-known good build. If the cause of the problem is immediately obvious then it can be fixed directly with a new commit, but otherwise reverting the mainline allows some folks to figure out the problem in a separate development environment, allowing the rest of the team to continue to work with the mainline.

修復構建的最佳方法通常是回退主線中的最新提交，將系統恢復到上一次已知的良好構建狀態。如果問題的原因很明顯，可以直接使用新提交來修復。但若原因不明，回退主線可以讓部分人員在單獨的開發環境中調查問題，同時讓其他團隊成員繼續在主線上進行工作。

Some teams prefer to remove all risk of breaking the mainline by using a [Pending Head](https://www.martinfowler.com/bliki/PendingHead.html) (also called Pre-tested, Delayed, or Gated Commit.) To do this the CI service needs to set things up so that commits pushed to the mainline for integration do not immediately go onto the mainline. Instead they are placed on another branch until the build completes and only migrated to the mainline after a green build. While this technique avoids any danger to mainline breaking, an effective team should rarely see a red mainline, and on the few times it happens its very visibility encourages folks to learn how to avoid it.

有些團隊傾向於使用「待處理頭」（Pending Head，又稱為預測試、延遲或門控提交）來完全避免主線出現中斷的風險。這樣做的話，CI 服務需要將提交推送到一個獨立分支，直到構建成功後才將更改遷移到主線。這種方法確實避免了主線中斷的風險，但對於一個高效的團隊而言，主線很少會出現紅色警告，偶爾出現的情況反而讓團隊成員從中學習如何避免類似情況，讓這種做法具有其教育意義。

### Keep the Build Fast

The whole point of Continuous Integration is to provide rapid feedback. Nothing sucks the blood of Continuous Integration more than a build that takes a long time. Here I must admit a certain crotchety old guy amusement at what's considered to be a long build. Most of my colleagues consider a build that takes an hour to be totally unreasonable. I remember teams dreaming that they could get it so fast - and occasionally we still run into cases where it's very hard to get builds to that speed.

Continuous Integration 的核心目的是提供快速反饋，而漫長的構建過程是其最大的障礙。我有些頑固的老派幽默，對於「漫長構建」的定義頗有些感慨。如今，大多數同事認為一小時的構建過程已經完全無法接受，回想過去，有的團隊夢想構建能達到這種速度；偶爾，我們仍然會遇到難以將構建壓縮到合理時間的情況。

For most projects, however, the XP guideline of a ten minute build is perfectly within reason. Most of our modern projects achieve this. It's worth putting in concentrated effort to make it happen, because every minute chiseled off the build time is a minute saved for each developer every time they commit. Since Continuous Integration demands frequent commits, this adds up to a lot of the time.

然而，對於大多數項目來說，極限編程（XP）建議的十分鐘構建時間是完全合理的。我們現在的大多數現代項目都能達到這個標準。為實現這個目標而付出集中努力是值得的，因為每減少一分鐘的構建時間，就意味著每個開發人員在每次提交代碼時都能節省一分鐘。由於持續整合要求頻繁提交，這累積起來就能節省大量時間。

If we're staring at a one hour build time, then getting to a faster build may seem like a daunting prospect. It can even be daunting to work on a new project and think about how to keep things fast. For enterprise applications, at least, we've found the usual bottleneck is testing - particularly tests that involve external services such as a database.

如果我們面對著一小時的構建時間，那麼要實現更快的構建可能看起來是一項艱鉅的任務。即使是在新項目中思考如何保持構建速度也可能令人望而生畏。至少對於企業應用程序來說，我們發現通常的瓶頸在於測試——特別是那些涉及數據庫等外部服務的測試。

Probably the most crucial step is to start working on setting up a Deployment Pipeline. The idea behind a **deployment pipeline** (also known as **build pipeline** or **staged build**) is that there are in fact multiple builds done in sequence. The commit to the mainline triggers the first build - what I call the commit build. The **commit build** is the build that's needed when someone pushes commits to the mainline. The commit build is the one that has to be done quickly, as a result it will take a number of shortcuts that will reduce the ability to detect bugs. The trick is to balance the needs of bug finding and speed so that a good commit build is stable enough for other people to work on.

可能最關鍵的步驟是著手建立部署流水線。**部署流水線**（也被稱為**構建流水線**或**分階段構建**）背後的理念是實際上有多個構建按順序執行。提交到主線的代碼會觸發第一個構建——我稱之為提交構建。**提交構建**是有人向主線推送提交時需要進行的構建。提交構建必須快速完成，因此它會採取一些捷徑，這會降低檢測錯誤的能力。訣竅在於平衡發現錯誤和速度的需求，使得一個好的提交構建能夠穩定到足以讓其他人在其基礎上工作。

Once the commit build is good then other people can work on the code with confidence. However there are further, slower, tests that we can start to do. Additional machines can run further testing routines on the build that take longer to do.

一旦提交構建通過，其他人就可以放心地基於這些代碼工作。然而，我們還可以開始執行一些更耗時的測試。額外的機器可以對這個構建運行更多需要較長時間的測試程序。

A simple example of this is a two stage deployment pipeline. The first stage would do the compilation and run tests that are more localized unit tests with slow services replaced by [Test Doubles](https://www.martinfowler.com/bliki/TestDouble.html), such as a fake in-memory database or a stub for an external service. Such tests can run very fast, keeping within the ten minute guideline. However any bugs that involve larger scale interactions, particularly those involving the real database, won't be found. The second stage build runs a different suite of tests that do hit a real database and involve more end-to-end behavior. This suite might take a couple of hours to run.

這裡是一個簡單的兩階段部署流水線的例子。第一階段會進行編譯，並運行那些較為局部化的單元測試，其中緩慢的服務會被測試替身所替代，比如內存中的假數據庫或外部服務的存根。這樣的測試可以非常快速地運行，能夠控制在十分鐘的指導原則之內。然而，任何涉及更大規模交互的錯誤，特別是那些涉及真實數據庫的錯誤，在這個階段是無法被發現的。第二階段的構建會運行一套不同的測試，這些測試會真正訪問實際的數據庫，並涉及更多的端到端行為測試。這套測試可能需要幾個小時才能運行完成。

In this scenario people use the first stage as the commit build and use this as their main CI cycle. If the secondary build fails, then this may not have the same 'stop everything' quality, but the team does aim to fix such bugs as rapidly as possible, while keeping the commit build running. Since the secondary build may be much slower, it may not run after every commit. In that case it runs as often as it can, picking the last good build from the commit stage.

在這種場景下，人們使用第一階段作為提交構建，並將其作為主要的持續整合循環。如果第二階段的構建失敗，可能不會產生"停止一切"的那種影響，但團隊仍會努力儘快修復這類錯誤，同時保持提交構建的運行。由於第二階段構建可能會慢得多，它可能不會在每次提交後都運行。在這種情況下，它會盡可能頻繁地運行，選擇提交階段中最後一個成功的構建來執行。

If the secondary build detects a bug, that's a sign that the commit build could do with another test. As much as possible we want to ensure that any later-stage failure leads to new tests in the commit build that would have caught the bug, so the bug stays fixed in the commit build. This way the commit tests are strengthened whenever something gets past them. There are cases where there's no way to build a fast-running test that exposes the bug, so we may decide to only test for that condition in the secondary build. Most of the time, fortunately, we can add suitable tests to the commit build.

如果第二階段構建檢測到一個錯誤，這表明提交構建需要增加另一個測試。我們要儘可能確保任何後期階段的失敗都能導致在提交構建中添加新的測試，這些測試本應該能夠捕獲到這個錯誤，這樣錯誤就能在提交構建階段就被修復。通過這種方式，每當有錯誤漏過提交測試時，提交測試就會得到加強。有些情況下，可能無法建立一個能夠暴露該錯誤的快速運行測試，這時我們可能會決定只在第二階段構建中測試這種情況。不過幸運的是，在大多數時候，我們都能在提交構建中添加合適的測試。

Another way to speed things up is to use parallelism and multiple machines. Cloud environments, in particular, allow teams to easily spin up a small fleet of servers for builds. Providing the tests can run reasonably independently, which well-written tests can, then using such a fleet can get very rapid build times. Such parallel cloud builds may also be worthwhile to a developer's pre-integration build too.

另一種加速的方法是使用並行處理和多台機器。特別是在雲環境中，團隊可以輕鬆地啟動一小組服務器用於構建。如果測試能夠合理地獨立運行（這是精心編寫的測試所具備的特質），那麼使用這樣的服務器集群可以獲得非常快的構建時間。這種並行的雲構建對於開發人員的預整合構建也可能是值得的。

While we're considering the broader build process, it's worth mentioning another category of automation, interaction with dependencies. Most software uses a large range of dependent software produced by different organizations. Changes in these dependencies can cause breakages in the product. A team should thus automatically check for new versions of dependencies and integrate them into the build, essentially as if they were another team member. This should be done frequently, usually at least daily, depending on the rate of change of the dependencies. A similar approach should be used with running Contract Tests. If these dependency interactions go red, they don't have the same “stop the line” effect as regular build failures, but do require prompt action by the team to investigate and fix.

在我們考慮更廣泛的構建過程時，值得提到另一類自動化——與依賴項的交互。大多數軟件都使用大量由不同組織開發的依賴軟件。這些依賴項的變化可能會導致產品出現故障。因此，團隊應該自動檢查依賴項的新版本並將其整合到構建中，基本上就像它們是另一個團隊成員一樣。這種檢查應該經常進行，通常至少每天一次，具體頻率取決於依賴項的變化率。同樣的方法也應該用於運行契約測試（Contract Tests）。如果這些依賴項交互出現紅色警告，它們不會像常規構建失敗那樣產生"停止生產線"的效果，但確實需要團隊及時採取行動進行調查和修復。

### Hide Work-in-Progress

Continuous Integration means integrating as soon as there is a little forward progress and the build is healthy. Frequently this suggests integrating before a user-visible feature is fully formed and ready for release. We thus need to consider how to deal with latent code: code that's part of an unfinished feature that's present in a live release.

Continuous Integration 意味著只要有些許進展且構建狀態正常，我們就應該進行集成。這通常意味著在一個用戶可見的功能尚未完全完成、還不能發布前就進行集成。因此，我們需要考慮如何處理潛伏代碼：這些代碼屬於尚未完成的功能，但已存在於實際發布的版本中。

Some people worry about latent code, because it's putting non-production quality code into the released executable. Teams doing Continuous Integration ensure that all code sent to the mainline is production quality, together with the tests that verify the code. Latent code may never be executed in production, but that doesn't stop it from being exercised in tests.

有些人擔心潛伏代碼，因為它將非生產質量的代碼放入了發布的可執行文件中。進行持續集成的團隊確保所有發送到主幹的代碼都是生產質量的，並附有驗證代碼的測試。潛伏代碼在生產環境中可能從未被執行，但這並不妨礙它在測試中得到驗證。

We can prevent the code being executed in production by using a [Keystone Interface](https://www.martinfowler.com/bliki/KeystoneInterface.html) - ensuring the interface that provides a path to the new feature is the last thing we add to the code base. Tests can still check the code at all levels other than that final interface. In a well-designed system, such interface elements should be minimal and thus simple to add with a short programming episode.

透過使用「Keystone Interface」來防止代碼在生產環境中被執行，這種做法能確保通往新功能的接口是我們在代碼庫中最後添加的部分。這樣，測試可以在所有層次上驗證代碼，而不依賴於那個最終接口。在一個設計良好的系統中，這些接口元素應該是最小化的，因此可以在短暫的編程過程中輕鬆地添加到系統中。

Using [Dark Launching](https://www.martinfowler.com/bliki/DarkLaunching.html) we can test some changes in production before we make them visible to the user. This technique is useful for assessing the impact on performance,

使用暗發布（Dark Launching），我们可以在用戶看不到的情況下，將一些更改部署到生产環境中進行測試。此技術非常適用於評估對性能的影響，

Keystones cover most cases of latent code, but for occasions where that's not possible we use Feature Flags. Feature flags are checked whenever we are about to execute latent code, they are set as part of the environment, perhaps in an environment-specific configuration file. That way the latent code can be active for testing, but disabled in production. As well as enabling Continuous Integration, feature flags also make it easier for runtime switching for A/B testing and Canary Releases. We then make sure we remove this logic promptly once a feature is fully released, so that the flags don't clutter the code base.

基石（Keystone）涵蓋了大多數潛伏代碼的情況，但在無法使用基石的情況下，我們會使用功能旗標（Feature Flags）。功能旗標在即將執行潛伏代碼時會進行檢查，並在環境中設置，例如在環境特定的配置文件中。這樣一來，潛伏代碼在測試時可以被啟用，而在生產環境中則禁用。除了實現持續整合外，功能旗標還能讓我們更輕鬆地進行 A/B 測試和金絲雀發布（Canary Releases）的運行時切換。我們確保在功能完全發布後及時移除這些邏輯，避免旗標混亂代碼基礎。

[Branch By Abstraction](https://www.martinfowler.com/bliki/BranchByAbstraction.html) is another technique for managing latent code, which is particularly useful for large infrastructural changes within a code base. Essentially this creates an internal interface to the modules that are being changed. The interface can then route between old and new logic, gradually replacing execution paths over time. We've seen this done to switch such pervasive elements as changing the persistence platform.

抽象分支（Branch By Abstraction）是管理潛伏代碼的另一種技術，特別適用於代碼基礎中大型的基礎結構變更。基本上，它為正在更改的模組創建了一個內部接口。該接口可以在舊邏輯和新邏輯之間進行路由，逐步替換執行路徑。我們曾見過使用此方法來切換諸如更改持久性平台等影響深遠的元素。

When introducing a new feature, we should always ensure that we can rollback in case of problems. [Parallel Change](https://www.martinfowler.com/bliki/ParallelChange.html) (aka expand-contract) breaks a change into reversible steps. For example, if we rename a database field, we first create a new field with the new name, then write to both old and new fields, then copy data from the exisitng old fields, then read from the new field, and only then remove the old field. We can reverse any of these steps, which would not be possible if we made such a change all at once. Teams using Continuous Integration often look to break up changes in this way, keeping changes small and easy to undo.

在引入新功能時，我們應該確保在遇到問題時可以進行回滾。並行變更（又稱擴展-收縮）將更改分成可逆的步驟。例如，如果我們要重新命名資料庫欄位，首先創建一個新名稱的欄位，然後同時寫入舊欄位和新欄位，接著將數據從現有的舊欄位複製到新欄位，然後從新欄位讀取數據，最後才刪除舊欄位。這樣我們可以回退任何一個步驟，這是一次性進行更改無法實現的。使用持續整合的團隊通常會尋求以這種方式分解更改，使更改保持小規模且容易撤銷。

### Test in a Clone of the Production Environment

The point of testing is to flush out, under controlled conditions, any problem that the system will have in production. A significant part of this is the environment within which the production system will run. If we test in a different environment, every difference results in a risk that what happens under test won't happen in production.

測試的目的是在可控的條件下排除系統在生產環境中可能遇到的任何問題。其中一個重要部分是生產系統將運行的環境。如果我們在不同的環境中進行測試，每個差異都會帶來一個風險，即測試中發生的情況可能不會在生產環境中發生。

Consequently, we want to set up our test environment to be as exact a mimic of our production environment as possible. Use the same database software, with the same versions, use the same version of the operating system. Put all the appropriate libraries that are in the production environment into the test environment, even if the system doesn't actually use them. Use the same IP addresses and ports, run it on the same hardware.

因此，我們希望將測試環境設置得與生產環境盡可能相似。使用相同版本的資料庫軟體、相同版本的作業系統。將生產環境中的所有適當程式庫放入測試環境，即使系統實際上並未使用它們。使用相同的 IP 地址和端口，並在相同的硬體上運行。

Virtual environments make it much easier than it was in the past to do this. We run production software in containers, and reliably build exactly the same containers for testing, even in a developer's workspace. It's worth the effort and cost to do this, the price is usually small compared to hunting down a single bug that crawled out of the hole created by environment mismatches.

虛擬環境使這項工作比過去更容易。我們在容器中運行生產軟體，並且可以可靠地為測試構建完全相同的容器，即使是在開發人員的工作區中。這項投入和成本是值得的，通常相較於追查因環境不匹配所引發的單一錯誤來說，代價小得多。

Some software is designed to run in multiple environments, such as different operating systems and platform versions. The deployment pipeline should arrange for testing in all of these environments in parallel.

有些軟體被設計成可以在多種環境中運行，例如不同的操作系統和平台版本。部署管道應安排在所有這些環境中並行進行測試。

A point to take care of is when the production environment isn't as good as the development environment. Will the production software be running on machines connected with dodgy wifi, like smartphones? Then ensure a test environment mimics poor network connections.

需要注意的一點是，當生產環境的質量不如開發環境時。我們是否會在連接不穩的 Wi-Fi 上運行生產軟體，比如智慧型手機？那麼測試環境應該模擬不佳的網路連接。

### Everyone can see what's happening

Continuous Integration is all about communication, so we want to ensure that everyone can easily see the state of the system and the changes that have been made to it.

持續整合的重點在於溝通，因此我們希望確保每個人都能輕鬆查看系統的狀態以及已對其進行的變更。

One of the most important things to communicate is the state of the mainline build. CI Services have dashboards that allow everyone to see the state of any builds they are running. Often they link with other tools to broadcast build information to internal social media tools such as Slack. IDEs often have hooks into these mechanisms, so developers can be alerted while still inside the tool they are using for much of their work. Many teams only send out notifications for build failures, but I think it's worth sending out messages on success too. That way people get used to the regular signals and get a sense for the length of the build. Not to mention the fact that it's nice to get a “well done” every day, even if it's only from a CI server.

傳達主線構建的狀態是溝通中最重要的事情之一。CI 服務提供儀表板，讓每個人都能看到任何正在執行的構建狀態。通常這些儀表板會與其他工具連接，以將構建資訊廣播到內部社交媒體工具（如 Slack）。開發工具（IDE）經常會與這些機制掛鈎，因此開發人員能夠在自己使用的大部分工具內收到提醒。許多團隊僅在構建失敗時發送通知，但我認為成功時也值得發送訊息。這樣人們會習慣定期的信號，並感受到構建的時間長度。更不用說，每天收到 CI 伺服器的一句「幹得好」也是一種鼓勵。

Teams that share a physical space often have some kind of always-on physical display for the build. Usually this takes the form of a large screen showing a simplified dashboard. This is particularly valuable to alert everyone to a broken build, often using the red/green colors on the mainline commit build.

共享實體空間的團隊通常會設置某種始終顯示構建狀態的物理顯示裝置。通常這會是一個大型螢幕，顯示簡化的儀表板，尤其在提醒所有人注意構建失敗時特別有價值，通常透過主線提交構建上的紅/綠色來顯示狀態。
	One of the older physical displays I rather liked were the use of red and green lava lamps. One of the features of a lava lamp is that after they are turned on for a while they start to bubble. The idea w
	
	as that if the red lamp came on, the team should fix the build before it starts to bubble. Physical displays for build status often got playful, adding some quirky personality to a team's workspace. I have fond memories of a dancing rabbit.
	
	我特別喜愛的一種較早期的物理顯示裝置是使用紅綠兩色的熔岩燈。熔岩燈有個特點，就是在開啟一段時間後會開始冒泡。這個設計概念是，如果紅色燈亮起，團隊應該在燈泡開始冒泡前修復構建。這類構建狀態的物理顯示裝置經常很有趣，為團隊工作空間增添了一些怪趣的個性。我對那隻跳舞的兔子依然記憶猶新。

As well as the current state of the build, these displays can show useful information about recent history, which can be an indicator of project health. Back at the turn of the century I worked with a team who had a history of being unable to create stable builds. We put a calendar on the wall that showed a full year with a small square for each day. Every day the QA group would put a green sticker on the day if they had received one stable build that passed the commit tests, otherwise a red square. Over time the calendar revealed the state of the build process showing a steady improvement until green squares were so common that the calendar disappeared - its purpose fulfilled.

除了顯示構建的當前狀態，這些顯示裝置還可以呈現最近的歷史紀錄，這些資訊可以反映專案的健康狀況。世紀之交時，我曾和一個團隊合作過，他們無法穩定地創建構建。我們在牆上放了一個日曆，顯示一整年，每天都有一個小方格。如果 QA 團隊當天收到一個通過提交測試的穩定構建，他們會在那天貼上一個綠色貼紙，否則貼上紅色貼紙。隨著時間的推移，日曆揭示了構建過程的狀況，顯示出穩定性不斷提升，直到綠色方格變得如此普遍，以至於日曆最終被移走——它的任務已經完成。

### Automate Deployment

To do Continuous Integration we need multiple environments, one to run commit tests, probably more to run further parts of the deployment pipeline. Since we are moving executables between these environments multiple times a day, we'll want to do this automatically. So it's important to have scripts that will allow us to deploy the application into any environment easily.

要進行持續整合，我們需要多個環境：一個用來執行提交測試，可能還需要更多環境來進行部署管道的其他部分。由於我們每天會多次在這些環境之間移動可執行文件，因此我們希望這個過程是自動化的。因此，擁有腳本來輕鬆地將應用程式部署到任何環境中是很重要的。

With modern tools for virtualization, containerization, and serverless we can go further. Not just have scripts to deploy the product, but also scripts to build the required environment from scratch. This way we can start with a bare-bones environment that's available off-the-shelf, create the environment we need for the product to run, install the product, and run it - all entirely automatically. If we're using feature flags to hide work-in-progress, then these environments can be set up with all the feature-flags on, so these features can be tested with all immanent interactions.

隨著現代虛擬化、容器化以及無伺服器技術的發展，我們可以更進一步。不僅擁有部署產品的腳本，還有從零構建所需環境的腳本。這樣一來，我們可以從現成的基礎環境開始，自動建立產品運行所需的環境、安裝產品並運行。如果我們使用功能標記來隱藏正在開發的功能，那麼這些環境可以在設定時開啟所有的功能標記，從而可以測試這些功能與即將上線的其他功能之間的互動。

A natural consequence of this is that these same scripts allow us to deploy into production with similar ease. Many teams deploy new code into production multiple times a day using these automations, but even if we choose a less frequent cadence, automatic deployment helps speed up the process and reduces errors. It's also a cheap option since it just uses the same capabilities that we use to deploy into test environments.

自然而然地，這些相同的腳本也讓我們能夠輕鬆部署到生產環境。許多團隊每天多次使用這些自動化腳本將新代碼部署到生產環境中，甚至即便選擇較低頻率的部署節奏，自動部署也能加快流程並減少錯誤。這種方法成本低廉，因為它僅僅使用了相同的能力來部署到測試環境。

If we deploy into production automatically, one extra capability we find handy is automated rollback. Bad things do happen from time to time, and if smelly brown substances hit rotating metal, it's good to be able to quickly go back to the last known good state. Being able to automatically revert also reduces a lot of the tension of deployment, encouraging people to deploy more frequently and thus get new features out to users quickly. [Blue Green Deployment](https://www.martinfowler.com/bliki/BlueGreenDeployment.html) allows us to both make new versions live quickly, and to roll back equally quickly if needed, by shifting traffic between deployed versions.

如果我們將生產環境部署自動化，那麼自動回滾的能力也非常實用。偶爾總會有問題發生，而在緊急情況下，能夠迅速回到最後已知的良好狀態是十分有幫助的。自動回滾的功能大大減少了部署時的壓力，鼓勵人們更頻繁地進行部署，從而更快地向用戶推出新功能。藍綠部署 (Blue Green Deployment) 不僅可以讓新版本快速上線，還能在必要時通過在不同版本之間轉移流量來同樣快速地回滾。

Automated Deployment make it easier to set up [Canary Releases](https://www.martinfowler.com/bliki/CanaryRelease.html), deploying a new version of a product to a subset of our users in order to flush out problems before releasing to the full population.

自動化部署讓金絲雀發布 (Canary Releases) 更加容易實現。金絲雀發布指的是將新版本產品部署給一部分用戶，以便在全面發布前找出潛在問題。

Mobile applications are good examples of where it's essential to automate deployment into test environments, in this case onto devices so that a new version can be explored before invoking the guardians of the App Store. Indeed any device-bound software needs ways to easily get new versions on to test devices.

行動應用程式是一個很好的例子，說明了將新版本自動部署到測試環境（即設備）上是多麼的重要。如此一來，團隊可以在正式提交至應用商店審核之前，進行探索與測試。事實上，任何裝置綁定的軟體都需要便捷的方法來將新版本部署到測試設備上。

When deploying software like this, remember to ensure that version information is visible. An about screen should contain a build id that ties back to version control, logs should make it easy to see which version of the software is running, there should be some API endpoint that will give version information.

在部署此類軟體時，務必確保版本資訊是可見的。應用程式的「關於」頁面應該包含一個可對應回版本控制的建置 ID，日誌中也應該清楚顯示正在運行的軟體版本，此外，應提供一個 API 端點來查詢版本資訊。

## Styles of Integration

Thus far, I've described one way to approach integration, but if it's not universal, then there must be other ways. As with anything, any classification I give has fuzzy boundaries, but I find it useful to think of three styles of handling integration: Pre-Release Integration, Feature Branches, and Continuous Integration.

到目前為止，我已經描述了一種整合方法，但如果它並非普遍適用，那麼就必然存在其他方式。正如任何分類一樣，這些方法之間的界限可能會模糊，但我認為將整合方式分為三種風格是有幫助的：預發布整合（Pre-Release Integration）、功能分支（Feature Branches）以及持續整合（Continuous Integration）。

The oldest is the one I saw in that warehouse in the 80's - **Pre-Release Integration**. This sees integration as a phase of a software project, a notion that is a natural part of a [Waterfall Process](https://www.martinfowler.com/bliki/WaterfallProcess.html). In such a project work is divided into units, which may be done by individuals or small teams. Each unit is a portion of the software, with minimal interaction with other units. These units are built and tested on their own (the original use of the term “unit test”). Then once the units are ready, we integrate them into the final product. This integration occurs once, and is followed by integration testing, and on to a release. Thus if we think of the work, we see two phases, one where everyone works in parallel on features, followed by a single stream of effort at integration.

最早的整合方式就是我在80年代那個倉庫裡見到的——預發布整合（Pre-Release Integration）。這種方法將整合視為軟體項目中的一個階段，是瀑布式開發流程（Waterfall Process）中的自然產物。在這類項目中，工作被分割為多個單元，每個單元可能由個人或小團隊完成。每個單元都是軟體的一部分，與其他單元之間的互動很少。這些單元獨立構建和測試（這也是“單元測試”一詞的最初用途）。當所有單元準備好後，將它們整合到最終產品中。此整合僅發生一次，之後進行整合測試，然後再進行發布。因此，如果我們考慮工作的過程，會看到兩個階段：首先每個人並行開發各自的功能，然後進行一次整合的單一流程。

The frequency of integration in this style is tied to the frequency of release, usually major versions of the software, usually measured in months or years. These teams will use a different process for urgent bug fixes, so they can be released separately to the regular integration schedule.

在這種方式下，整合的頻率與發布的頻率緊密相關，通常是軟體的主要版本發布，時間單位通常以數月或數年計。這類團隊會針對緊急的錯誤修復使用不同的流程，以便能夠在常規的整合排程之外單獨發布。

One of the most popular approaches to integration these days is to use **[Feature Branches](https://www.martinfowler.com/bliki/FeatureBranch.html)**. In this style features are assigned to individuals or small teams, much as units in the older approach. However, instead of waiting until all the units are done before integrating, developers integrate their feature into the mainline as soon as it's done. Some teams will release to production after each feature integration, others prefer to batch up a few features for release.

現在最受歡迎的整合方式之一是使用功能分支（Feature Branches）。在這種方式中，功能會像以前的單元一樣分配給個人或小團隊。然而，開發人員不再等待所有單元完成後才進行整合，而是每當功能完成時就將其整合到主線中。有些團隊會在每次功能整合後立即發布到生產環境，而另一些團隊則偏好將多個功能整合後一併發布。

Teams using feature branches will usually expect everyone to pull from mainline regularly, but this is semi-integration. If Rebecca and I are working on separate features, we might pull from mainline every day, but we don't see each other's changes until one of us completes our feature and integrates, pushing it to the mainline. Then the other will see that code on their next pull, integrating it into their working copy. Thus after each feature is pushed to mainline, every other developer will then do integration work to combine this latest mainline push with their own feature branch.

使用功能分支的團隊通常會要求所有人定期從主線拉取更新，但這僅屬於「半整合」（semi-integration）。如果 Rebecca 和我正在開發不同的功能，我們可能每天都從主線拉取更新，但在其中一人完成功能並整合後將其推送到主線之前，我們無法看到彼此的更改。隨後，另一位成員在下次拉取時會看到這些變更並將其整合到他們的工作副本中。因此，每次功能被推送到主線後，每位開發人員都需要執行整合工作，以將此最新的主線更新與各自的功能分支相結合。

This is only semi-integration because each developer combines the changes on mainline to their own local branch. Full integration can't happen until a developer pushes their changes, causing another round of semi-integrations. Even if Rebecca and I both pull the same changes from mainline, we've only integrated with those changes, not with each other's branches.

這只是半整合，因為每個開發人員將主線上的更改合併到自己的本地分支。完整的整合必須等到開發人員將自己的更改推送到主線上，從而引發另一輪的半整合。即使我和 Rebecca 都從主線拉取了相同的更改，我們也只是在整合這些更改，而不是彼此的分支。

With **Continuous Integration**, every day we are all pushing our changes to the mainline and pulling everyone else's changes into our own work. This leads to many more bouts of integration work, but each bout is much smaller. It's much easier to combine a few hours work on a code base than to combine several days.

在持續整合中，我們每天都將自己的更改推送到主線，並將他人的更改拉取到自己的工作中。這會導致更多次的整合工作，但每次整合的規模都會小得多。將幾個小時的工作合併到代碼庫中要比合併幾天的工作簡單得多。

## Benefits of Continuous Integration

When discussing the relative merits of the three styles of integration, most of the discussion is truly about the [frequency of integration](https://www.martinfowler.com/articles/branching-patterns.html#integration-frequency). Both Pre-Release Integration and Feature Branching can operate at different frequencies and it's possible to change integration frequency without changing the style of integration. If we're using Pre-Release Integration, there's a big difference between monthly releases and annual releases. Feature Branching usually works at a higher frequency, because integration occurs when each feature is individually pushed to mainline, as opposed to waiting to batch a bunch of units together. If a team is doing Feature Branching and all its features are less than a day's work to build, then they are effectively the same as Continuous Integration. But Continuous Integration is different in that it's _defined_ as a high-frequency style. Continuous Integration makes a point of setting integration frequency as a target in itself, and not binding it to feature completion or release frequency.

在討論三種整合方式的相對優缺點時，大部分討論其實都是關於整合頻率。無論是預發布整合還是功能分支，它們都可以在不同頻率下運作，並且可以在不改變整合方式的情況下調整整合頻率。如果採用預發布整合，每月發布和每年發布之間的差異是很大的。功能分支通常在較高的頻率下運作，因為每個功能在推送到主線時就會進行整合，而不是等待將多個單元批量整合在一起。如果一個團隊在使用功能分支並且所有功能的構建時間都少於一天，那麼它們實際上與持續整合相同。但持續整合的不同之處在於，它本身被定義為高頻率的方式，並將整合頻率設置為一個目標，而不是與功能完成或發布頻率綁定。

It thus follows that most teams can see a useful improvement in the factors I'll discuss below by increasing their frequency without changing their style. There are significant benefits to reducing the size of features from two months to two weeks. Continuous Integration has the advantage of setting high-frequency integration as the baseline, setting habits and practices that make it sustainable.

因此，大多數團隊可以通過提高整合頻率而不改變整合方式來看到有用的改善，這些改善將會在接下來討論的幾個因素中體現出來。將功能的開發時間從兩個月縮短到兩週能帶來顯著的好處。持續整合的優勢在於，將高頻率整合作為基準，並建立起能夠持續運行的習慣和實踐。

### Reduced risk of delivery delays

It's very hard to estimate how long it takes to do a complex integration. Sometimes it can be a struggle to merge in git, but then all works well. Other times it can be a quick merge, but a subtle integration bug takes days to find and fix. The longer the time between integrations, the more code to integrate, the longer it takes - but what's worse is the increase in unpredictability.

要估算完成複雜整合所需的時間非常困難。有時候合併程式碼（例如在 Git 中合併）可能很費力，但隨後一切運行良好；而有時候合併很快，但一個微妙的整合錯誤可能需要幾天才能找到並修復。整合之間的間隔時間越長，需整合的程式碼越多，所需時間就越長——更糟糕的是，不可預測性也隨之增加。

This all makes pre-release integration a special form of nightmare. Because the integration is one of the last steps before release, time is already tight and the pressure is on. Having a hard-to-predict phase late in the day means we have a significant risk that's very difficult to mitigate. That was why my 80's memory is so strong, and it's hardly the only time I've seen projects stuck in an integration hell, where every time they fix an integration bug, two more pop up.

這一切讓發佈前的整合成為一種特別的惡夢。由於整合是發佈前的最後幾步之一，時間已經非常緊迫，壓力也很大。在這種情況下，有一個難以預測的階段意味著存在難以緩解的顯著風險。這就是為什麼我對 80 年代的記憶如此深刻，且這絕非我唯一一次見到專案陷入整合地獄的情況——每次修復一個整合錯誤時，又會冒出兩個新的問題。

Any steps to increase integration frequency lowers this risk. The less integration there is to do, the less unknown time there is before a new release is ready. Feature Branching helps by pushing this integration work to individual feature streams, so that, if left alone, a stream can push to mainline as soon as the feature is ready.

任何增加整合頻率的步驟都可以降低此風險。整合工作越少，發佈新版本前的未知時間就越短。功能分支可以透過將整合工作推到各個功能流來幫助實現這一點，這樣一來，只要該功能準備就緒，該分支便可立即推送到主線。

But that _left alone_ point is important. If anyone else pushes to mainline, then we introduce some integration work before the feature is done. Because the branches are isolated, a developer working on one branch doesn't have much visibility about what other features may push, and how much work would be involved to integrate them. While there is a danger that high priority features can face integration delays, we can manage this by preventing pushes of lower-priority features.

但“保持孤立”這一點非常重要。如果其他人推送更改到主線，則在該功能完成之前，會引入一些整合工作。由於分支是獨立的，因此在一個分支上工作的開發人員對其他功能的推送情況和整合所需的工作量並沒有太多的可見性。雖然高優先級的功能可能會面臨整合延遲的風險，但我們可以通過防止低優先級功能的推送來管理這個問題。

Continuous Integration effectively eliminates delivery risk. The integrations are so small that they usually proceed without comment. An awkward integration would be one that takes more than a few minutes to resolve. The very worst case would be conflict that causes someone to restart their work from scratch, but that would still be less than a day's work to lose, and is thus not going to be something that's likely to trouble a board of stakeholders. Furthermore we're doing integration regularly as we develop the software, so we can face problems while we have more time to deal with them and can practice how to resolve them.

持續整合有效地消除了交付風險。由於整合工作量非常小，通常進行得很順利。處理起來比較麻煩的整合，通常也不會超過幾分鐘。最糟糕的情況可能是發生衝突，導致某人需要從頭開始重做工作，但即便如此，也不會超過一天的工作量，因此這類情況不太可能引起利益相關者的擔憂。此外，隨著我們在軟體開發過程中定期進行整合，我們可以在有較多時間處理問題時面對這些挑戰，同時也能累積解決問題的經驗。

Even if a team isn't releasing to production regularly, Continuous Integration is important because it allows everyone to see exactly what the state of the product is. There's no hidden integration efforts that need to be done before release, any effort in integration is already baked in.

即使團隊並非定期發布至生產環境，持續整合仍然非常重要，因為它能讓所有人清楚看到產品的實際狀態。發布前不會存在隱藏的整合工作，所有的整合努力都已經融入日常流程中。

### Less time wasted in integration

I've not seen any serious studies that measure how time spent on integration matches the size of integrations, but my anecdotal evidence strongly suggests that the relationship isn't linear. If there's twice as much code to integrate, it's more likely to be four times as long to carry out the integration. It's rather like how we need three lines to fully connect three nodes, but six lines to connect four of them. Integration is all about connections, hence the non-linear increase, one that's reflected in the experience of my colleagues.

雖然我還沒看過任何嚴謹的研究去衡量整合所需時間與整合規模之間的關係，但根據我的親身經驗強烈顯示，這種關係並非線性的。如果要整合的程式碼量增加一倍，所需的整合時間很可能會增加四倍。這就像我們需要三條線才能完全連接三個節點，但需要六條線才能連接四個節點一樣。整合就是關於連接的過程，因此呈現非線性增長，這點也反映在我同事們的經驗中。

In organizations that are using feature branches, much of this lost time is felt by the individual. Several hours spent trying to rebase on a big change to mainline is frustrating. A few days spent waiting for a code review on a finished pull request, which another big mainline change during the waiting period is even more frustrating. Having to put work on a new feature aside to debug a problem found in an integration test of feature finished two weeks ago saps productivity.

在使用功能分支的組織中，這些浪費的時間大多是由個人承擔的。花費數小時試圖在主線的重大變更上進行 rebase 是令人沮喪的。花費數天等待已完成的 pull request 的程式碼審查，而在等待期間主線又發生重大變更，這更令人沮喪。不得不把新功能的工作擱置一旁，去除錯兩週前完成的功能的整合測試中發現的問題，這會削弱生產力。

When we're doing Continuous Integration, integration is generally a non-event. I pull down the mainline, run the build, and push. If there is a conflict, the small amount of code I've written is fresh in my mind, so it's usually easy to see. The workflow is regular, so we're practiced at it, and we're incentives to automate it as much as possible.

當我們在進行持續整合（Continuous Integration）時，整合通常是一件稀鬆平常的事。我下載主線程式碼、執行建置、然後推送。如果發生衝突，因為我所撰寫的少量程式碼還新鮮地留在腦海中，所以通常很容易理解問題所在。這個工作流程很規律，所以我們對它很熟練，而且我們也有動機去盡可能地將它自動化。

Like many of these non-linear effects, integration can easily become a trap where people learn the wrong lesson. A difficult integration may be so traumatic that a team decides it should do integrations less often, which only exacerbates the problem in the future.

就像許多這樣的非線性效應一樣，整合很容易成為一個讓人吸取錯誤教訓的陷阱。一次困難的整合可能會帶來如此大的創傷，以致於團隊決定減少整合的頻率，但這只會使未來的問題變得更加嚴重。

What's happening here is that we are seeing much closer collaboration between the members of the team. Should two developers make decisions that conflict, we find out when we integrate. So the less time between integrations, the [less time before we detect the conflict](https://www.martinfowler.com/articles/branching-patterns.html#compare-freq), and we can deal with the conflict before it grows too big. With high-frequency integration, our source control system becomes a communication channel, one that can communicate things that can otherwise be unsaid.

這裡發生的是，我們看到團隊成員之間有更緊密的協作。如果兩個開發人員做出相互衝突的決定，我們會在整合時發現。因此，整合之間的時間間隔越短，我們就能越早發現衝突，並且能在衝突變得太大之前解決它。透過高頻率的整合，我們的版本控制系統成為一個溝通管道，能夠傳達一些原本可能不會被說出來的事情。

### Less Bugs

Bugs - these are the nasty things that destroy confidence and mess up schedules and reputations. Bugs in deployed software make users angry with us. Bugs cropping up during regular development get in our way, making it harder to get the rest of the software working correctly.

軟體錯誤（bugs）- 這些惱人的東西會破壞信心，搞亂進度和名譽。已部署軟體中的錯誤會讓使用者對我們生氣。在日常開發過程中出現的錯誤會阻礙我們的工作，使得要讓軟體其餘部分正確運作變得更加困難。

Continuous Integration doesn't get rid of bugs, but it does make them dramatically easier to find and remove. This is less because of the high-frequency integration and more due to the essential introduction of self-testing code. Continuous Integration doesn't work without self-testing code because without decent tests, we can't keep a healthy mainline. Continuous Integration thus institutes a regular regimen of testing. If the tests are inadequate, the team will quickly notice, and can take corrective action. If a bug appears due to a semantic conflict, it's easy to detect because there's only a small amount of code to be integrated. Frequent integrations also work well with [Diff Debugging](https://www.martinfowler.com/bliki/DiffDebugging.html), so even a bug noticed weeks later can be narrowed down to a small change.

持續整合並不能消除錯誤，但它確實讓錯誤變得更容易被發現和移除。這與其說是因為高頻率的整合，不如說是由於引入了自我測試程式碼這個關鍵要素。持續整合若沒有自我測試程式碼就無法運作，因為如果沒有適當的測試，我們就無法維持一個健康的主線。因此，持續整合建立了一個定期的測試制度。如果測試不夠充分，團隊會很快注意到，並且可以採取糾正措施。如果由於語義衝突而出現錯誤，因為每次只需要整合少量程式碼，所以很容易被發現。頻繁的整合也能很好地配合差異除錯（Diff Debugging），因此即使是數週後才發現的錯誤，也能被縮小到一個小範圍的變更中。

Bugs are also cumulative. The more bugs we have, the harder it is to remove each one. This is partly because we get bug interactions, where failures show as the result of multiple faults - making each fault harder to find. It's also psychological - people have less energy to find and get rid of bugs when there are many of them. Thus self-testing code reinforced by Continuous Integration has another exponential effect in reducing the problems caused by defects.

錯誤是會累積的。我們擁有的錯誤越多，要移除每一個錯誤就越困難。這部分是因為我們會遇到錯誤之間的相互作用，失敗可能是多個故障共同造成的結果 - 這使得每個故障都更難被發現。這也有心理因素 - 當錯誤很多時，人們尋找和消除錯誤的精力就會減少。因此，由持續整合所強化的自我測試程式碼在減少缺陷所造成的問題上，也產生了另一個指數效應。

This runs into another phenomenon that many people find counter-intuitive. Seeing how often introducing a change means introducing bugs, people conclude that to have high reliability software they need to slow down the release rate. This was firmly contradicted by the [DORA research program](https://www.martinfowler.com/bliki/StateOfDevOpsReport.html) led by Nicole Forsgren. They found that elite teams deployed to production more rapidly, more frequently, and had a dramatically lower incidence of failure when they made these changes. The research also finds that teams have higher levels of performance when they have three or fewer active branches in the application’s code repository, merge branches to mainline at least once a day, and don’t have code freezes or integration phases.

這與另一個許多人覺得違反直覺的現象有關。看到引入變更多常意味著引入錯誤，人們就認為要擁有高可靠性的軟體，他們需要放慢發布速度。但這個觀點被 Nicole Forsgren 領導的 DORA 研究計畫明確地否定了。他們發現，精英團隊更快速、更頻繁地部署到生產環境，而且在進行這些變更時，故障發生率大幅降低。研究還發現，當團隊在應用程式的程式碼儲存庫中只有三個或更少的活躍分支、每天至少將分支合併到主線一次，並且沒有程式碼凍結或整合階段時，他們能達到更高的績效水準。

### Enables Refactoring for sustained productivity

Most teams observe that over time, codebases deteriorate. Early decisions were good at the time, but are no longer optimal after six month's work. But changing the code to incorporate what the team has learned means introducing changes deep in the existing code, which results in difficult merges which are both time-consuming and full of risk. Everyone recalls that time someone made what would be a good change for the future, but caused days of effort breaking other people's work. Given that experience, nobody wants to rework the structure of existing code, even though it's now awkward for everyone to build on, thus slowing down delivery of new features.

大多數團隊都觀察到，隨著時間推移，程式碼庫會逐漸退化。早期的決策在當時是好的，但在六個月的工作之後就不再是最佳選擇了。要將團隊所學到的知識運用到程式碼中，意味著必須在現有程式碼的深處進行修改，這會導致困難的合併，不僅耗時且充滿風險。每個人都記得那次有人為了未來做出本該是好的變更，卻導致花費數天的努力去修復其他人的工作。有了這樣的經驗，即使現有的程式碼結構讓每個人在開發新功能時都感到彆扭而拖慢了進度，也沒有人願意重新調整現有程式碼的結構。

Refactoring is an essential technique to attenuate and indeed reverse this process of decay. A team that refactors regularly has a disciplined technique to improve the structure of a code base by using small, behavior-preserving transformations of the code. These characteristics of the transformations greatly reduce their chances of introducing bugs, and they can be done quickly, especially when supported by a foundation of self-testing code. Applying refactoring at every opportunity, a team can improve the structure of an existing codebase, making it easier and faster to add new capabilities.

重構是一種減緩甚至逆轉這種衰退過程的重要技術。定期進行重構的團隊擁有一套有紀律的技術，透過對程式碼進行小規模、保持行為不變的轉換來改善程式碼庫的結構。這些轉換的特性大大降低了引入錯誤的可能性，而且在自我測試程式碼的基礎支持下，可以快速完成。團隊在每個可能的機會都進行重構，就能改善現有程式碼庫的結構，使添加新功能變得更容易、更快速。

But this happy story can be torpedoed by integration woes. A two week refactoring session may greatly improve the code, but result in long merges because everyone else has been spending the last two weeks working with the old structure. This raises the costs of refactoring to prohibitive levels. Frequent integration solves this dilemma by ensuring that both those doing the refactoring and everyone else are regularly synchronizing their work. When using Continuous Integration, if someone makes intrusive changes to a core library I'm using, I only have to adjust a few hours of programming to these changes. If they do something that clashes with the direction of my changes, I know right away, so have the opportunity to talk to them so we can figure out a better way forward.

然而，整合問題可能讓這一切變得複雜。一場為期兩週的重構或許能大幅改善代碼質量，但可能會導致冗長的合併，因為其他成員這段期間仍在使用舊結構開發。這樣一來，重構成本可能變得難以承受。頻繁的整合可以解決這個問題，因為無論是進行重構的人還是其他成員，都能定期同步各自的工作。在使用持續整合的情況下，如果有人對我使用的核心庫做出重大更改，我只需調整幾小時的程式碼即可。如果他們的變更和我的修改方向有衝突，我可以立即得知，從而有機會與他們溝通，找出更好的解決方案。

So far in this article I've raised several counter-intuitive notions about the merits of high-frequency integration: that the more often we integrate, the less time we spend integrating, and that frequent integration leads to less bugs. Here is perhaps the most important counter-intuitive notion in software development: that teams that spend a lot of effort keeping their code base healthy [deliver features faster and cheaper](https://www.martinfowler.com/articles/is-quality-worth-cost.html). Time invested in writing tests and refactoring delivers impressive returns in delivery speed, and Continuous Integration is a core part of making that work in a team setting.

到目前為止，在本文中我提出了幾個關於高頻率整合優點的反直覺觀點：整合越頻繁，我們花在整合上的時間就越少，而頻繁的整合會導致更少的錯誤。這裡或許是軟體開發中最重要的反直覺觀點：花大量精力保持代碼庫健康的團隊，交付功能的速度更快、成本更低。在編寫測試和重構上所投入的時間，能顯著提升交付速度，而持續整合是讓團隊中這些工作發揮作用的核心要素。

### Release to Production is a business decision

Imagine we are demonstrating some newly built feature to a stakeholder, and she reacts by saying - “this is really cool, and would make a big business impact. How long before we can make this live?” If that feature is being shown on an unintegrated branch, then the answer may be weeks or months, particularly if there is poor automation on the path to production. Continuous Integration allows us to maintain a [Release-Ready Mainline](https://www.martinfowler.com/articles/branching-patterns.html#release-ready-mainline), which means the decision to release the latest version of the product into production is purely a business decision. If the stakeholders want the latest to go live, it's a matter of minutes running an automated pipeline to make it so. This allows the customers of the software greater control of when features are released, and encourages them to collaborate more closely with the development team.

想像一下，我們正在向一位利害關係人展示一個新開發的功能，她的反應是說：「這真的很酷，並且將對業務產生重大影響。我們還需要多久才能讓這功能上線？」如果這個功能是在未整合的分支上展示的，那麼答案可能是幾週甚至幾個月，特別是在通往生產環境的路徑上缺乏良好自動化的情況下。而持續整合讓我們能夠維持一個隨時可發布的主線，這意味著產品最新版本是否上線，純粹取決於業務決策。如果利害關係人希望讓最新功能上線，只需在幾分鐘內運行自動化流程即可。這讓軟體的客戶更能掌控功能發布時機，並鼓勵他們與開發團隊更密切地合作。

Continuous Integration and a Release-Ready Mainline removes one of the biggest barriers to frequent deployment. Frequent deployment is valuable because it allows our users to get new features more rapidly, to give more rapid feedback on those features, and generally become more collaborative in the development cycle. This helps break down the barriers between customers and development - barriers which I believe are the biggest barriers to successful software development.

持續整合和隨時可發布的主線消除了頻繁部署的最大障礙之一。頻繁部署的價值在於能讓用戶更快地獲得新功能，並能更快地對這些功能提供回饋，從而更加參與到開發周期中。這有助於打破客戶與開發團隊之間的障礙，而我相信這些障礙正是成功軟體開發的最大障礙。

## When we should _not_ use Continuous Integration

All those benefits sound rather juicy. But folks as experienced (or cynical) as I am are always suspicious of a bare list of benefits. Few things come without a cost, and decisions about architecture and process are usually a matter of trade-offs.

所有這些好處聽起來相當誘人。但像我這樣有經驗（或說多疑）的人，對單列好處總是抱持懷疑的態度。很少有事情是沒有成本的，而架構和流程的決策通常都是權衡取捨的問題。

But I confess that Continuous Integration is one of those rare cases where there's little downside for a committed and skillful team to utilize it. The cost imposed by sporadic integration is so great, that almost any team can benefit by increasing their integration frequency. There is some limit to when the benefits stop piling up, but that limit sits at hours rather than days, which is exactly the territory of Continuous Integration. The interplay between self-testing code, Continuous Integration, and Refactoring is particularly strong. We've been using this approach for two decades at Thoughtworks, and our only question is how to do it more effectively - the core approach is proven.

但我必須承認，對於一支投入且技術精湛的團隊來說，持續整合是少數幾乎沒有缺點的案例之一。零星整合帶來的成本非常高，因此幾乎任何團隊都能透過增加整合頻率獲益。雖然好處的增長會有一個極限，但這個極限落在「小時」而非「天」的範圍內，這正是持續整合的領域。自測代碼、持續整合與重構之間的互動尤其強大。我們在 Thoughtworks 已經使用這種方法二十多年了，我們唯一的問題是如何更有效地執行——這個核心方法已被證明有效。

But that doesn't mean that Continuous Integration is for everyone. You might notice that I said that “there’s little downside for a _committed and skillful_ team to utilize it”. Those two adjectives indicate the contexts where Continuous Integration isn't a good fit.

但這並不代表持續整合適合所有人。你可能注意到我提到「對於一支投入且技術精湛的團隊來說，持續整合幾乎沒有缺點」。這兩個形容詞表明了在某些情境下，持續整合並不是理想的選擇。

By “committed”, I mean a team that's working full-time on a product. A good counter-example to this is a classical open-source project, where there is one or two maintainers and many contributors. In such a situation even the maintainers are only doing a few hours a week on the project, they don't know the contributors very well, and don't have good visibility for when contributors contribute or the standards they should follow when they do. This is the environment that led to a feature branch workflow and pull-requests. In such a context Continuous Integration isn't plausible, although efforts to increase the integration frequency can still be valuable.

「投入」意指一個全職投入於產品開發的團隊。一個典型的反例是傳統的開源專案，這類專案通常有一兩個維護者和許多貢獻者。在這種情況下，甚至維護者每週只花幾個小時在專案上，他們對貢獻者的瞭解有限，也無法掌握貢獻者的貢獻時間及應遵循的標準。這樣的環境促使了特性分支工作流程和拉取請求的誕生。在這種情境下，持續整合並不現實，儘管提高整合頻率的努力仍然具有價值。

Continuous Integration is more suited for team working full-time on a product, as is usually the case with commercial software. But there is much middle ground between the classical open-source and the full-time model. We need to use our judgment about what integration policy to use that fits the commitment of the team.

持續整合更適合全職投入於產品開發的團隊，這通常出現在商業軟體開發中。不過，在傳統的開源專案與全職模式之間，仍有很大的中間地帶。我們需要根據團隊的投入程度來判斷應使用何種整合策略，以符合團隊的需求。

The second adjective looks at the skill of the team in following the necessary practices. If a team attempts Continuous Integration without a strong test suite, they will run into all sorts of trouble because they don't have a mechanism for screening out bugs. If they don't automate, integration will take too long, interfering with the flow of development. If folks aren't disciplined about ensuring their pushes to mainline are done with green builds, then the mainline will end up broken all the time, getting in the way of everyone's work.

第二個形容詞則著眼於團隊在遵循必要實踐方面的技能。如果團隊在缺乏強大測試套件的情況下嘗試持續整合，將會遇到各種麻煩，因為沒有有效的機制篩選出錯誤。如果沒有自動化，整合過程會變得過於耗時，進而干擾開發流程。如果團隊成員沒有嚴格遵守推送至主線時的綠色構建原則，那麼主線將經常處於中斷狀態，阻礙每個人的工作。

Anyone who is considering introducing Continuous Integration has to bear these skills in mind. Instituting Continuous Integration without self-testing code won't work, and it will also give a inaccurate impression of what Continuous Integration is like when it's done well.

任何正在考慮引入持續整合的人都必須考慮到這些技能。如果沒有自我測試的程式碼,建立持續整合是行不通的,這也會給人一個錯誤的印象,認為持續整合做得很好。

That said, I don't think the skill demands are particularly hard. We don't need rock-star developers to get this process working in a team. (Indeed rock-star developers are often a barrier, as people who think of themselves that way usually aren't very disciplined.) The skills for these technical practices aren't that hard to learn, usually the problem is finding a good teacher, and forming the habits that crystallize the discipline. Once the team gets the hang of the flow, it usually feels comfortable, smooth - and fast.

雖然如此說,但我並不認為技能要求特別困難。要讓這個流程在團隊中運作,我們不需要超級明星級的開發者。(事實上,超級明星級的開發者常常是障礙,因為那些認為自己如此的人通常缺乏紀律性。)這些技術實踐的技能並不難學習,通常問題在於找到好的老師,培養那些使紀律定型的習慣。一旦團隊掌握了這種流程,通常會覺得很舒適、順暢,而且速度也很快。

## Introducing Continuous Integration

One of the hard things about describing how to introduce a practice like Continuous Integration is that the path depends very much on where you're starting. Writing this, I don't know what kind code you are working on, what skills and habits your team possesses, let alone the broader organizational context. All anyone like me can do is point out some common signposts, in the hope that it will help you find your own path.

要描述如何引入像持續整合這樣的實踐,其中一個困難之處在於,這條路徑很大程度上取決於你的起點。在寫這篇文章時,我不知道你正在處理什麼樣的代碼,你的團隊擁有什麼樣的技能和習慣,更不用說更廣泛的組織環境。像我這樣的人所能做的就是指出一些常見的指標,希望這能幫助你找到自己的道路。

When introducing any new practice, it's important to be clear on why we're doing it. My list of benefits above includes the most common reasons, but different contexts lead to a different level of importance for them. Some benefits are harder to appreciate than others. Reducing waste in integration addresses a frustrating problem, and can be easily sensed as we make progress. Enabling refactoring to reduce the cruft in a system and improve overall productivity is more tricky to see. It takes time before we see an effect, and it's hard to compare to the counter-factual. Yet this is probably the most valuable benefit of Continuous Integration.

引入任何新的實踐時,明確我們這麼做的原因都很重要。我之前列舉的那些好處是最常見的,但不同的背景會導致它們的重要性程度不同。有些好處比其他的更難以體現。減少集成過程中的浪費解決了一個令人沮喪的問題，當我們取得進展時也很容易感受到。而使重構更加容易以減少系統中的雜質，提高整體生產力則更難察覺。需要一段時間才能看到效果，很難與反面情況進行比較。但這可能是持續集成最有價值的好處。

The list of practices above indicate the skills a team needs to learn in order to make Continuous Integration work. Some of these can bring value even before we get close to the high integration frequency. Self-testing code adds stability to a system even with infrequent commits.

上面列出的實踐清單表示了團隊需要學習的技能，才能使持續集成發揮作用。即使在頻繁集成之前,其中一些也能帶來價值。自我測試代碼即使在提交頻率低的情況下，也能為系統增加穩定性。

One target can be to double the integration frequency. If feature branches typically run for ten days, figure out how to cut them down to five. This may involve better build and test automation, and creative thinking on how a large task can be split into smaller, independently integrated tasks. If we use pre-integration reviews, we could include explicit steps in those reviews to check test coverage and encourage smaller commits.

一個可行的目標是將集成頻率翻倍。如果特性分支通常需要10天,則要想辦法將其縮短到5天。這可能涉及更好的構建和測試自動化，以及如何將一項大任務分拆成更小、可獨立集成的任務的創造性思維。如果我們使用集成前審查，可以在審查過程中包括明確的步驟來檢查測試覆蓋率,並鼓勵提交更小的變更。

If you are starting a new project, we can begin with Continuous Integration from the beginning. We should keep an eye on build times and take action as soon as we start going slower than the ten minute rule. By acting quickly we'll make the necessary restructurings before the code base gets so big that it becomes a major pain.

如果是開始一個新專案，我們可以從一開始就採用持續集成。我們應該留意構建時間,一旦開始超過十分鐘的規則,就要立即採取行動。通過及時行動，我們將在代碼庫變得太大而成為痛苦之源之前，進行必要的重構。

Above all we should get some help. We should find someone who has done Continuous Integration before to help us. Like any new technique it's hard to introduce it when we don't know what the final result looks like. It may cost money to get this support, but we'll otherwise pay in lost time and productivity. (Disclaimer / Advert - yes we at Thoughtworks do some consultancy in this area. After all we've made most of the mistakes that there are to make.)

總之我們應該尋求幫助。我們應該找到之前有持續集成經驗的人來幫助我們。就像任何新技術一樣,如果我們不知道最終效果是什麼樣子，要引入它是很困難的。獲得這種支援可能需要花錢，但如果不這樣做，我們最終會付出更多的時間和生產力。(免責聲明/廣告 - 是的，我們Thoughtworks公司在這個領域提供一些諮詢服務。畢竟，我們已經犯過大多數能犯的錯誤。)

## Common Questions

### Where did Continuous Integration come from?

Continuous Integration was developed as a practice by Kent Beck as part of Extreme Programming in the 1990s. At that time pre-release integration was the norm, with release frequencies often measured in years. There had been a general push to iterative development, with faster release cycles. But few teams were thinking in weeks between releases. Kent defined the practice, developed it with projects he worked on, and established how it interacted with the other key practices upon which it relies.

持續集成這種實踐是由肯特·貝克於1990年代作為極限編程的一部分而開發出來的。那時，預發布集成是常態，發布頻率通常以年為單位。人們正在推動敏捷開發，以較快的發布週期。但很少有團隊會考慮每隔幾周就發布一次。肯特定義了這種實踐，並在自己參與的專案中進行開發，確立了它與其依賴的其他關鍵實踐之間的相互作用。

>	Microsoft had been known for doing daily builds (usually overnight), but without the testing regimen  or the focus on fixing defects that are such crucial elements of Continuous Integration.
>	
>	Some people credit Grady Booch for coining the term, but he only used the phrase as an offhand description in a single sentence in his object-oriented design book. He did not treat it as a defined practice, indeed it didn't appear in the index.
>	
>	微軟曾以每日構建(通常在夜間)而聞名，但缺乏持續集成如此關鍵的測試機制和修復缺陷的重點。
>	
>	有人認為格雷迪·布奇(Grady Booch)是創造這個詞的人，但他只是在物件導向設計的一本書中隨意提到過這個短語。他沒有把它當做一種定義的實踐，事實上它也沒有出現在書籍的索引中。

### What is the difference between Continuous Integration and Trunk-Based Development?

As CI Services became popular, many people used them to run regular builds on feature branches. This, as explained above, isn't Continuous Integration at all, but it led to many people saying (and thinking) they were doing Continuous Integration when they were doing something significantly different, which causes a lot of confusion.

隨著持續集成(CI)服務越來越流行，許多人開始在功能分支上執行定期構建。如上所述，這並不是真正的持續集成，但它導致許多人說(並認為)他們正在進行持續集成，而實際上卻在做與之完全不同的事情，造成了很多混淆。

Some folks decided to tackle this [Semantic Diffusion](https://www.martinfowler.com/bliki/SemanticDiffusion.html) by coining a new term: Trunk-Based Development. In general I see this as a synonym to Continuous Integration and acknowledge that it doesn't tend to suffer from confusion with “running Jenkins on our feature branches”. I've read some people trying to formulate some distinction between the two, but I find these distinctions are neither consistent nor compelling.

有些人決定通過創造一個新術語來解決這個語義差異問題：主幹開發(Trunk-Based Development)。一般來說，我認為這可以視為持續集成的同義詞，它不太容易被誤解為"在我們的功能分支上運行Jenkins"。我讀到有些人試圖在這兩者之間找出一些區別，但我發現這些區別既不一致也缺乏說服力。

I don't use the term Trunk-Based Development, partly because I don't think coining a new name is a good way to counter semantic diffusion, but mostly because renaming this technique rudely erases the work of those, especially Kent Beck, who championed and developed Continuous Integration in the beginning.

我不使用"主幹開發"這個術語，部分原因是我不認為創造新名稱是遏制語義擴散的好方法，但主要原因是重新命名這種技術粗暴地抹去了那些，特別是肯特·貝克,最初倡導和發展持續集成的人的貢獻。

Despite me avoiding the term, there is a lot of good information about Continuous Integration that's written under the flag of Trunk-Based Development. In particular, Paul Hammant has written a lot of excellent material on his [website](https://trunkbaseddevelopment.com/).

儘管我避免使用"主幹開發"這個術語，但在這個旗幟下也有大量優秀的持續集成相關資訊。特別是保羅·哈曼特(Paul Hammant)在他的網站上撰寫了很多出色的資料。

### Can we run a CI Service on our feature branches?

The simple answer is “yes - but you're _not_ doing Continuous Integration”. The key principle here is that “Everyone Commits To the Mainline Every Day”. Doing an automated build on feature branches is useful, but it is only [semi-integration](https://www.martinfowler.com/articles/continuousIntegration.html#semi-integration).

簡單的答案是"是的 - 但你並不是在做持續集成"。這裡的關鍵原則是"每個人每天都提交到主線"。在功能分支上進行自動化構建是有用的，但這只是半集成。

However it is a common confusion that using a daemon build in this way is what Continuous Integration is about. The confusion comes from calling these tools Continuous Integration Services, a better term would be something like “Continuous Build Services”. While using a CI Service is a useful aid to doing Continuous Integration, we shouldn't confuse a tool for the practice.

然而，將這種使用守護進程構建的方式稱為持續集成是一個常見的混淆。這種混淆來自於將這些工具稱為持續集成服務，一個更好的術語應該是"持續構建服務"之類的。雖然使用CI服務是進行持續集成的有用輔助，但我們不應將工具與實踐混淆。

### Can a team do both Continuous Integration and Feature Branching at the same time?

In general, Continuous Integration and Feature Branching are mutually exclusive approaches. Most folks who think they are doing both are running a CI Service on their Feature Branches, which as I explain in the previous question, isn't doing Continuous Integration.

一般來說，持續整合和功能分支是互斥的兩種方法。大多數認為自己在執行兩者的團隊，實際上是在功能分支上運行 CI 服務，但正如我在前文所解釋的，那並不是真正的持續整合。

There is one situation where it is possible to do both, that is when all the features are so small they can be completed in less than a day. But that seems to be a very rare case, and most people would just call that Continuous Integration.

有一種情況下是可以同時進行兩者的，即所有功能都足夠小，可以在一天內完成。但這樣的情況非常罕見，大多數人會直接將其稱為持續整合。

A secondary point here is that it's perfectly permissible to do personal work on a separate branch, then merge it with main and push when I integrate. I might do that if I were worried I'd fat-finger my IDE and push a broken local main by accident. The key question is whether I'm integrating continuously, not how I manage my personal workspace.

這裡有一個次要重點，那就是在個人分支上工作是完全允許的，然後在整合時將它合併到主幹並推送。如果我擔心自己在整合過程中會誤操作 IDE 而推送一個有問題的本地主幹分支，我可能會這麼做。關鍵問題在於我是否在持續整合，而不是如何管理我的個人工作區。

### What is the difference between Continuous Integration and Continuous Delivery?

The early descriptions of Continuous Integration focused on the cycle of developer integration with the mainline in the team's development environment. Such descriptions didn't talk much about the journey from an integrated mainline to a production release. That doesn't mean they weren't in people's minds. Practices like “Automate Deployment” and “Test in a Clone of the Production Environment” clearly indicate a recognition of the path to production.

早期對持續整合的描述主要集中在開發人員在團隊開發環境中與主幹的整合循環。這些描述並沒有太多談及從整合完成的主幹到生產環境的釋出過程。這並不意味著當時人們沒有意識到這一點。「自動化部署」和「在複製的生產環境中測試」等實踐明確表明了對進入生產環境路徑的認識。

In some situations, there wasn't much else after mainline integration. I recall Kent showing me a system he was working on in Switzerland in the late 90's where they deployed to production, every day, automatically. But this was a Smalltalk system, that didn't have complicated steps for a production deploy. In the early 2000s at Thoughtworks, we often had situations where that path to production was much more complicated. This led to the notion that there was an activity beyond Continuous Integration that addressed that path. That activity came to knows as Continuous Delivery.

在某些情況下，主幹整合之後就沒有太多後續步驟了。我記得在 90 年代末，Kent 曾向我展示他在瑞士開發的一個系統，他們每天自動部署到生產環境。但這是個 Smalltalk 系統，生產部署並不複雜。到了 2000 年代初期，在 Thoughtworks，我們經常遇到生產環境的路徑更加複雜的情況。這促使了一種超越持續整合、專注於部署過程的活動的出現，這項活動後來被稱為持續交付。

The aim of Continuous Delivery is that the product should always be in a state where we can release the latest build. This is essentially ensuring that the release to production is a business decision.

持續交付的目標是使產品始終處於隨時可發布的狀態，確保最新的構建版本可以隨時發布到生產環境。本質上，這是將發布決策交由業務層面來決定。

For many people these days, Continuous Integration is about integrating code to the mainline in the development team's environment, and Continuous Delivery is the rest of the deployment pipeline heading to a production release. Some people treat Continuous Delivery as encompassing Continuous Integration, others see them as closely linked partners, often with the moniker CI/CD. Others argue that Continuous Delivery is merely a synonym for Continuous Integration.

如今，對許多人而言，持續集成（Continuous Integration）指的是在開發團隊的環境中將代碼集成到主線，而持續交付（Continuous Delivery）則涵蓋了剩餘的部署流程，直至產品發布。一些人認為持續交付包含了持續集成，另一些人則將它們視為密切相關的夥伴，常用「CI/CD」來指代。還有人主張持續交付只是持續集成的同義詞。

### How does Continuous Deployment fit in with all this?

Continuous Integration ensures everyone integrates their code at least daily to the mainline in version control. Continuous Delivery then carries out any steps required to ensure that the product is releasable to product whenever anyone wishes. Continuous Deployment means the product is automatically released to production whenever it passes all the automated tests in the deployment pipeline.

持續集成（Continuous Integration）確保每個人每天至少將自己的代碼集成到版本控制的主線中。持續交付（Continuous Delivery）則進一步執行任何必要的步驟，以確保產品隨時處於可發布的狀態，以便在任何人需要時可以發布。持續部署（Continuous Deployment）則指的是，只要產品通過了部署流程中的所有自動化測試，它就會自動發布到生產環境。

With Continuous Deployment every commit pushed to mainline as part of Continuous Integration will be automatically deployed to production providing all of the verifications in the deployment pipeline are green. Continuous Delivery just assures that this is possible (and is thus a pre-requisite for Continuous Deployment).

在持續部署（Continuous Deployment）中，每次提交並推送到主線作為持續集成的一部分，只要部署流程中的所有驗證都是通過的，就會自動部署到生產環境。而持續交付（Continuous Delivery）僅確保這種情況是可行的，因此它是實現持續部署的前提條件。

### How do we do pull requests and code reviews?

[Pull Requests](https://www.martinfowler.com/bliki/PullRequest.html), an artifact of GitHub, are now widely used on software projects. Essentially they provide a way to add some process to the push to mainline, usually involving a [pre-integration code review](https://www.martinfowler.com/articles/branching-patterns.html#reviewed-commits), requiring another developer to approve before the push can be accepted into the mainline. They developed mostly in the context of feature branching in open-source projects, ensuring that the maintainers of a project can review that a contribution fits properly into the style and future intentions of the project.

Pull Requests，是 GitHub 引入的一種工具，如今已在許多軟體項目中廣泛使用。其基本作用是為推送到主線的過程增添一些流程控制，通常包括集成前的代碼審查，要求其他開發者在推送的變更被接受到主線之前進行批准。Pull Requests 主要在開源項目的功能分支（feature branching）中發展而來，確保項目的維護者能夠審查貢獻的內容是否符合項目的風格和未來方向。

The pre-integration code review can be problematic for Continuous Integration because it usually adds significant friction to the integration process. Instead of an automated process that can be done within minutes, we have to find someone to do the code review, schedule their time, and wait for feedback before the review is accepted. Although some organizations may be able to get to flow within minutes, this can easily end up being hours or days - breaking the timing that makes Continuous Integration work.

預先整合的程式碼審查對持續整合來說可能很麻煩，因為它通常會給整合過程增添顯著的阻力。不同於可以在幾分鐘內完成的自動化過程，我們必須找人進行程式碼審查、安排他們的時間，並在審查被接受之前等待回饋。雖然有些組織可能能夠在幾分鐘內完成這個流程，但這很容易最終變成數小時或數天的等待 - 打破了使持續整合發揮作用的時間節奏。

Those who do Continuous Integration deal with this by reframing how code review fits into their workflow. [Pair Programming](https://www.martinfowler.com/bliki/PairProgramming.html) is popular because it creates a continuous real-time code review as the code is being written, producing a much faster feedback loop for the review. The [Ship / Show / Ask](https://www.martinfowler.com/articles/ship-show-ask.html) process encourages teams to use a blocking code review only when necessary, recognizing that post-integration review is often a better bet as it doesn't interfere with integration frequency. Many teams find that [Refinement Code Review](https://www.martinfowler.com/bliki/RefinementCodeReview.html) is an important force to maintaining a healthy code base, but works at its best when Continuous Integration produces an environment friendly to refactoring.

那些實施持續整合的人們透過重新定義程式碼審查如何融入他們的工作流程來解決這個問題。結對編程很受歡迎，因為它在程式碼編寫的同時就進行持續的即時程式碼審查，為審查創造了一個更快的回饋循環。Ship / Show / Ask（發布/展示/詢問）流程鼓勵團隊只在必要時才使用阻塞式的程式碼審查，認識到整合後的審查通常是更好的選擇，因為它不會干擾整合頻率。許多團隊發現改進型程式碼審查是維持健康程式碼庫的重要力量，但只有當持續整合創造出一個有利於重構的環境時，它才能發揮最佳效果。

We should remember that pre-integration review grew out of an open-source context where contributions appear impromptu from weakly connected developers. Practices that are effective in that environment need to be reassessed for a full-time team of closely-knit staff.

我們應該記住，預先整合審查是從開源環境中發展出來的，在那裡貢獻是由鬆散連結的開發者即興提供的。在那種環境中有效的做法，需要在一個由緊密合作的全職員工組成的團隊中重新評估。

### How do we handle databases?

Databases offer a specific challenge as we increase integration frequency. It's easy to include database schema definitions and load scripts for test data in the version-controlled sources. But that doesn't help us with data outside of version-control, such as production databases. If we change the database schema, we need to know how to handle existing data.

當我們增加整合頻率時，資料庫提供了一個特殊的挑戰。在版本控制的源碼中包含資料庫架構定義和測試資料的載入腳本是很容易的。但這對於版本控制之外的資料（比如生產資料庫）並沒有幫助。如果我們改變資料庫架構，我們需要知道如何處理現有的資料。

With traditional pre-release integration, data migration is a considerable challenge, often spinning up special teams just to carry out the migration. At first blush, attempting high-frequency integration would introduce an untenable amount of data migration work.

在傳統的發布前整合中，資料遷移是一個相當大的挑戰，經常需要組建特別的團隊來執行遷移工作。乍看之下，嘗試高頻率的整合似乎會帶來難以承受的大量資料遷移工作。

In practice, however, a change in perspective removes this problem. We faced this issue in Thoughtworks on our early projects using Continuous Integration, and solved it by shifting to an [Evolutionary Database Design](https://www.martinfowler.com/articles/evodb.html) approach, developed by my colleague Pramod Sadalage. The key to this methodology is to define database schema and data through a series of migration scripts, that alter both the database schema and data. Each migration is small, so is easy to reason about and test. The migrations compose naturally, so we can run hundreds of migrations in sequence to perform significant schema changes and migrate the data as we go. We can store these migrations in version-control in sync with the data access code in the application, allowing us to build any version of the software, with the correct schema and correctly structured data. These migrations can be run on test data, and on production databases.

然而在實踐中，改變觀點就能消除這個問題。我們在 Thoughtworks 早期使用持續整合的專案中就面臨這個問題，並透過採用由我的同事 Pramod Sadalage 開發的演進式資料庫設計（Evolutionary Database Design）方法來解決它。這個方法的關鍵是透過一系列的遷移腳本來定義資料庫架構和資料，這些腳本可以同時修改資料庫架構和資料。每個遷移都很小，所以容易理解和測試。這些遷移自然地組合在一起，所以我們可以按順序執行數百個遷移，在過程中完成重要的架構變更並遷移資料。我們可以將這些遷移與應用程式中的資料存取程式碼同步存儲在版本控制中，使我們能夠建構軟體的任何版本，並具有正確的架構和正確結構的資料。這些遷移可以在測試資料上執行，也可以在生產資料庫上執行。
## Final Thoughts

Most software development is about changing existing code. The cost and response time for adding new features to a code base depends greatly upon the condition of that code base. [A crufty code base is harder and more expensive to modify.](https://www.martinfowler.com/articles/is-quality-worth-cost.html) To keep cruft to a minimum a team needs to be able to regularly refactor the code, changing its structure to reflect changing needs and incorporate lessons the team learns from working on the product.

大多數的軟體開發都是關於修改現有的程式碼。在程式碼庫中添加新功能的成本和回應時間，很大程度上取決於該程式碼庫的狀況。一個雜亂的程式碼庫更難修改，也更昂貴。為了將雜亂程度降到最低，團隊需要能夠定期重構程式碼，改變其結構以反映不斷變化的需求，並將團隊在產品開發過程中學到的經驗融入其中。

Continuous Integration is vital for a healthy product because it is a key component of this kind of evolutionary design ecosystem. Together with and supported by self-testing code, it's the underpinning for refactoring. These technical practices, born together in Extreme Programming, can enable a team to deliver regular enhancement of a product to take advantage of changing needs and technological opportunities.

持續整合對於一個健康的產品來說是至關重要的，因為它是這種演進式設計生態系統的關鍵組成部分。與自我測試程式碼一起，並在其支持下，它是重構的基礎。這些在極限編程中一同誕生的技術實踐，能使團隊定期改進產品，以利用不斷變化的需求和技術機會。

---
## Further Reading

An essay like this can only cover so much ground, but this is an important topic so I've created a [guide page](https://www.martinfowler.com/delivery.html) on my website to point you to more information.

To explore Continuous Integration in more detail I suggest taking a look at Paul Duvall's [appropriately titled book](https://www.martinfowler.com/books/duvall.html) on the subject (which won a Jolt award - more than I've ever managed). For more on the broader process of Continuous Delivery, take a look at [Jez Humble and Dave Farley's book](https://www.martinfowler.com/books/continuousDelivery.html) - which also beat me to a Jolt award.

My article on [Patterns for Managing Source Code Branches](https://www.martinfowler.com/articles/branching-patterns.html) looks at the broader context, showing how Continuous Integration fits into the wider decision space of choosing a branching strategy. As ever, the driver for choosing when to branch is knowing you are going to integrate.

The [original article on Continuous Integration](https://www.martinfowler.com/articles/originalContinuousIntegration.html) describes our experiences as Matt helped put together continuous integration on a Thoughtworks project in 2000.

As I [indicated earlier](https://www.martinfowler.com/articles/continuousIntegration.html#trunk-based), many people write about Continuous Integration using the term “Trunk-Based Development”. Paul Hammant's [website](https://trunkbaseddevelopment.com/) contains a lot of useful and practical information. Clare Sudbery recently wrote an [informative report](https://www.oreilly.com/library/view/what-is-trunk-based/9781098146658/) available through O'Reilly.

像這樣的文章只能涵蓋這麼多內容，但這是一個重要的主題，所以我在我的網站上創建了一個指南頁面來為您提供更多資訊。

要更詳細地探索持續整合，我建議您看看 Paul Duvall 在這個主題上的專書（它贏得了 Jolt 獎 - 比我所獲得的還多）。要了解更多關於持續交付這個更廣泛的流程，可以看看 Jez Humble 和 Dave Farley 的書 - 它也在我之前獲得了 Jolt 獎。

我關於管理源碼分支模式的文章探討了更廣泛的背景，展示了持續整合如何融入選擇分支策略的更大決策空間。一如既往，選擇何時建立分支的驅動因素是知道你要進行整合。

關於持續整合的原始文章描述了我們在 2000 年當 Matt 幫助在 Thoughtworks 專案中建立持續整合時的經驗。

如我先前提到的，許多人在寫持續整合時使用「主幹開發」（Trunk-Based Development）這個詞。Paul Hammant 的網站包含了許多有用且實用的資訊。Clare Sudbery 最近寫了一份可以透過 O'Reilly 獲得的資訊豐富的報告。