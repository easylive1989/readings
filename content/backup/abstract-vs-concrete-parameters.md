---
date: '2025-02-08T16:07:52+08:00'
title: 'Abstract vs Concrete Parameters'
tags: ["Kent Beck"]
---

[Source](https://tidyfirst.substack.com/p/abstract-vs-concrete-parameters?utm_source=post-email-title&publication_id=256838&post_id=121830629&isFreemail=true&utm_medium=email)

Originally published August 2008. It’s interesting to me how many of the themes of 15 years ago are still relevant today. I was more down in the programming details then than I usually am today. Let me know if this sort of specific advice helps.

最初發表於 2008 年 8 月。令我感到有趣的是，15 年前的許多主題至今仍然具有相關性。當時我比現在更深入編程細節。如果這類具體的建議對你有幫助，請告訴我。

>Easy-to-test software is "controllable". Testers can cheaply and accurately simulate the contexts in which the software needs to run. Two contradictory patterns help achieve controllability: making parameters more concrete and more abstract. This apparent contradiction resolves when looked at from a broader perspective.

>易於測試的軟體是「可控的」。測試人員可以以低成本且準確地模擬軟體需要運行的上下文。兩種看似矛盾的模式有助於實現可控性：讓參數更具體和更抽象。當從更廣泛的角度來看，這種表面上的矛盾可以得到解釋。

# Introduction

Designing software is hard. Designs need to:

設計軟體是一件困難的事。設計需要：

- Be understandable to those who will implement them
- Support currently required functionality
- Support future changes, anticipated and unanticipated.

- 對於將要實現它的人來說是易於理解的
- 支援當前所需的功能
- 支援未來的變更，不論是預期的還是非預期的

The two economic imperatives of software development—rapid initial time to market and low lifecycle costs—often call for different designs. Designers need to find some resolution of these technical and economic conflicts.

軟體開發的兩個經濟驅動因素——快速的初期上市時間和低生命週期成本——往往需要不同的設計方法。設計者需要在這些技術和經濟衝突中找到某種解決方式。

As if this wasn't difficult enough, Extreme Programming comes along and calls for software to be testable, automatically testable, as well. For software to be testable, it must be controllable and observable. Observability is the ability to measure the effects of a computation. I won't talk about more in this note. Controllability is the ability to reproduce all the contexts in which an object is to run, normal and abnormal. The new job of design is to make sure that software is controllable cheaply in both developer time and execution time.

如果這還不夠困難，極限編程（Extreme Programming）還要求軟體必須是可測試的，並且是自動化測試的。為了使軟體可測試，它必須是可控的和可觀測的。**可觀測性**指的是能夠測量計算結果的能力，這裡不作進一步討論。**可控性**則是指能夠重現一個物件執行的所有上下文，包括正常和異常的情況。新的設計工作在於確保軟體在開發者時間和執行時間的成本都能夠以低成本實現可控性。

I wrote tests and programs together for years without explicitly worrying about controllability. One of the strengths of test-driven development is that using it provides immediate feedback about whether or not a proposed API can exercise an object and how difficult the API is to use. In more complicated situations, though, or when dealing with legacy code, I've found thinking explicitly about controllability to be helpful.

多年的測試驅動開發實踐中，我一邊編寫測試和程式，一邊沒有明確擔憂可控性。測試驅動開發的一大優勢在於它能提供即時的反饋，幫助判斷所設計的 API 是否能輕鬆操作物件，以及使用該 API 的難易程度。然而，在更複雜的情況下，或者處理遺留程式碼時，我發現明確考慮可控性是有幫助的。

## Concrete Parameters Enhance Controllability 更具體的參數增強可控性

An early encounter with controllability came when I was working on an insurance system. We needed to be sure that we were retrieving the correct mortality table for a customer. Our first attempt at a test/interface pair was to pass the customer as a parameter:

第一次接觸可控性時，我正在開發一個保險系統。我們需要確保能為客戶檢索到正確的死亡率表。我們的第一個測試/介面組合嘗試是將客戶作為參數傳遞：

```java
class MortalityTableTest {  
    @Test retrieveMaleNonSmoker() {  
       // ... a bunch of stuff to create a male, non-smoking customer  
       MortalityTable result= MortalityTable.lookup(customer);  
       assertEquals("QM115", result.getName());  
    }  
}
```

This worked, but it was expensive, both writing the code to create a customer and the execution time to create the megabyte of objects necessary to create a well-formed customer. This left the test slow and vulnerable to changes in the way we set up customers.

這樣可以工作，但代價很高，因為要撰寫程式碼建立客戶，以及執行期間需要建立大量的物件（可能達到數百萬字節）來生成一個良好格式的客戶。這導致測試速度緩慢，且容易受我們設置客戶方式的變更影響。

Looking at the MortalityTable.lookup() code we could see that the only parts of a Customer we used were the gender and smoker status: two one-bit enums out of a megabyte. Rather than wait and let the table lookup extract this information, we could shift that bit of code to the caller [ed: more on this kind of call tree manipulation later]:

檢視 `MortalityTable.lookup()` 的程式碼後，我們發現實際用到的客戶資訊只有性別和吸菸狀態：也就是只有兩個一位元的枚舉值，而不是整個物件。因此，我們可以將這部分程式碼移到呼叫者來完成（稍後會更多討論這種呼叫樹的操作）：

```java
class MortalityTableTest {  
    @Test retrieveMaleNonSmoker() {  
       // ... a bunch of stuff to create a customer  
       MortalityTable result=  MortalityTable.lookup(customer.getGender(), customer.getSmoker());
         
       assertEquals("QM115", result.getName());  
    }  
}
```

The second step was to inline all the customer creation and gender/smoker retrieval, since we knew what the answers would be:

第二步是內嵌所有的客戶建立和性別/吸菸狀態檢索，因為我們已經知道答案是什麼：

```java
class MortalityTableTest {  
    @Test retrieveMaleNonSmoker() {  
       MortalityTable result=  MortalityTable.lookup(Gender.MALE, Smoker.NO);  
       assertEquals("QM115", result.getName());  
    }  
}
```

The resulting test is easier to read and runs fast. Changes to the Customer won't affect the test. However, the test is vulnerable to changes in the lookup process. In the first version, if the lookup began also checking for marital status we would only have to change the test to set the appropriate status. In the third version, we would not only have to change the test, we would also have to change the API of MortalityTable. As long as mortality table lookup remains stable, though, we improved the controllability of our system by passing more-concrete parameters.

最後生成的測試更容易閱讀，且執行速度更快。客戶相關的變更不會影響測試。然而，這個測試容易受到查找過程變更的影響。在第一個版本中，如果查找過程開始檢查婚姻狀況，我們只需要更改測試來設置合適的狀態。在第三個版本中，我們不僅需要更改測試，還需要更改 `MortalityTable` 的 API。然而，只要死亡率表查找過程保持穩定，我們就透過傳遞更具體的參數來提升了系統的可控性。

## Abstract Parameters Enhance Controllability 更抽象的參數增強可控性

Recently, I had an experience that seemed to offer the opposite conclusion. A questioner to the JUnit mailing list asked about testing a legacy object that needed to communicate over a socket. The existing API took an IP address and port number. How could they write tests?

最近，我有一次經驗似乎得出了相反的結論。一位 JUnit 郵件列表上的提問者詢問如何測試需要通過套接字通訊的遺留物件。現有的 API 接受 IP 地址和埠號作為參數。他們該如何撰寫測試？

A black box strategy is to create a test fixture that can open up server sockets simulating the various test conditions. This has the advantage that the object-under-test need not be modified. However, it would be a fairly complex fixture to write, making sure to completely clean up the test bed, correctly handle timeouts and avoid race conditions.

一種黑箱策略是建立一個測試固定裝置，該裝置可以打開伺服器套接字以模擬各種測試條件。這樣的好處是被測物件不需要被修改。然而，這需要撰寫一個相當複雜的固定裝置，確保測試環境能完全清理、正確處理超時並避免競態條件。

An alternative is to objectify the IP address and port number into a SocketConnection. Rather than pass raw numbers into the constructor, pass a SocketConnection (an existing implementation, if possible). The current constructor can be grandfathered by having it create a connection:

另一種方法是將 IP 地址和埠號抽象為一個 `SocketConnection`。而不是將原始數字傳入構造函數，改為傳遞一個 `SocketConnection`（如果可能的話，使用現有的實現）。現有的構造函數可以透過讓其建立一個連接來繼續使用：

```java
class Client {  
    Client(int address, int port) {  
       this(new SocketConnection(address, port));  
    }  
}
```

Tests can then create impostor connections to simulate all the test scenarios. This solution will likely run faster and be easier to write tests for, but at the cost of modifying the Client. The big point, from the perspective of this paper, is that controllability in this case was achieved by passing a more abstract parameter.

這樣，測試可以創建模擬連接來模擬所有測試場景。這種解決方案可能執行速度更快，且更容易撰寫測試，但代價是需要修改 `Client`。從本文的角度來看，關鍵點是，在這種情況下，通過傳遞更抽象的參數實現了可控性。

## Ambiguous? Maybe. Contradictory? No. 模糊？也許。矛盾？並非如此。

Here we run aground. In the first case, controllability came from making the parameters more concrete, in the second, from making them more abstract. Which is it? What is the simple rule?

在這裡，我們陷入了困境。在第一個案例中，可控性來自於讓參數更具體；而在第二個案例中，可控性則來自於讓參數更抽象。到底是哪一個？有什麼簡單的規則可以遵循？

It turns out that in this situation, the rule for achieving controllability is not simple and linear. How best to design for controllability is the result of a tradeoff between the cost and the required flexibility of the tests. One leg of the tradeoff says that concrete parameters are more economical than abstract parameters:

事實證明，在這種情況下，實現可控性的規則並不簡單，也不是線性的。設計可控性的最佳方式取決於測試成本和所需靈活性之間的權衡。權衡的一方面是具體參數比抽象參數更具經濟性：

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8e4071db-34f1-4126-a8e1-4e4693643a42_320x146.png)

