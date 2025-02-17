---
date: '2025-02-17T13:21:05+08:00'
title: 'Keystone Interface'
tags: ["Martin Fowler"]
---

[Source](https://martinfowler.com/bliki/KeystoneInterface.html)

Software development teams find life can be much easier if they integrate their work as often as they can. They also find it valuable to release frequently into production. But teams don't want to expose half-developed features to their users. A useful technique to deal with this tension is to build all the back-end code, integrate, but don't build the user-interface. The feature can be integrated and tested, but the UI is held back until the end until, like a keystone, it's added to complete the feature, revealing it to the users.

軟體開發團隊發現，如果他們能夠經常整合他們的工作，生活會變得更加輕鬆。他們還發現，頻繁地將功能發布到正式環境中是有價值的。但團隊並不希望將尚未開發完成的功能暴露給使用者。為了應對這種矛盾，一種實用的技巧是先開發所有後端程式碼並進行整合，但暫不開發使用者介面。這樣一來，該功能可以被整合並進行測試，而 UI 則被延後，直到最後階段才像關鍵拼圖一樣被加入，從而完整該功能並向使用者公開。

A simple example of this technique might be to give a customer the option of a rush order. Such an order needs to be priced, depending on where the customer lives and what delivery companies operate there. The nature of the goods involved affects the picking approach used in the warehouse. Certain customers may qualify to have rush orders available to them, which may also depend on the delivery location, the time of year, and the kind of goods ordered.

這種技巧的一個簡單例子是為客戶提供「加急訂單」的選項。此類訂單需要根據客戶所在地區及當地運營的物流公司來確定價格。此外，商品的性質會影響倉庫的揀貨方式。某些客戶可能符合加急訂單的資格，這還可能取決於配送地點、季節以及訂購的商品種類。

All in all that's a fair bit of business logic, particularly since it will involve gnarly integration with various warehousing, catalog, and customer service systems. Doing this could take several weeks, while other features, need to be released every few days. But as far as the user is concerned, a rush order is just a check-box on the order form.

總體來說，這涉及相當多的業務邏輯，尤其是還需要與倉儲、商品目錄和客戶服務系統進行複雜的整合。這可能需要數週時間，而其他功能可能每隔幾天就需要發布一次。但對使用者而言，加急訂單只是訂單表單上的一個核取方塊（checkbox）。

To build this using the check-box as the keystone, the team does development work on the underlying business logic and interfaces to internal systems over the course of several production releases. The user is unaware of all this latent code. Only with the last step does the keystone check-box need to be made visible, which can be done in a relatively short time. This way all latent code can be integrated and be part of the system going into production, reducing the problems that come with a long-lived feature branch.

為了使用這種「核取方塊作為關鍵拼圖」的方法，開發團隊會先在多次正式發布中逐步完成底層業務邏輯以及內部系統的對接。這些潛在的程式碼對使用者來說是不可見的。只有到了最後一步，才需要讓這個關鍵的核取方塊可見，而這通常只需要很短的時間。透過這種方式，所有潛在的程式碼都能夠被整合並投入正式環境，從而減少長時間開發某個功能分支所帶來的問題。

![](https://martinfowler.com/bliki/images/keystone-interface/sketch.png)

The latent code does need to be tested to the same degree of confidence that it would be if it were active. This can be done providing the architecture of the system is setup so that most testing isn't done through the user interface. [Unit Tests](https://martinfowler.com/bliki/UnitTest.html) and other lower layers of the [Test Pyramid](https://martinfowler.com/bliki/TestPyramid.html) should be easy to run this way. Even [Broad Stack Tests](https://martinfowler.com/bliki/BroadStackTest.html) can be run providing there is a mechanism to make them [Subcutaneous Tests](https://martinfowler.com/bliki/SubcutaneousTest.html). In some cases there will a significant amount of behavior within the UI itself, but this can also be tested if the design allows the visible UI to be a [Humble Object](https://martinfowler.com/bliki/HumbleObject.html).

潛在的程式碼需要經過充分測試，以達到與其正式啟用時相同的信心水準。要做到這一點，系統的架構應該設計成大部分測試不依賴使用者介面（UI）來進行。這樣一來，[單元測試（Unit Tests）](https://martinfowler.com/bliki/UnitTest.html) 和測試金字塔（[Test Pyramid](https://martinfowler.com/bliki/TestPyramid.html)）的較低層測試就能輕鬆執行。即使是[廣域堆疊測試（Broad Stack Tests）](https://martinfowler.com/bliki/BroadStackTest.html)，也可以透過某種機制將其轉為[皮下測試（Subcutaneous Tests）](https://martinfowler.com/bliki/SubcutaneousTest.html)來運行。在某些情況下，UI 本身可能包含大量行為邏輯，但如果設計允許 UI 層作為[謙遜物件（Humble Object）](https://martinfowler.com/bliki/HumbleObject.html)，這些行為也能夠被適當地測試。

Not all applications are built in such a way that they can be extensively tested in a subcutaneous manner - but the effort required to do this is worthwhile even without the capability to use a keystone. Tests running through the UI are always more trouble to setup, even with the best tools to automate the process. Moving more tests to subcutaneous and lower level tests, especially unit tests, can dramatically speed up [Deployment Pipelines](https://martinfowler.com/bliki/DeploymentPipeline.html) and enable [Continuous Delivery](https://martinfowler.com/bliki/ContinuousDelivery.html).

並非所有應用程式都能夠以皮下測試的方式進行廣泛測試，但即使無法完全實現「關鍵拼圖」方法，投入這方面的努力仍然是值得的。透過 UI 進行測試往往更難設置，即使有最好的自動化工具也是如此。因此，將更多測試移至皮下測試及較低層級測試（尤其是單元測試），可以顯著加快[部署流水線（Deployment Pipelines）](https://martinfowler.com/bliki/DeploymentPipeline.html)，並促進[持續交付（Continuous Delivery）](https://martinfowler.com/bliki/ContinuousDelivery.html)。

Of course, most UIs will be more than a check-box, although often they aren't that much more work to keystone. In a web app, a complex feature will often be an independent web page, that can be built and tested in full, and the keystone is merely a link. A desktop may have several screens where the keystone is the menu-item to make them visible.

當然，大多數 UI 不只是單純的一個核取方塊（checkbox），但通常也不會比「關鍵拼圖」的應用更複雜多少。在 Web 應用程式中，某些複雜功能通常是獨立的網頁，可以完整開發與測試，而「關鍵拼圖」只是將其連結起來的超連結。在桌面應用程式中，可能涉及多個畫面，而「關鍵拼圖」則是用來顯示這些畫面的選單項目。

That said, there are cases when the UI can't be packaged into a simple keystone. When that's the case then it's time to use [Feature Flags](https://martinfowler.com/bliki/FeatureFlag.html). Even in this case, however, thinking of a keystone can be useful by ensuring that the feature toggle only applies to the UI. This avoids scattering lots of toggle points through the back end code, reduces the complexity of applying the toggle, allows the use of [simple toggle mechanisms](https://martinfowler.com/articles/feature-toggles.html#ImplementationTechniques), and makes it easier to remove when the time comes.

當 UI 無法被簡單地包裝成「關鍵拼圖」時，就需要使用[功能標記（Feature Flags）](https://martinfowler.com/bliki/FeatureFlag.html)。即便如此，採用「關鍵拼圖」的思維仍然有助於確保功能開關僅影響 UI，避免在後端程式碼中散布大量開關點，從而降低管理複雜度，簡化開關機制（[簡單的開關機制](https://martinfowler.com/articles/feature-toggles.html#ImplementationTechniques)），並讓未來的移除變得更加容易。

There is a general danger with developing a UI last, in that the back-end code may be designed in a way that doesn't work with the UI once it's built, or the UI isn't given the attention it needs until late, leading to a lack of iteration and a poor user experience. For those reasons a keystone approach works best within an overall approach that encourages building a product through thin vertical slices that lead to releasing small but fully working features rapidly.

不過，開發 UI 置於最後仍然存在一個風險，即後端程式碼的設計可能無法順利支援 UI，或 UI 的開發被延後，導致缺乏必要的迭代與優化，最終影響使用者體驗。因此，「關鍵拼圖」方法最適合應用於整體開發策略中，也就是透過「垂直切片（thin vertical slices）」逐步構建產品，使每次發布的小功能都是完整可用的。

I've used the example of a user-interface here, but of course the same approach can be used with any other interface, such as an API. By building the consumer's interface last, and keeping it simple, we can build and integrate even large features in small chunks.

雖然本文主要以使用者介面為例，但相同的方式也適用於其他介面，例如 API。透過最後才開發使用端介面（並保持簡潔），我們可以將大型功能拆分為小部分來逐步開發與整合。

[Dark Launching](https://martinfowler.com/bliki/DarkLaunching.html) is a variation where the new feature is called once its built, but no results are shown to the user. This is done to measure the impact on the back-end systems, which is useful for some changes. Once all is good, we can add the keystone.

[暗地發布（Dark Launching）](https://martinfowler.com/bliki/DarkLaunching.html) 是「關鍵拼圖」方法的一種變體，在新功能開發完成後，系統會實際調用它，但不向使用者顯示結果。這樣可以用來測試後端系統的影響，確保系統穩定後，再加入「關鍵拼圖」，讓功能正式可見。

## Acknowledgements

I first came across the metaphor of a keystone for this technique in the second edition of Kent Beck's [Extreme Programming Explained](https://www.amazon.com/gp/product/0321278658/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0321278658&linkCode=as2&tag=martinfowlerc-20). Pete Hodgson, Brandon Duff, and Stefan Smith reminded me that I'd forgotten this.

我最早是在 Kent Beck 的[《Extreme Programming Explained》（極限編程解析）](https://www.amazon.com/gp/product/0321278658/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0321278658&linkCode=as2&tag=martinfowlerc-20)第二版中，看到「關鍵拼圖」這個比喻。Pete Hodgson、Brandon Duff 和 Stefan Smith 提醒了我，讓我回想起這個概念。

Dave Farley, Paul Hammant, and Pete Hodgson commented on drafts on this post.

Dave Farley、Paul Hammant 和 Pete Hodgson 也對本文章的草稿提供了寶貴的意見。