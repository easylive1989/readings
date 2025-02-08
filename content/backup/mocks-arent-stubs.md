---
date: '2025-02-08T09:28:05+08:00'
title: 'Mocks Arent Stubs'
tags: ["Martin Fowler"]
---

[Source](https://martinfowler.com/articles/mocksArentStubs.html)

The term 'Mock Objects' has become a popular one to describe special case objects that mimic real objects for testing. Most language environments now have frameworks that make it easy to create mock objects. What's often not realized, however, is that mock objects are but one form of special case test object, one that enables a different style of testing. In this article I'll explain how mock objects work, how they encourage testing based on behavior verification, and how the community around them uses them to develop a different style of testing.

「模擬對象」這個術語已經成為描述模擬真實對象進行測試的特殊情況對象的流行詞彙。大多數語言環境現在都有框架,使得創建模擬對象變得容易。然而,通常沒有意識到的是,模擬對象只是特殊測試對象的一種形式,這種形式使得不同風格的測試成為可能。在本文中,我將解釋模擬對象如何工作,它們如何鼓勵基於行為驗證的測試,以及圍繞它們的社群如何使用它們來發展不同風格的測試。

I first came across the term "mock object" a few years ago in the Extreme Programming (XP) community. Since then I've run into mock objects more and more. Partly this is because many of the leading developers of mock objects have been colleagues of mine at Thoughtworks at various times. Partly it's because I see them more and more in the XP-influenced testing literature.

幾年前,我在極限編程(XP)社群中第一次接觸到“模擬對象”這個術語。從那時起,我遇到模擬對象的次數越來越多。部分原因是,許多模擬對象的主要開發者在不同時期都是我在Thoughtworks的同事。部分原因是,我在受XP影響的測試文獻中看到它們的頻率越來越高。

But as often as not I see mock objects described poorly. In particular I see them often confused with stubs - a common helper to testing environments. I understand this confusion - I saw them as similar for a while too, but conversations with the mock developers have steadily allowed a little mock understanding to penetrate my tortoiseshell cranium.

但我經常看到對模擬對象的描述不夠準確。特別是,我經常看到它們與存根(stub)混淆——這是測試環境中常見的輔助工具。我理解這種混淆——我也曾一度認為它們相似,但與模擬對象開發者的對話逐漸讓我對模擬對象有了一點理解。

This difference is actually two separate differences. On the one hand there is a difference in how test results are verified: a distinction between state verification and behavior verification. On the other hand is a whole different philosophy to the way testing and design play together, which I term here as the classical and mockist styles of Test Driven Development.

這種區別實際上是兩個獨立的區別。一方面,測試結果的驗證方式不同:狀態驗證和行為驗證之間的區別。另一方面,是測試和設計如何協同工作的完全不同的哲學,我在這裡稱之為經典和模擬風格的測試驅動開發。

## Regular Tests

I'll begin by illustrating the two styles with a simple example. (The example is in Java, but the principles make sense with any object-oriented language.) We want to take an order object and fill it from a warehouse object. The order is very simple, with only one product and a quantity. The warehouse holds inventories of different products. When we ask an order to fill itself from a warehouse there are two possible responses. If there's enough product in the warehouse to fill the order, the order becomes filled and the warehouse's amount of the product is reduced by the appropriate amount. If there isn't enough product in the warehouse then the order isn't filled and nothing happens in the warehouse.

我將通過一個簡單的例子來說明這兩種風格。(例子使用Java,但這些原則適用於任何面向對象的語言。)我們想要從倉庫對象中填充訂單對象。這個訂單非常簡單,只有一個產品和數量。倉庫持有不同產品的庫存。當我們要求訂單從倉庫中填充自己時,有兩種可能的反應。如果倉庫中有足夠的產品來填充訂單,訂單就會被填充,倉庫中的產品數量會相應減少。如果倉庫中沒有足夠的產品,那麼訂單就不會被填充,倉庫中什麼也不會發生。

These two behaviors imply a couple of tests, these look like pretty conventional JUnit tests.

這兩種行為意味著幾個測試,這些測試看起來像是相當傳統的JUnit測試。

```Java
public class OrderStateTester extends TestCase {
  private static String TALISKER = "Talisker";
  private static String HIGHLAND_PARK = "Highland Park";
  private Warehouse warehouse = new WarehouseImpl();

  protected void setUp() throws Exception {
    warehouse.add(TALISKER, 50);
    warehouse.add(HIGHLAND_PARK, 25);
  }
  public void testOrderIsFilledIfEnoughInWarehouse() {
    Order order = new Order(TALISKER, 50);
    order.fill(warehouse);
    assertTrue(order.isFilled());
    assertEquals(0, warehouse.getInventory(TALISKER));
  }
  public void testOrderDoesNotRemoveIfNotEnough() {
    Order order = new Order(TALISKER, 51);
    order.fill(warehouse);
    assertFalse(order.isFilled());
    assertEquals(50, warehouse.getInventory(TALISKER));
  }
```

xUnit tests follow a typical four phase sequence: setup, exercise, verify, teardown. In this case the setup phase is done partly in the setUp method (setting up the warehouse) and partly in the test method (setting up the order). The call to order.fill is the exercise phase. This is where the object is prodded to do the thing that we want to test. The assert statements are then the verification stage, checking to see if the exercised method carried out its task correctly. In this case there's no explicit teardown phase, the garbage collector does this for us implicitly.

xUnit測試遵循典型的四個階段順序:設置、運行、驗證、拆卸。在這個例子中,設置階段部分是在setUp方法中完成的(設置倉庫),部分是在測試方法中完成的(設置訂單)。對order.fill的調用是運行階段。這是對象被推動去執行我們想要測試的事情的階段。assert語句是驗證階段,檢查運行的方法是否正確執行了其任務。在這種情況下,沒有明確的拆卸階段,垃圾收集器為我們隱式地完成了這個階段。

During setup there are two kinds of object that we are putting together. Order is the class that we are testing, but for Order.fill to work we also need an instance of Warehouse. In this situation Order is the object that we are focused on testing. Testing-oriented people like to use terms like object-under-test or system-under-test to name such a thing. Either term is an ugly mouthful to say, but as it's a widely accepted term I'll hold my nose and use it. Following Meszaros I'll use System Under Test, or rather the abbreviation SUT.

在設置過程中,我們將兩種類型的對象放在一起。Order是我們正在測試的類,但為了使Order.fill工作,我們還需要一個Warehouse實例。在這種情況下,Order是我們重點測試的對象。測試導向的人喜歡使用諸如測試對象或測試系統之類的術語來命名這種東西。這兩個術語說起來都很繞口,但由於它是被廣泛接受的術語,我會捏著鼻子使用它。遵循Meszaros的說法,我將使用System Under Test,簡稱SUT。

So for this test I need the SUT (Order) and one collaborator (warehouse). I need the warehouse for two reasons: one is to get the tested behavior to work at all (since Order.fill calls warehouse's methods) and secondly I need it for verification (since one of the results of Order.fill is a potential change to the state of the warehouse). As we explore this topic further you'll see there we'll make a lot of the distinction between SUT and collaborators. (In the earlier version of this article I referred to the SUT as the "primary object" and collaborators as "secondary objects")

所以對於這個測試,我需要SUT(Order)和一個協作者(warehouse)。我需要倉庫的原因有兩個:一是為了使測試行為能夠正常工作(因為Order.fill調用了倉庫的方法),其次是為了驗證(因為Order.fill的結果之一是倉庫狀態的潛在變化)。隨著我們進一步探討這個話題,你會看到我們會在SUT和協作者之間做很多區分。(在本文的早期版本中,我將SUT稱為“主要對象”,協作者稱為“次要對象”)

This style of testing uses state verification: which means that we determine whether the exercised method worked correctly by examining the state of the SUT and its collaborators after the method was exercised. As we'll see, mock objects enable a different approach to verification.

這種測試風格使用狀態驗證:這意味著我們通過檢查SUT及其協作者在方法運行後的狀態來確定方法是否正確運行。正如我們將看到的,模擬對象使得不同的驗證方法成為可能。

## Tests with Mock Objects

Now I'll take the same behavior and use mock objects. For this code I'm using the jMock library for defining mocks. jMock is a java mock object library. There are other mock object libraries out there, but this one is an up to date library written by the originators of the technique, so it makes a good one to start with.

現在我將使用模擬對象來測試相同的行為。對於這段代碼,我使用了jMock庫來定義模擬對象。jMock是一個Java模擬對象庫。還有其他模擬對象庫,但這個庫是由該技術的創始人編寫的最新庫,所以它是一個很好的入門選擇。

```java
public class OrderInteractionTester extends MockObjectTestCase {
  private static String TALISKER = "Talisker";

  public void testFillingRemovesInventoryIfInStock() {
    //setup - data
    Order order = new Order(TALISKER, 50);
    Mock warehouseMock = new Mock(Warehouse.class);
    
    //setup - expectations
    warehouseMock.expects(once()).method("hasInventory")
      .with(eq(TALISKER),eq(50))
      .will(returnValue(true));
    warehouseMock.expects(once()).method("remove")
      .with(eq(TALISKER), eq(50))
      .after("hasInventory");

    //exercise
    order.fill((Warehouse) warehouseMock.proxy());
    
    //verify
    warehouseMock.verify();
    assertTrue(order.isFilled());
  }

  public void testFillingDoesNotRemoveIfNotEnoughInStock() {
    Order order = new Order(TALISKER, 51);    
    Mock warehouse = mock(Warehouse.class);
      
    warehouse.expects(once()).method("hasInventory")
      .withAnyArguments()
      .will(returnValue(false));

    order.fill((Warehouse) warehouse.proxy());

    assertFalse(order.isFilled());
  }
```

Concentrate on testFillingRemovesInventoryIfInStock first, as I've taken a couple of shortcuts with the later test.

首先集中在testFillingRemovesInventoryIfInStock,因為在後面的測試中我做了一些簡化。

To begin with, the setup phase is very different. For a start it's divided into two parts: data and expectations. The data part sets up the objects we are interested in working with, in that sense it's similar to the traditional setup. The difference is in the objects that are created. The SUT is the same - an order. However the collaborator isn't a warehouse object, instead it's a mock warehouse - technically an instance of the class Mock.

首先,設置階段非常不同。首先,它被分為兩部分:數據和預期。數據部分設置了我們感興趣的對象,從這個意義上講,它類似於傳統的設置。不同之處在於創建的對象。SUT(測試系統)是一樣的——一個訂單。然而,協作者不是一個倉庫對象,而是一個模擬倉庫——技術上是一個Mock類的實例。

The second part of the setup creates expectations on the mock object.The expectations indicate which methods should be called on the mocks when the SUT is exercised.

設置的第二部分是在模擬對象上創建預期。預期指示當測試系統被運行時應該調用哪些方法。

Once all the expectations are in place I exercise the SUT. After the exercise I then do verification, which has two aspects. I run asserts against the SUT - much as before. However I also verify the mocks - checking that they were called according to their expectations.

一旦所有預期設置完畢,我就運行測試系統。在運行之後,我進行驗證,這包括兩個方面。我對測試系統進行斷言——與之前非常相似。然而,我也驗證模擬對象——檢查它們是否按照預期被調用。

The key difference here is how we verify that the order did the right thing in its interaction with the warehouse. With state verification we do this by asserts against the warehouse's state. Mocks use behavior verification, where we instead check to see if the order made the correct calls on the warehouse. We do this check by telling the mock what to expect during setup and asking the mock to verify itself during verification. Only the order is checked using asserts, and if the method doesn't change the state of the order there's no asserts at all.

這裡的關鍵區別在於我們如何驗證訂單在與倉庫的互動中是否做了正確的事情。使用狀態驗證,我們通過對倉庫狀態的斷言來完成這一點。模擬對象使用行為驗證,我們檢查訂單是否對倉庫進行了正確的調用。我們通過在設置期間告訴模擬對象預期什麼,並在驗證期間要求模擬對象自我驗證來完成這一檢查。只有訂單是通過斷言檢查的,如果該方法不改變訂單的狀態,就不需要斷言。

In the second test I do a couple of different things. Firstly I create the mock differently, using the mock method in MockObjectTestCase rather than the constructor. This is a convenience method in the jMock library that means that I don't need to explicitly call verify later on, any mock created with the convenience method is automatically verified at the end of the test. I could have done this in the first test too, but I wanted to show the verification more explicitly to show how testing with mocks works.

在第二個測試中,我做了幾個不同的操作。首先,我用不同的方式創建模擬對象,使用MockObjectTestCase中的mock方法而不是構造函數。這是jMock庫中的一個便捷方法,意味著我不需要在之後顯式調用verify,任何使用便捷方法創建的模擬對象在測試結束時會自動驗證。我也可以在第一個測試中這樣做,但我想更明確地展示驗證,以展示使用模擬對象進行測試的工作原理。

The second different thing in the second test case is that I've relaxed the constraints on the expectation by using withAnyArguments. The reason for this is that the first test checks that the number is passed to the warehouse, so the second test need not repeat that element of the test. If the logic of the order needs to be changed later, then only one test will fail, easing the effort of migrating the tests. As it turns out I could have left withAnyArguments out entirely, as that is the default.

第二個測試中的另一個不同點是,我通過使用withAnyArguments放寬了對預期的約束。這樣做的原因是第一個測試檢查了數字是否傳遞給了倉庫,所以第二個測試不需要重複這一部分。如果訂單的邏輯需要稍後更改,那麼只有一個測試會失敗,從而減少遷移測試的工作量。事實上,我可以完全省略withAnyArguments,因為這是默認設置。

### Using EasyMock

There are a number of mock object libraries out there. One that I come across a fair bit is EasyMock, both in its java and .NET versions. EasyMock also enable behavior verification, but has a couple of differences in style with jMock which are worth discussing. Here are the familiar tests again:

有許多模擬對象庫。EasyMock 是我經常遇到的一個庫,它有 Java 和 .NET 版本。EasyMock 也支持行為驗證,但在風格上與 jMock 有一些值得討論的差異。以下是熟悉的測試:

```java
public class OrderEasyTester extends TestCase {
  private static String TALISKER = "Talisker";
  
  private MockControl warehouseControl;
  private Warehouse warehouseMock;
  
  public void setUp() {
    warehouseControl = MockControl.createControl(Warehouse.class);
    warehouseMock = (Warehouse) warehouseControl.getMock();    
  }

  public void testFillingRemovesInventoryIfInStock() {
    //setup - data
    Order order = new Order(TALISKER, 50);
    
    //setup - expectations
    warehouseMock.hasInventory(TALISKER, 50);
    warehouseControl.setReturnValue(true);
    warehouseMock.remove(TALISKER, 50);
    warehouseControl.replay();

    //exercise
    order.fill(warehouseMock);
    
    //verify
    warehouseControl.verify();
    assertTrue(order.isFilled());
  }

  public void testFillingDoesNotRemoveIfNotEnoughInStock() {
    Order order = new Order(TALISKER, 51);    

    warehouseMock.hasInventory(TALISKER, 51);
    warehouseControl.setReturnValue(false);
    warehouseControl.replay();

    order.fill((Warehouse) warehouseMock);

    assertFalse(order.isFilled());
    warehouseControl.verify();
  }
}
```

EasyMock uses a record/replay metaphor for setting expectations. For each object you wish to mock you create a control and mock object. The mock satisfies the interface of the secondary object, the control gives you additional features. To indicate an expectation you call the method, with the arguments you expect on the mock. You follow this with a call to the control if you want a return value. Once you've finished setting expectations you call replay on the control - at which point the mock finishes the recording and is ready to respond to the primary object. Once done you call verify on the control.

EasyMock 使用錄製/重放的隱喻來設置預期。對於每個你想要模擬的對象,你創建一個控制和模擬對象。模擬對象滿足次要對象的接口,控制對象提供額外的功能。為了表示預期,你在模擬對象上調用方法,並使用預期的參數。如果你想要返回值,則調用控制對象的方法。一旦你完成了預期的設置,你在控制對象上調用replay,此時模擬對象完成錄製,準備響應主要對象。完成後,你在控制對象上調用verify進行驗證。

It seems that while people are often fazed at first sight by the record/replay metaphor, they quickly get used to it. It has an advantage over the constraints of jMock in that you are making actual method calls to the mock rather than specifying method names in strings. This means you get to use code-completion in your IDE and any refactoring of method names will automatically update the tests. The downside is that you can't have the looser constraints.

儘管人們在第一次看到錄製/重放的隱喻時通常會感到困惑,但他們很快就會習慣它。相比於 jMock 的約束,它有一個優勢,即你實際調用的是模擬對象的方法,而不是在字符串中指定方法名稱。這意味著你可以在 IDE 中使用代碼自動完成功能,並且任何方法名稱的重構都會自動更新測試。缺點是你不能擁有較鬆的約束。

The developers of jMock are working on a new version which will use other techniques to allow you use actual method calls.

jMock 的開發者正在開發一個新版本,該版本將使用其他技術來允許你使用實際的方法調用。

## The Difference Between Mocks and Stubs

When they were first introduced, many people easily confused mock objects with the common testing notion of using stubs. Since then it seems people have better understood the differences (and I hope the earlier version of this paper helped). However to fully understand the way people use mocks it is important to understand mocks and other kinds of test doubles. ("doubles"? Don't worry if this is a new term to you, wait a few paragraphs and all will be clear.)

在模擬對象首次引入時,許多人容易將模擬對象與使用存根的常見測試概念混淆。自那以來,人們似乎更好地理解了兩者的區別(我希望本文的早期版本有所幫助)。然而,要完全理解人們如何使用模擬對象,重要的是要理解模擬對象和其他類型的測試替身。("doubles"? 如果這是你第一次聽到這個術語,別擔心,讀幾段後你會明白的。)

When you're doing testing like this, you're focusing on one element of the software at a time -hence the common term unit testing. The problem is that to make a single unit work, you often need other units - hence the need for some kind of warehouse in our example.

當你進行這樣的測試時,你會一次專注於軟件的一個元素,因此有了單元測試這個常見術語。問題是,要使單個單元工作,你通常需要其他單元,這就是我們的示例中需要某種倉庫的原因。

In the two styles of testing I've shown above, the first case uses a real warehouse object and the second case uses a mock warehouse, which of course isn't a real warehouse object. Using mocks is one way to not use a real warehouse in the test, but there are other forms of unreal objects used in testing like this.

在我上面展示的兩種測試風格中,第一個案例使用的是一個真實的倉庫對象,而第二個案例使用的是一個模擬倉庫,當然它不是真實的倉庫對象。使用模擬對象是一種不在測試中使用真實倉庫的方法,但還有其他形式的虛擬對象也可以這樣使用。

The vocabulary for talking about this soon gets messy - all sorts of words are used: stub, mock, fake, dummy. For this article I'm going to follow the vocabulary of Gerard Meszaros's book. It's not what everyone uses, but I think it's a good vocabulary and since it's my essay I get to pick which words to use.

談論這些的詞彙很快就會變得混亂——各種詞彙被使用:存根(stub)、模擬(mock)、假對象(fake)、假人(dummy)。在這篇文章中,我將遵循 Gerard Meszaros 書中的詞彙。這不是每個人都使用的詞彙,但我認為這是一個好的詞彙,既然這是我的文章,我可以選擇使用哪些詞。

Meszaros uses the term Test Double as the generic term for any kind of pretend object used in place of a real object for testing purposes. The name comes from the notion of a Stunt Double in movies. (One of his aims was to avoid using any name that was already widely used.) Meszaros then defined five particular kinds of double:

Meszaros 使用測試替身(Test Double)作為任何用於測試目的而代替真實對象的虛擬對象的統稱。這個名稱來自電影中的替身演員概念。(他的目標之一是避免使用已經廣泛使用的名稱。)Meszaros 定義了五種特定類型的替身:

- Dummy objects are passed around but never actually used. Usually they are just used to fill parameter lists.
- Fake objects actually have working implementations, but usually take some shortcut which makes them not suitable for production (an in memory database is a good example).
- Stubs provide canned answers to calls made during the test, usually not responding at all to anything outside what's programmed in for the test.
- Spies are stubs that also record some information based on how they were called. One form of this might be an email service that records how many messages it was sent.
- Mocks are what we are talking about here: objects pre-programmed with expectations which form a specification of the calls they are expected to receive.

- 假對象(Dummy objects):被傳遞但從未實際使用。通常它們只是用來填充參數列表。
- 假對象(Fake objects):實際有工作實現,但通常採取某種捷徑,使其不適合生產環境(內存數據庫就是一個很好的例子)。
- 存根(Stubs):為測試期間的調用提供預定答案,通常對測試編程之外的任何內容不做回應。
- 測試間諜(Spies):也是存根,但還記錄一些基於調用方式的信息。這種形式的一個例子可能是一個記錄發送了多少消息的電子郵件服務。
- 模擬(Mocks):我們在這裡討論的對象:預先編程了預期,形成它們預期接收的調用規範的對象。

Of these kinds of doubles, only mocks insist upon behavior verification. The other doubles can, and usually do, use state verification. Mocks actually do behave like other doubles during the exercise phase, as they need to make the SUT believe it's talking with its real collaborators - but mocks differ in the setup and the verification phases.

在這些替身中,只有模擬堅持行為驗證。其他替身可以,並且通常使用狀態驗證。模擬在運行階段實際上像其他替身一樣行事,因為它們需要讓測試對象相信它正在與其真實的協作者對話——但模擬在設置和驗證階段有所不同。

To explore test doubles a bit more, we need to extend our example. Many people only use a test double if the real object is awkward to work with. A more common case for a test double would be if we said that we wanted to send an email message if we failed to fill an order. The problem is that we don't want to send actual email messages out to customers during testing. So instead we create a test double of our email system, one that we can control and manipulate.

要進一步探討測試替身,我們需要擴展我們的示例。許多人只有在真實對象難以操作時才使用測試替身。一個更常見的測試替身情況是,我們說如果未能填充訂單,我們想要發送一封電子郵件。問題是我們不想在測試期間向客戶發送真實的電子郵件。因此,我們創建了一個電子郵件系統的測試替身,我們可以控制和操作它。

Here we can begin to see the difference between mocks and stubs. If we were writing a test for this mailing behavior, we might write a simple stub like this.

在這裡我們可以開始看到模擬和存根之間的區別。如果我們為這個郵件行為編寫測試,我們可能會編寫一個簡單的存根,如下所示。

```java
public interface MailService {
  public void send (Message msg);
}

public class MailServiceStub implements MailService {
  private List<Message> messages = new ArrayList<Message>();
  public void send (Message msg) {
    messages.add(msg);
  }
  public int numberSent() {
    return messages.size();
  }
}     
```

We can then use state verification on the stub like this.

我們可以這樣對存根進行狀態驗證。

```java
class OrderStateTester...

  public void testOrderSendsMailIfUnfilled() {
    Order order = new Order(TALISKER, 51);
    MailServiceStub mailer = new MailServiceStub();
    order.setMailer(mailer);
    order.fill(warehouse);
    assertEquals(1, mailer.numberSent());
  }
```

Of course this is a very simple test - only that a message has been sent. We've not tested it was sent to the right person, or with the right contents, but it will do to illustrate the point.

當然這是一個非常簡單的測試——只測試了一封郵件是否被發送。我們沒有測試它是否發送給了正確的人,或包含正確的內容,但這已經足夠說明問題。

Using mocks this test would look quite different.

使用模擬對象這個測試會看起來非常不同。

```java
class OrderInteractionTester...

  public void testOrderSendsMailIfUnfilled() {
    Order order = new Order(TALISKER, 51);
    Mock warehouse = mock(Warehouse.class);
    Mock mailer = mock(MailService.class);
    order.setMailer((MailService) mailer.proxy());

    mailer.expects(once()).method("send");
    warehouse.expects(once()).method("hasInventory")
      .withAnyArguments()
      .will(returnValue(false));

    order.fill((Warehouse) warehouse.proxy());
  }
}
```

In both cases I'm using a test double instead of the real mail service. There is a difference in that the stub uses state verification while the mock uses behavior verification.

在這兩種情況下,我都使用了測試替身而不是實際的郵件服務。不同之處在於存根使用狀態驗證,而模擬使用行為驗證。

In order to use state verification on the stub, I need to make some extra methods on the stub to help with verification. As a result the stub implements MailService but adds extra test methods.

為了在存根上使用狀態驗證,我需要在存根上添加一些額外的方法來幫助驗證。結果是,存根實現了MailService,但增加了額外的測試方法。

Mock objects always use behavior verification, a stub can go either way. Meszaros refers to stubs that use behavior verification as a Test Spy. The difference is in how exactly the double runs and verifies and I'll leave that for you to explore on your own.

模擬對象總是使用行為驗證,存根可以兩種方式都使用。Meszaros稱使用行為驗證的存根為測試間諜(Test Spy)。區別在於替身如何運行和驗證,我會讓你自行探索。

## Classical and Mockist Testing

Now I'm at the point where I can explore the second dichotomy: that between classical and mockist TDD. The big issue here is _when_ to use a mock (or other double).

現在我可以探討第二個二分法:經典TDD和模擬TDD之間的區別。這裡的主要問題是何時使用模擬對象(或其他替身)。

The **classical TDD** style is to use real objects if possible and a double if it's awkward to use the real thing. So a classical TDDer would use a real warehouse and a double for the mail service. The kind of double doesn't really matter that much.

經典TDD風格是在可能的情況下使用真實對象,只有在使用真實對象不方便時才使用替身。因此,一個經典的TDD開發者會使用一個真實的倉庫和一個郵件服務的替身。替身的種類並不那麼重要。

A **mockist TDD** practitioner, however, will always use a mock for any object with interesting behavior. In this case for both the warehouse and the mail service.

然而,模擬TDD從業者總是會為任何具有有趣行為的對象使用模擬。在這個案例中,他們會為倉庫和郵件服務都使用模擬。

Although the various mock frameworks were designed with mockist testing in mind, many classicists find them useful for creating doubles.

雖然各種模擬框架是以模擬測試為設計理念,但許多經典主義者也發現它們在創建替身時非常有用。

An important offshoot of the mockist style is that of [Behavior Driven Development](http://dannorth.net/introducing-bdd/) (BDD). BDD was originally developed by my colleague Daniel Terhorst-North as a technique to better help people learn Test Driven Development by focusing on how TDD operates as a design technique. This led to renaming tests as behaviors to better explore where TDD helps with thinking about what an object needs to do. BDD takes a mockist approach, but it expands on this, both with its naming styles, and with its desire to integrate analysis within its technique. I won't go into this more here, as the only relevance to this article is that BDD is another variation on TDD that tends to use mockist testing. I'll leave it to you to follow the link for more information.

模擬風格的一個重要分支是行為驅動開發(BDD)。BDD最初由我的同事Daniel Terhorst-North開發,作為一種技術來更好地幫助人們學習測試驅動開發,重點是TDD如何作為設計技術運行。這導致將測試重命名為行為,以更好地探索TDD在思考對象需要做什麼方面的幫助。BDD採用模擬風格,但在此基礎上擴展了它,不僅包括其命名風格,還包括其將分析整合到技術中的願望。我不會在這裡深入討論這一點,因為這篇文章的唯一相關性是BDD是另一種變體的TDD,它傾向於使用模擬測試。更多信息可以通過鏈接了解。

You sometimes see "Detroit" style used for "classical" and "London" for "mockist". This alludes to the fact that XP was originally developed with the C3 project in Detroit and the mockist style was developed by early XP adopters in London. I should also mention that many mockist TDDers dislike that term, and indeed any terminology that implies a different style between classical and mockist testing. They don't consider that there is a useful distinction to be made between the two styles.

你有時會看到“底特律”風格用於“經典”,而“倫敦”風格用於“模擬”。這暗示了XP最初在底特律的C3項目中開發,而模擬風格是由早期的XP採用者在倫敦開發的。我還應該提到,許多模擬TDD開發者不喜歡這個術語,實際上也不喜歡任何暗示經典和模擬測試之間有不同風格的術語。他們不認為兩種風格之間有必要做出有意義的區分。

## Choosing Between the Differences

In this article I've explained a pair of differences: state or behavior verification / classic or mockist TDD. What are the arguments to bear in mind when making the choices between them? I'll begin with the state versus behavior verification choice.

在這篇文章中,我解釋了一對差異:狀態驗證與行為驗證 / 經典TDD與模擬TDD。當在它們之間做出選擇時,需要考慮哪些因素?我將從狀態驗證與行為驗證的選擇開始。

The first thing to consider is the context. Are we thinking about an easy collaboration, such as order and warehouse, or an awkward one, such as order and mail service?

首先要考慮的是上下文。我們是在考慮簡單的協作(例如訂單和倉庫),還是尷尬的協作(例如訂單和郵件服務)?

If it's an easy collaboration then the choice is simple. If I'm a classic TDDer I don't use a mock, stub or any kind of double. I use a real object and state verification. If I'm a mockist TDDer I use a mock and behavior verification. No decisions at all.

如果是簡單的協作,那麼選擇很簡單。如果我是經典TDD開發者,我不會使用模擬對象、存根或任何替身。我會使用真實對象和狀態驗證。如果我是模擬TDD開發者,我會使用模擬對象和行為驗證。沒有任何決策。

If it's an awkward collaboration, then there's no decision if I'm a mockist - I just use mocks and behavior verification. If I'm a classicist then I do have a choice, but it's not a big deal which one to use. Usually classicists will decide on a case by case basis, using the easiest route for each situation.

如果是尷尬的協作,對於模擬主義者來說沒有選擇——我只會使用模擬對象和行為驗證。如果我是經典主義者,那我有選擇,但使用哪一種並不是大問題。通常經典主義者會根據具體情況做出決定,使用每種情況最簡單的路徑。

So as we see, state versus behavior verification is mostly not a big decision. The real issue is between classic and mockist TDD. As it turns out the characteristics of state and behavior verification do affect that discussion, and that's where I'll focus most of my energy.

所以我們看到,狀態驗證與行為驗證大多數情況下不是一個大決定。真正的問題在於經典TDD和模擬TDD之間的區別。事實上,狀態驗證和行為驗證的特性確實影響了這個討論,這也是我將大部分精力集中在這裡的原因。

But before I do, let me throw in an edge case. Occasionally you do run into things that are really hard to use state verification on, even if they aren't awkward collaborations. A great example of this is a cache. The whole point of a cache is that you can't tell from its state whether the cache hit or missed - this is a case where behavior verification would be the wise choice for even a hard core classical TDDer. I'm sure there are other exceptions in both directions.

但在我深入探討之前,讓我提一下邊緣情況。有時候你確實會遇到一些即使不是尷尬的協作,也很難使用狀態驗證的情況。緩存是一個很好的例子。緩存的全部意義在於你不能從其狀態判斷緩存命中還是未命中——這是一個即使對於堅定的經典TDD者來說,也應該選擇行為驗證的情況。我相信還有其他雙向的例外情況。

As we delve into the classic/mockist choice, there's lots of factors to consider, so I've broken them out into rough groups.

在我們深入探討經典和模擬TDD的選擇時,有很多因素需要考慮,所以我將它們分為粗略的組合。

### Driving TDD

Mock objects came out of the XP community, and one of the principal features of XP is its emphasis on Test Driven Development - where a system design is evolved through iteration driven by writing tests.

模擬對象來自XP社群,而XP的一個主要特點是強調測試驅動開發(TDD)——系統設計通過編寫測試來驅動迭代發展。

Thus it's no surprise that the mockists particularly talk about the effect of mockist testing on a design. In particular they advocate a style called need-driven development. With this style you begin developing a user story by writing your first test for the outside of your system, making some interface object your SUT. By thinking through the expectations upon the collaborators, you explore the interaction between the SUT and its neighbors - effectively designing the outbound interface of the SUT.

因此,不足為奇的是,模擬主義者特別強調模擬測試對設計的影響。特別是他們提倡一種稱為需求驅動開發的風格。使用這種風格,你通過編寫系統外部的第一個測試來開始開發用戶故事,將一些接口對象作為你的SUT。通過考慮對協作者的期望,你探索SUT與其鄰居之間的交互——有效地設計SUT的外部接口。

Once you have your first test running, the expectations on the mocks provide a specification for the next step and a starting point for the tests. You turn each expectation into a test on a collaborator and repeat the process working your way into the system one SUT at a time. This style is also referred to as outside-in, which is a very descriptive name for it. It works well with layered systems. You first start by programming the UI using mock layers underneath. Then you write tests for the lower layer, gradually stepping through the system one layer at a time. This is a very structured and controlled approach, one that many people believe is helpful to guide newcomers to OO and TDD.

一旦你的第一個測試運行起來,對模擬對象的期望為下一步提供了規範和測試的起點。你將每個期望轉化為對協作者的測試,並逐步將過程深入系統,一次處理一個SUT。這種風格也被稱為外部到內部,這是一個非常貼切的名稱。它在分層系統中運行良好。你首先開始編寫UI,使用下面的模擬層。然後你為較低的層編寫測試,逐層進行編寫。這是一種非常結構化和受控的方法,許多人認為它對指導新手學習面向對象和TDD很有幫助。

Classic TDD doesn't provide quite the same guidance. You can do a similar stepping approach, using stubbed methods instead of mocks. To do this, whenever you need something from a collaborator you just hard-code exactly the response the test requires to make the SUT work. Then once you're green with that you replace the hard coded response with a proper code.

經典TDD不提供完全相同的指導。你可以使用存根方法而不是模擬,進行類似的逐步方法。為此,無論何時你需要協作者的東西,你只需硬編碼測試所需的響應來使SUT工作。然後,一旦你達到綠色狀態,你就用適當的代碼替換硬編碼的響應。

But classic TDD can do other things too. A common style is middle-out. In this style you take a feature and decide what you need in the domain for this feature to work. You get the domain objects to do what you need and once they are working you layer the UI on top. Doing this you might never need to fake anything. A lot of people like this because it focuses attention on the domain model first, which helps keep domain logic from leaking into the UI.

但是經典TDD還可以做其他事情。一種常見的風格是由內而外。在這種風格中,你選擇一個功能,並決定這個功能在領域中需要什麼。你讓領域對象做你需要的事情,一旦它們工作起來,你將UI層疊加在上面。這樣做你可能永遠不需要模擬任何東西。許多人喜歡這樣,因為它首先關注領域模型,有助於防止領域邏輯泄漏到UI中。

I should stress that both mockists and classicists do this one story at a time. There is a school of thought that builds applications layer by layer, not starting one layer until another is complete. Both classicists and mockists tend to have an agile background and prefer fine-grained iterations. As a result they work feature by feature rather than layer by layer.

我應該強調,無論是模擬主義者還是經典主義者都是一次處理一個故事。有一種思想流派是分層構建應用程序,不開始新的一層,直到上一層完成。經典主義者和模擬主義者都傾向於有敏捷背景,更喜歡細粒度的迭代。因此,他們按功能逐步工作,而不是按層逐步工作。

### Fixture Setup

With classic TDD, you have to create not just the SUT but also all the collaborators that the SUT needs in response to the test. While the example only had a couple of objects, real tests often involve a large amount of secondary objects. Usually these objects are created and torn down with each run of the tests.

使用經典TDD,你不僅需要創建SUT,還需要創建SUT根據測試所需的所有協作者。雖然示例中只有幾個對象,但實際測試通常涉及大量的次要對象。通常這些對象在每次測試運行時都會被創建和拆除。

Mockist tests, however, only need to create the SUT and mocks for its immediate neighbors. This can avoid some of the involved work in building up complex fixtures (At least in theory. I've come across tales of pretty complex mock setups, but that may be due to not using the tools well.)

然而,模擬測試只需要創建SUT和其直接鄰居的模擬對象。這可以避免一些構建復雜固定設置的工作(至少理論上是這樣。我遇到過一些相當復雜的模擬設置,但這可能是因為沒有很好地使用工具)。

In practice, classic testers tend to reuse complex fixtures as much as possible. In the simplest way you do this by putting fixture setup code into the xUnit setup method. More complicated fixtures need to be used by several test classes, so in this case you create special fixture generation classes. I usually call these Object Mothers, based on a naming convention used on an early Thoughtworks XP project. Using mothers is essential in larger classic testing, but the mothers are additional code that need to be maintained and any changes to the mothers can have significant ripple effects through the tests. There also may be a performance cost in setting up the fixture - although I haven't heard this to be a serious problem when done properly. Most fixture objects are cheap to create, those that aren't are usually doubled.

實際上,經典測試者傾向於盡可能多地重用復雜的固定設置。最簡單的方法是將固定設置代碼放入xUnit的設置方法中。更複雜的固定設置需要被多個測試類使用,所以在這種情況下,你創建特殊的固定生成類。我通常稱這些為Object Mothers,這基於早期Thoughtworks XP項目中的命名約定。在較大的經典測試中使用mothers是必不可少的,但mothers是需要維護的額外代碼,對mothers的任何更改都可能對測試產生顯著的連鎖反應。在設置固定時也可能有性能成本——雖然如果正確操作,我還沒有聽說過這是一個嚴重的問題。大多數固定對象創建起來都很便宜,那些不便宜的通常會被替代。

As a result I've heard both styles accuse the other of being too much work. Mockists say that creating the fixtures is a lot of effort, but classicists say that this is reused but you have to create mocks with every test.

結果,我聽到兩種風格互相指責對方工作量太大。模擬主義者說創建固定設置需要很多努力,但經典主義者說這是重用的,但你必須在每個測試中創建模擬。

### Test Isolation

If you introduce a bug to a system with mockist testing, it will usually cause only tests whose SUT contains the bug to fail. With the classic approach, however, any tests of client objects can also fail, which leads to failures where the buggy object is used as a collaborator in another object's test. As a result a failure in a highly used object causes a ripple of failing tests all across the system.

如果你在模擬測試的系統中引入了一個錯誤,它通常只會導致包含該錯誤的SUT的測試失敗。然而,在經典方法中,任何客戶端對象的測試也可能失敗,這會導致在另一個對象的測試中使用有錯誤的對象時失敗。因此,高度使用的對象中的錯誤會在整個系統中引起一連串的測試失敗。

Mockist testers consider this to be a major issue; it results in a lot of debugging in order to find the root of the error and fix it. However classicists don't express this as a source of problems. Usually the culprit is relatively easy to spot by looking at which tests fail and the developers can tell that other failures are derived from the root fault. Furthermore if you are testing regularly (as you should) then you know the breakage was caused by what you last edited, so it's not difficult to find the fault.

模擬測試者認為這是個主要問題;這會導致大量調試以找到錯誤的根源並修復它。然而,經典主義者並不認為這是問題的根源。通常,罪魁禍首通過查看哪些測試失敗就很容易找到,開發人員可以告訴其他失敗是由根本錯誤引起的。此外,如果你定期測試(如你應該的那樣),那麼你知道破壞是由你最後編輯的東西引起的,因此找到錯誤並不難。

One factor that may be significant here is the granularity of the tests. Since classic tests exercise multiple real objects, you often find a single test as the primary test for a cluster of objects, rather than just one. If that cluster spans many objects, then it can be much harder to find the real source of a bug. What's happening here is that the tests are too coarse grained.

這裡可能重要的一個因素是測試的粒度。由於經典測試運行多個真實對象,你通常會發現一個測試是多個對象集群的主要測試,而不僅僅是一個。如果該集群跨越多個對象,那麼找到錯誤的真正來源可能會更加困難。這裡發生的事情是測試太粗粒度了。

It's quite likely that mockist tests are less likely to suffer from this problem, because the convention is to mock out all objects beyond the primary, which makes it clear that finer grained tests are needed for collaborators. That said, it's also true that using overly coarse grained tests isn't necessarily a failure of classic testing as a technique, rather a failure to do classic testing properly. A good rule of thumb is to ensure that you separate fine-grained tests for every class. While clusters are sometimes reasonable, they should be limited to only very few objects - no more than half a dozen. In addition, if you find yourself with a debugging problem due to overly coarse-grained tests, you should debug in a test driven way, creating finer grained tests as you go.

模擬測試很可能不太容易遇到這個問題,因為常規是將主要對象以外的所有對象模擬掉,這使得很明顯需要對協作者進行更細粒度的測試。話雖如此,也有可能使用過於粗粒度的測試並不是經典測試技術的失敗,而是未能正確地進行經典測試。一個好的經驗法則是確保你為每個類分開細粒度的測試。雖然集群有時是合理的,但應限制在非常少的對象——不超過六個。此外,如果你發現自己因過於粗粒度的測試而遇到調試問題,你應該以測試驅動的方式進行調試,創建更細粒度的測試。

In essence classic xunit tests are not just unit tests, but also mini-integration tests. As a result many people like the fact that client tests may catch errors that the main tests for an object may have missed, particularly probing areas where classes interact. Mockist tests lose that quality. In addition you also run the risk that expectations on mockist tests can be incorrect, resulting in unit tests that run green but mask inherent errors.

本質上,經典的xunit測試不僅僅是單元測試,還是迷你集成測試。因此,許多人喜歡這樣的事實,即客戶端測試可能會發現對象主要測試中可能錯過的錯誤,特別是探測類之間交互的區域。模擬測試失去了這種質量。此外,你還面臨著模擬測試中的期望可能不正確的風險,導致單元測試運行綠色,但掩蓋了固有的錯誤。

It's at this point that I should stress that whichever style of test you use, you must combine it with coarser grained acceptance tests that operate across the system as a whole. I've often come across projects which were late in using acceptance tests and regretted it.

在這一點上,我應該強調,無論你使用哪種測試風格,你必須將其與跨系統運行的粗粒度驗收測試結合使用。我經常遇到一些項目,它們在太晚使用驗收測試,並為此感到遺憾。

### Coupling Tests to Implementations

When you write a mockist test, you are testing the outbound calls of the SUT to ensure it talks properly to its suppliers. A classic test only cares about the final state - not how that state was derived. Mockist tests are thus more coupled to the implementation of a method. Changing the nature of calls to collaborators usually cause a mockist test to break.

當你編寫模擬測試時,你正在測試SUT的 the outbound calls,以確保它正確地與其供應商通信。經典測試只關心最終狀態——不關心該狀態是如何導出的。模擬測試因此更依賴於方法的實現。改變對協作者的調用性質通常會導致模擬測試失敗。

This coupling leads to a couple of concerns. The most important one is the effect on Test Driven Development. With mockist testing, writing the test makes you think about the implementation of the behavior - indeed mockist testers see this as an advantage. Classicists, however, think that it's important to only think about what happens from the external interface and to leave all consideration of implementation until after you're done writing the test.

這種耦合導致了幾個問題。最重要的是對測試驅動開發的影響。使用模擬測試,編寫測試使你思考行為的實現——事實上,模擬測試者認為這是一個優勢。而經典主義者則認為,重要的是只考慮外部接口發生了什麼,並且在編寫測試後才考慮實現。

Coupling to the implementation also interferes with refactoring, since implementation changes are much more likely to break tests than with classic testing.

實現的耦合也干擾了重構,因為實現更改比經典測試更容易破壞測試。

This can be worsened by the nature of mock toolkits. Often mock tools specify very specific method calls and parameter matches, even when they aren't relevant to this particular test. One of the aims of the jMock toolkit is to be more flexible in its specification of the expectations to allow expectations to be looser in areas where it doesn't matter, at the cost of using strings that can make refactoring more tricky.

這可能因模擬工具包的性質而變得更糟。通常模擬工具指定非常具體的方法調用和參數匹配,即使它們與此特定測試無關。jMock工具包的一個目標是使期望的規範更加靈活,以允許在不重要的區域放寬期望,代價是使用字符串可能使重構變得更加棘手。

### Design Style

One of the most fascinating aspects of these testing styles to me is how they affect design decisions. As I've talked with both types of tester I've become aware of a few differences between the designs that the styles encourage, but I'm sure I'm barely scratching the surface.

對我來說,這些測試風格最有趣的方面之一是它們如何影響設計決策。當我與這兩種類型的測試者交談時,我注意到一些設計差異,但我相信我只觸及了表面。

I've already mentioned a difference in tackling layers. Mockist testing supports an outside-in approach while developers who prefer a domain model out style tend to prefer classic testing.

我已經提到過處理層的差異。模擬測試支持從外到內的方法,而偏好領域模型的方法則傾向於經典測試。

On a smaller level I noticed that mockist testers tend to ease away from methods that return values, in favor of methods that act upon a collecting object. Take the example of the behavior of gathering information from a group of objects to create a report string. A common way to do this is to have the reporting method call string returning methods on the various objects and assemble the resulting string in a temporary variable. A mockist tester would be more likely to pass a string buffer into the various objects and get them to add the various strings to the buffer - treating the string buffer as a collecting parameter.

在較小的層面上,我注意到模擬測試者傾向於避免返回值的方法,而傾向於操作收集對象的方法。以從一組對象中收集信息以創建報告字符串的行為為例。一種常見的方法是讓報告方法調用各個對象的字符串返回方法,並在臨時變量中組裝結果字符串。模擬測試者更有可能將字符串緩衝區傳遞給各個對象,讓它們將各種字符串添加到緩衝區中——將字符串緩衝區視為收集參數。

Mockist testers do talk more about avoiding 'train wrecks' - method chains of style of getThis().getThat().getTheOther(). Avoiding method chains is also known as following the Law of Demeter. While method chains are a smell, the opposite problem of middle men objects bloated with forwarding methods is also a smell. (I've always felt I'd be more comfortable with the Law of Demeter if it were called the Suggestion of Demeter.)

模擬測試者更多地談論避免“火車殘骸”——方法鏈式的getThis().getThat().getTheOther()。避免方法鏈也被稱為遵循Demeter法則。雖然方法鏈是一個氣味問題,但中間對象充滿轉發方法的相反問題也是一個氣味問題。(我一直覺得如果Demeter法則被稱為Demeter建議,我會更舒服。)

One of the hardest things for people to understand in OO design is the "Tell Don't Ask" principle, which encourages you to tell an object to do something rather than rip data out of an object to do it in client code. Mockists say that using mockist testing helps promote this and avoid the getter confetti that pervades too much of code these days. Classicists argue that there are plenty of other ways to do this.

對於人們理解面向對象設計中的“告訴而不是詢問”原則來說,是最難理解的,這鼓勵你告訴一個對象做某事,而不是從對象中提取數據在客戶端代碼中完成它。模擬測試者說,使用模擬測試有助於促進這一點,並避免充斥代碼的getter方法。經典主義者則認為,有很多其他方法可以做到這一點。

An acknowledged issue with state-based verification is that it can lead to creating query methods only to support verification. It's never comfortable to add methods to the API of an object purely for testing, using behavior verification avoids that problem. The counter-argument to this is that such modifications are usually minor in practice.

狀態驗證的一個公認問題是,它可能導致創建查詢方法僅支持驗證。在對象的API中僅為測試添加方法從來都不是令人舒服的事,使用行為驗證可以避免這個問題。對此的反駁是,這種修改在實踐中通常是次要的。

Mockists favor role interfaces and assert that using this style of testing encourages more role interfaces, since each collaboration is mocked separately and is thus more likely to be turned into a role interface. So in my example above using a string buffer for generating a report, a mockist would be more likely to invent a particular role that makes sense in that domain, which may be implemented by a string buffer.

模擬測試者偏好角色接口,並斷言使用這種測試風格鼓勵更多角色接口,因為每個協作都是單獨模擬的,因此更有可能轉變為角色接口。所以在我上面使用字符串緩衝區生成報告的例子中,模擬測試者更有可能發明一個在該領域中有意義的特定角色,這可能由字符串緩衝區實現。

It's important to remember that this difference in design style is a key motivator for most mockists. TDD's origins were a desire to get strong automatic regression testing that supported evolutionary design. Along the way its practitioners discovered that writing tests first made a significant improvement to the design process. Mockists have a strong idea of what kind of design is a good design and have developed mock libraries primarily to help people develop this design style.

重要的是要記住,這種設計風格的差異是大多數模擬測試者的關鍵動機。TDD的起源是一個強大的自動回歸測試,以支持演進設計。其從業者發現,先編寫測試對設計過程有顯著改進。模擬測試者對什麼樣的設計是好的設計有強烈的看法,並且主要開發模擬庫以幫助人們開發這種設計風格。

## So should I be a classicist or a mockist?

I find this a difficult question to answer with confidence. Personally I've always been a old fashioned classic TDDer and thus far I don't see any reason to change. I don't see any compelling benefits for mockist TDD, and am concerned about the consequences of coupling tests to implementation.

我發現很難自信地回答這個問題。個人來說，我一直是個老派的經典TDD者，到目前為止，我沒有看到任何改變的理由。我看不出模擬TDD有任何令人信服的好處，並且擔心將測試與實現耦合的後果。

This has particularly struck me when I've observed a mockist programmer. I really like the fact that while writing the test you focus on the result of the behavior, not how it's done. A mockist is constantly thinking about how the SUT is going to be implemented in order to write the expectations. This feels really unnatural to me.

這一點特別在我觀察一個模擬程序員時引起了我的注意。我真的很喜歡在編寫測試時，你專注於行為的結果，而不是它是如何完成的。一個模擬主義者在編寫期望時，總是在考慮SUT將如何實現。這對我來說感覺非常不自然。

I also suffer from the disadvantage of not trying mockist TDD on anything more than toys. As I've learned from Test Driven Development itself, it's often hard to judge a technique without trying it seriously. I do know many good developers who are very happy and convinced mockists. So although I'm still a convinced classicist, I'd rather present both arguments as fairly as I can so you can make your own mind up.

我也因未在除了玩具項目之外的任何實際項目中嘗試過模擬TDD而處於劣勢。正如我從測試驅動開發本身學到的那樣，不認真嘗試某種技術通常很難評判它。我確實認識很多優秀的開發者，他們對模擬TDD非常滿意並深信不疑。所以，儘管我仍然是個堅定的經典主義者，我寧願儘可能公平地呈現雙方的論點，讓你自己做出決定。

So if mockist testing sounds appealing to you, I'd suggest giving it a try. It's particularly worth trying if you are having problems in some of the areas that mockist TDD is intended to improve. I see two main areas here. One is if you're spending a lot of time debugging when tests fail because they aren't breaking cleanly and telling you where the problem is. (You could also improve this by using classic TDD on finer-grained clusters.) The second area is if your objects don't contain enough behavior, mockist testing may encourage the development team to create more behavior rich objects.

所以，如果模擬測試對你有吸引力，我建議你嘗試一下。特別是如果你在某些模擬TDD旨在改進的領域遇到了問題，我認為值得嘗試。我看到這裡有兩個主要領域。其一是當測試失敗時，如果它們沒有清晰地告訴你問題所在，你可能會花很多時間調試。（你也可以通過在更細粒度的集群上使用經典TDD來改進這一點。）第二個領域是如果你的對象不包含足夠的行為，模擬測試可能會鼓勵開發團隊創建更多行為豐富的對象。

## Final Thoughts

As interest in unit testing, the xunit frameworks and Test Driven Development has grown, more and more people are running into mock objects. A lot of the time people learn a bit about the mock object frameworks, without fully understanding the mockist/classical divide that underpins them. Whichever side of that divide you lean on, I think it's useful to understand this difference in views. While you don't have to be a mockist to find the mock frameworks handy, it is useful to understand the thinking that guides many of the design decisions of the software.

隨著對單元測試、xUnit框架和測試驅動開發（TDD）的興趣不斷增長，越來越多的人開始接觸模擬對象。很多時候，人們會學到一些關於模擬對象框架的知識，但未能完全理解模擬主義/經典主義之間的區別。無論你傾向於哪一邊，我認為了解這種觀點的差異是有益的。雖然你不必成為模擬主義者才能發現模擬框架的便利性，但理解指導許多設計決策的思維是有用的。

The purpose of this article was, and is, to point out these differences and to lay out the trade-offs between them. There is more to mockist thinking than I've had time to go into, particularly its consequences on design style. I hope that in the next few years we'll see more written on this and that will deepen our understanding of the fascinating consequences of writing tests before the code.

本文的目的在於指出這些差異並展示它們之間的權衡取捨。模擬主義思維還有很多內容未能深入探討，特別是它對設計風格的影響。我希望在接下來的幾年裡，我們會看到更多關於這方面的討論，這將加深我們對在編寫代碼之前編寫測試這一令人著迷的做法的理解。