The second leg of the tradeoff says that the flexibility of tests are enhanced by abstract parameters:

權衡的另一面則指出，抽象參數能增強測試的靈活性：

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F98d9c192-6b28-4bd9-8e64-98e60b251409_320x121.png)

Put the two together and you have the tradeoff curve:

將這兩者結合起來，就得到了權衡曲線：

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F30fc97da-f169-426b-9182-3696bdc007f0_320x143.png)

As a designer, I find such tradeoffs to be extremely useful, especially as a way of explaining my decisions. I may get an intuition without explicitly thinking of the tradeoff, but when I want to discuss a decision I find it valuable to be able to say, "In this case, we really don't need flexibility, so concrete parameters are appropriate":

作為設計者，我發現這種權衡極其有用，尤其是在解釋我的決策時。我可能憑直覺得出結論，未必會明確地考慮到這種權衡，但當我需要討論某個決策時，我覺得能說明如下的觀點是很有價值的：「在這種情況下，我們其實並不需要太多靈活性，因此使用具體參數是合適的。」

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4278c928-27ea-4ece-b1bb-895735681669_320x146.png)

Alternatively, if my gut tells me a more-abstract parameter would be an improvement, I like being able to illustrate it, "We have all these tests and all these parameters. I think it would be simpler to bundle them together.":

或者，如果直覺告訴我更抽象的參數會是一種改進，我也喜歡能這樣說明：「我們有這麼多測試和這麼多參數。我認為將它們整合在一起會更簡單一些。」

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8980090c-7d84-4b83-ba5d-d179db6d42e3_319x146.png)

Seeing the two factors together helps me be more aware of when I have an opportunity to improve my design by sliding towards abstract or concrete parameters.

將這兩個因素結合起來，有助於讓我更清楚地意識到，什麼時候可以透過偏向抽象或具體參數來改進設計。

## Conclusion

Now I can state the general rule: to make software controllable, pass concrete parameters when tests don't need much flexibility and pass abstract parameters when they need more flexibility. Having just realized this relationship, though, I'm not sure how far it will go. If you find interesting examples (or counter-examples), I'd love to hear about them.

現在我可以提出一般規則：為了讓軟體具備可控性，當測試不需要太多靈活性時，傳遞具體參數；當測試需要更多靈活性時，傳遞抽象參數。不過，我剛剛才認識到這種關係，還不確定它的適用範圍有多廣。如果你發現有趣的例子（或反例），我很樂意聽到你的分享。