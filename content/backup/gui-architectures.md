---
date: '2025-02-08T21:37:55+08:00'
title: 'Gui Architectures'
tags: ["Martin Fowler"]
---
[Source](https://martinfowler.com/eaaDev/uiArchs.html)

_Graphical User Interfaces provide a rich interaction between the user and a software system. Such richness is complex to manage, so it's important to contain that complexity with a thoughtful architecture. The Forms and Controls pattern works well for systems with a simple flow, but as it breaks down under the weight of greater complexity, most people turn to “Model-View-Controller” (MVC). Sadly MVC is one of the most misunderstood architectural patterns around, and systems using that name display a range of important differences, sometimes described under names like Application Model, Model-View-Presenter, Presentation Model, MVVM, and the like. The best way to think of MVC is as set of principles including the separation of presentation from domain logic and synchronizing presentation state through events (the observer pattern)._

圖形用戶界面提供了使用者與軟體系統之間的豐富互動。這種豐富性很難管理，因此用周到的架構來控制這種複雜性是很重要的。表單和控制項模式對於具有簡單流程的系統效果很好，但隨著複雜性增加，這種模式會逐漸崩潰，大多數人轉向「模型-視圖-控制器」（MVC）。遺憾的是，MVC是最被誤解的架構模式之一，使用該名稱的系統顯示出一系列重要的差異，有時被描述為應用模型、模型-視圖-呈現者、展示模型、MVVM等。最好的方式是將MVC視為一組原則，包括將展示層與領域邏輯分離，並通過事件（觀察者模式）同步展示層狀態。

This is part of the [Further Enterprise Application Architecture development](https://martinfowler.com/eaaDev) writing that I was doing in the mid 2000’s. Sadly too many other things have claimed my attention since, so I haven’t had time to work on them further, nor do I see much time in the foreseeable future. As such this material is very much in draft form and I won’t be doing any corrections or updates until I’m able to find time to work on it again.

這是我在2000年代中期所撰寫的《進一步企業應用架構開發》的一部分。遺憾的是，許多其他事情佔據了我的注意力，因此我無法進一步處理，也看不到近期有時間來處理。因此，這些材料還處於草稿形式，我在有時間重新處理之前，不會進行任何修正或更新。

Graphical user interfaces have become a familiar part of our software landscape, both as users and as developers. Looking at it from a design perspective they represent a particular set of problems in system design - problems that have led to a number of different but similar solutions.

圖形用戶界面已成為我們軟體環境中熟悉的一部分，無論是作為使用者還是開發者。從設計的角度來看，它們代表了系統設計中的一組特定問題——這些問題導致了許多不同但相似的解決方案。

My interest is identifying common and useful patterns for application developers to use in rich-client development. I've seen various designs in project reviews and also various designs that have been written in a more permanent way. Inside these designs are the useful patterns, but describing them is often not easy. Take Model-View-Controller as an example. It's often referred to as a pattern, but I don't find it terribly useful to think of it as a pattern because it contains quite a few different ideas. Different people reading about MVC in different places take different ideas from it and describe these as 'MVC'. If this doesn't cause enough confusion you then get the effect of misunderstandings of MVC that develop through [Semantic Diffusion](https://martinfowler.com/bliki/SemanticDiffusion.html).

我的興趣在於為應用程式開發者辨識出常見且有用的模式，用於豐富客戶端開發。我在專案審查中看到了各種設計，也看到了一些已被永久記錄下來的設計。在這些設計中有許多有用的模式，但描述它們通常並不容易。以模型-視圖-控制器為例。它通常被稱為一種模式，但我認為將其視為一種模式並不是非常有用，因為它包含了許多不同的想法。不同的人在不同的地方閱讀MVC，會從中獲取不同的想法，並將這些想法稱為「MVC」。如果這還不足以引起混淆，那麼隨後的語義擴散所導致的對MVC的誤解就會產生影響。

In this essay I want to explore a number of interesting architectures and describe my interpretation of their most interesting features. My hope is that this will provide a context for understanding the patterns that I describe.

在這篇文章中，我想探討一些有趣的架構，並描述我對它們最有趣特徵的詮釋。我希望這能為理解我描述的模式提供一個背景。

To some extent you can see this essay as a kind of intellectual history that traces ideas in UI design through multiple architectures over the years. However I must issue a caution about this. Understanding architectures isn't easy, especially when many of them change and die. Tracing the spread of ideas is even harder, because people read different things from the same architecture. In particular I have not done an exhaustive examination of the architectures I describe. What I have done is referred to common descriptions of the designs. If those descriptions miss things out, I'm utterly ignorant of that. So don't take my descriptions as authoritative. Furthermore there are things I've left out or simplified if I didn't think they were particularly relevant. Remember my primary interest is the underlying patterns, not in the history of these designs.

在某種程度上，你可以將這篇文章視為一種智識史，追溯多年間UI設計中的理念，經由多種架構的演變。然而，我必須對此提出一個警告。理解架構並不容易，尤其是當許多架構隨時間變化或消失時。追蹤理念的傳播更為困難，因為人們從同一個架構中讀取到不同的內容。特別是我對所描述的架構並未進行詳盡的檢查。我所做的只是參考了一些設計的常見描述。如果這些描述有遺漏，我對此完全無知。所以不要將我的描述視為權威。此外，如果我認為某些內容不太相關，就會將其省略或簡化。請記住，我的主要興趣在於基礎模式，而非這些設計的歷史。

(There is something of an exception here, in that I did have access to a running Smalltalk-80 to examine MVC. Again I wouldn't describe my examination of it as exhaustive, but it did reveal things that common descriptions of it failed to - which even further makes me cautious about descriptions of other architectures that I have here. If you are familiar with one of these architectures and you see I have something important that is incorrect and missing I'd like to know about it. I also think that a more exhaustive survey of this territory would be a good object of academic study.)

（這裡有一個例外，我曾使用運行中的Smalltalk-80來研究MVC。再一次強調，我不會將我的研究描述為詳盡無遺，但它揭示了一些常見描述中未提及的事情——這讓我對於其他架構的描述更加謹慎。如果你熟悉其中一個架構並發現我有重要的錯誤或遺漏，我希望能了解這些。進行更詳盡的調查將是學術研究的一個良好對象。）

## Forms and Controls

I shall begin this exploration with an architecture that is both simple and familiar. It doesn't have a common name, so for the purposes of this essay I shall call it “Forms and Controls”. It's a familiar architecture because it was the one encouraged by client-server development environments in the 90's - tools like Visual Basic, Delphi, and Powerbuilder. It continues to be commonly used, although also often vilified by design geeks like me.

我將從一個既簡單又熟悉的架構開始探索。這個架構沒有通用的名稱，因此在這篇文章中，我將其稱為「表單與控制項」。它是一個熟悉的架構，因為在90年代的客戶端-伺服器開發環境中，工具如Visual Basic、Delphi和Powerbuilder都鼓勵使用這種架構。儘管設計狂熱者如我經常批評它，它仍然常被使用。

To explore it, and indeed the other architectures, I'll use a common example. In New England, where I live, there is a government program that monitors the amount of ice-cream particulate in the atmosphere. If the concentration is too low, this indicates that we aren't eating enough ice-cream - which poses a serious risk to our economy and public order. (I like to use examples that are no less realistic as you usually find in books like this.)

為了探索它，並且確實探索其他架構，我將使用一個常見的例子。在我居住的新英格蘭地區，有一個政府項目監測大氣中的冰淇淋顆粒含量。如果濃度過低，這表明我們吃的冰淇淋不夠，這對經濟和公共秩序構成嚴重威脅。（我喜歡使用的例子通常與你在這類書中看到的例子同樣現實。）

To monitor our ice-cream health, the government has set up monitoring stations all over the New England states. Using complex atmospheric modeling the department sets a target for each monitoring station. Every so often staffers go out on an assessment where they go to various stations and note the actual ice-cream particulate concentrations. This UI allows them to select a station, and enter the date and actual value. The system then calculates and displays the variance from the target. The system highlights the variance in red when it is 10% or more below the target, or in green when 5% or more above the target.

為了監控我們的冰淇淋健康，政府在新英格蘭各州設立了監測站。通過複雜的大氣建模，該部門為每個監測站設定了一個目標。每隔一段時間，工作人員會進行評估，前往各個站點並記錄實際的冰淇淋顆粒濃度。這個UI允許他們選擇站點，輸入日期和實際數值。系統隨後計算並顯示與目標的差異。當差異低於目標10%或更多時，系統以紅色突出顯示，當高於目標5%或更多時，則以綠色突出顯示。

![Figure 1: The UI I'll use as an example.](https://martinfowler.com/eaaDev/uiArchs/assessmentUI.gif)

As we look at this screen we can see there is an important division as we put it together. The form is specific to our application, but it uses controls that are generic. Most GUI environments come with a hefty bunch of common controls that we can just use in our application. We can build new controls ourselves, and often it's a good idea to do so, but there is still a distinction between generic reusable controls and specific forms. Even specially written controls can be reused across multiple forms.

當我們查看這個螢幕時，可以看到在組裝時有一個重要的區分。表單是針對我們的應用程式特定的，但它使用的控制項是通用的。大多數GUI環境都提供一堆常見的控制項，讓我們可以直接在應用程式中使用。我們可以自己構建新的控制項，通常這是個好主意，但通用的可重用控制項與特定的表單之間仍然存在區別。即使是特別編寫的控制項，也可以在多個表單中重複使用。

The form contains two main responsibilities:

表單包含兩個主要責任：

- Screen layout: defining the arrangement of the controls on the screen, together with their hierarchic structure with one other.
- Form logic: behavior that cannot be easily programmed into the controls themselves.

- 螢幕佈局：定義控制項在螢幕上的排列方式及其層次結構。
- 表單邏輯：無法輕易編程到控制項本身的行為。

Most GUI development environments allow the developer to define screen layout with a graphical editor that allows you to drag and drop the controls onto a space in the form. This pretty much handles the form layout. This way it's easy to setup a pleasing layout of controls on the form (although it isn't always the best way to do it - we'll come to that later.)

大多數GUI開發環境允許開發者使用圖形編輯器來定義螢幕佈局，這種編輯器讓你可以拖放控制項到表單中的空間。這基本上處理了表單的佈局。這種方式使得在表單上設置令人滿意的控制項佈局變得容易（雖然這不一定是最好的方式——我們稍後會談到這一點）。

The controls display data - in this case about the reading. This data will pretty much always come from somewhere else, in this case let's assume a SQL database as that's the environment that most of these client-server tools assume. In most situations there are three copies of the data involved:

控制項顯示數據——在這個例子中是讀數。這些數據幾乎總是來自其他地方，在這裡我們假設來自SQL數據庫，因為這是大多數這些客戶端-伺服器工具假設的環境。在大多數情況下，涉及到三份數據副本：

- One copy of data lies in the database itself. This copy is the lasting record of the data, so I call it the **record state**. The record state is usually shared and visible to multiple people via various mechanisms.
- 一份數據副本位於數據庫本身中。這份副本是數據的永久記錄，因此我稱其為**記錄狀態**。記錄狀態通常通過各種機制共享並對多人可見。
- A further copy lies inside in-memory [Record Sets](https://martinfowler.com/eaaCatalog/recordSet.html) within the application. Most client-server environments provided tools which made this easy to do. This data was only relevant for one particular session between the application and the database, so I call it **session state**. Essentially this provides a temporary local version of the data that the user works on until they save, or commit it, back to the database - at which point it merges with the record state. I won't worry about the issues around coordinating record state and session state here: I did go into various techniques in [[P of EAA]](https://martinfowler.com/books.html#eaa).
- 另一份副本位於應用程式內存中的[記錄集](https://martinfowler.com/eaaCatalog/recordSet.html)中。大多數客戶端-伺服器環境提供了使此操作變得容易的工具。這些數據只與應用程式與數據庫之間的特定會話相關，因此我稱其為**會話狀態**。本質上，它提供了用戶在保存或提交回數據庫之前工作的數據的臨時本地版本，這時它與記錄狀態合併。在這裡我不會擔心協調記錄狀態和會話狀態的問題：我在[[P of EAA]](https://martinfowler.com/books.html#eaa)中探討了各種技術。
- The final copy lies inside the GUI components themselves. This, strictly, is the data they see on the screen, hence I call it the **screen state**. It is important to the UI how screen state and session state are kept synchronized.
- 最後一份副本位於GUI組件本身內。這嚴格來說就是它們在螢幕上顯示的數據，因此我稱之為**螢幕狀態**。螢幕狀態與會話狀態如何保持同步對UI很重要。

Keeping screen state and session state synchronized is an important task. A tool that helped make this easier was [Data Binding](https://martinfowler.com/eaaDev/DataBinding.html). The idea was that any change to either the control data, or the underlying record set was immediately propagated to the other. So if I alter the actual reading on the screen, the text field control effectively updates the correct column in the underlying record set.

保持螢幕狀態和會話狀態同步是一項重要的任務。一種有助於使這一點變得更容易的工具是[數據綁定](https://martinfowler.com/eaaDev/DataBinding.html)。這個想法是，對控制項數據或底層記錄集的任何更改都會立即傳播到另一個。因此，如果我在螢幕上更改實際讀數，文本字段控制項會有效地更新底層記錄集中的正確列。

In general data binding gets tricky because if you have to avoid cycles where a change to the control, changes the record set, which updates the control, which updates the record set.... The flow of usage helps avoid these - we load from the session state to the screen when the screen is opened, after that any changes to the screen state propagate back to the session state. It's unusual for the session state to be updated directly once the screen is up. As a result data binding might not be entirely bi-directional - just confined to initial upload and then propagating changes from the controls to the session state.

總體而言，數據綁定變得棘手，因為你必須避免循環——控制項的更改更改了記錄集，然後更新了控制項，接著又更新了記錄集......使用流有助於避免這些——我們在螢幕打開時從會話狀態加載到螢幕，之後任何對螢幕狀態的更改都會傳播回會話狀態。一旦螢幕顯示出來，很少直接更新會話狀態。因此，數據綁定可能並不完全是雙向的——僅限於初始上傳，然後將更改從控制項傳播到會話狀態。

[Data Binding](https://martinfowler.com/eaaDev/DataBinding.html) handles much of the functionality of a client-sever application pretty nicely. If I change the actual value the column is updated, even changing the selected station alters the currently selected row in the record set, which causes the other controls to refresh.

[數據綁定](https://martinfowler.com/eaaDev/DataBinding.html) 很好地處理了客戶端-伺服器應用程式的大部分功能。如果我更改了實際值，該列會被更新，即使更改選定的站點，也會改變記錄集中當前選定的行，從而導致其他控制項刷新。

Much of this behavior is built in by the framework builders, who look at common needs and make it easy to satisfy them. In particular this is done by setting values, usually called properties, on the controls. The control binds to a particular column in a record set by having its column name set through a simple property editor.

這種行為中的大部分是由框架構建者內置的，他們考慮到常見需求並使其容易滿足。特別是通過在控制項上設置值，通常稱為屬性，來實現。控制項通過其列名在簡單的屬性編輯器中設置，綁定到記錄集中的特定列。

Using data binding, with the right kind of parameterization, can take you a long way. However it can't take you all the way - there's almost always some logic that won't fit with the parameterization options. In this case calculating the variance is an example of something that doesn't fit in this built in behavior - since it's application specific it usually lies in the form.

使用數據綁定，加上適當的參數化設置，可以走很長一段路。然而，它無法涵蓋所有情況——幾乎總會有一些邏輯無法適應參數化選項。在這種情況下，計算差異就是一個例子，這種邏輯不屬於內置行為——因為它是應用程式特定的，通常位於表單中。

In order for this to work the form needs to be alerted whenever the value of the actual field changes, which requires the generic text field to call some specific behavior on the form. This is a bit more involved than taking a class library and using it through calling it as Inversion of Control is involved.

為了使這一切正常工作，每當實際字段的值發生變化時，表單需要被提醒，這需要通用文本字段調用表單上的一些特定行為。這比通過調用來使用類庫更複雜，因為涉及了控制反轉。

There are various ways of getting this kind of thing to work - the common one for client-server toolkits was the notion of events. Each control had a list of events it could raise. Any external object could tell a control that it was interested in an event - in which case the control would call that external object when the event was raised. Essentially this is just a rephrasing of the [Observer](http://www.amazon.com/exec/obidos/tg/detail/-/0201633612) pattern where the form is observing the control. The framework usually provided some mechanism where the developer of the form could write code in a subroutine that would be invoked when the event occurred. Exactly how the link was made between event and routine varied between platform and is unimportant for this discussion - the point is that some mechanism existed to make it happen.

有各種方式可以實現這種工作——客戶端-伺服器工具包中常見的方法是事件的概念。每個控制項都有一個可以觸發的事件列表。任何外部對象都可以告訴控制項它對某個事件感興趣——在這種情況下，當事件觸發時，控制項會調用該外部對象。基本上，這只是[觀察者](http://www.amazon.com/exec/obidos/tg/detail/-/0201633612)模式的一種重述，其中表單觀察控制項。框架通常提供一些機制，使表單的開發者可以在事件發生時寫在子程序中的代碼被調用。不同平台之間事件與例程之間的鏈接方式有所不同，但這對我們的討論並不重要——重點是存在某種機制來實現這一點。

Once the routine in the form has control, it can then do whatever is needed. It can carry out the specific behavior and then modify the controls as necessary, relying on data binding to propagate any of these changes back to the session state.

一旦表單中的例程獲得控制權，它就可以執行所需的任何操作。它可以執行特定的行為，然後根據需要修改控制項，依賴數據綁定將這些更改傳播回會話狀態。

This is also necessary because data binding isn't always present. There is a large market for windows controls, not all of them do data binding. If data binding isn't present then it's up to the form to carry out the synchronization. This could work by pulling data out of the record set into the widgets initially, and copying the changed data back to the record set when the save button was pressed.

這也是必要的，因為數據綁定並不總是存在。市場上有許多Windows控制項，不是所有控制項都支持數據綁定。如果沒有數據綁定，則需要表單來執行同步。這可以通過最初從記錄集中提取數據到小部件中，然後在按下保存按鈕時將更改的數據複製回記錄集來實現。

Let's examine our editing of the actual value, assuming that data binding is present. The form object holds direct references to the generic controls. There'll be one for each control on the screen, but I'm just interested in the actual, variance, and target fields here.

讓我們檢查一下對實際值的編輯，假設數據綁定是存在的。表單對象直接持有對通用控制項的引用。螢幕上的每個控制項都有一個，但這裡我只關心實際值、差異和目標字段。


![Figure 2: Class diagram for forms and controls](https://martinfowler.com/eaaDev/uiArchs/formsAndControls-cd.gif)

The text field declares an event for text changed, when the form assembles the screen during initialization it subscribes itself to that event, binding it a method on itself - here `actual_textChanged`.

文本字段聲明了一個文本更改的事件，當表單在初始化期間組裝螢幕時，它將自己訂閱到該事件，將其綁定到自身的一個方法——在此處是`actual_textChanged`。

![Figure 3: Sequence diagram for changing a genre with forms and controls.](https://martinfowler.com/eaaDev/uiArchs/formsAndControls-seq.gif)

When the user changes the actual value, the text field control raises its event and through the magic of framework binding the `actual_textChanged` is run. This method gets the text from the actual and target text fields, does the subtraction, and puts the value into the variance field. It also figures out what color the value should be displayed with and adjusts the text color appropriately.

當用戶更改實際值時，文本字段控制項觸發其事件，通過框架綁定的魔力，`actual_textChanged`方法被運行。這個方法從實際和目標文本字段中獲取文本，進行減法運算，然後將值放入差異字段中。它還會計算出應該顯示的顏色並適當調整文本顏色。

We can summarize the architecture with a few soundbites:

我們可以用一些簡短的總結來概述這個架構：

- Developers write application specific forms that use generic controls.
- The form describes the layout of controls on it.
- The form observes the controls and has handler methods to react to interesting events raised by the controls.
- Simple data edits are handled through data binding.
- Complex changes are done in the form's event handling methods.

- 開發人員編寫使用通用控制項的應用程式特定表單。
- 表單描述了其上的控制項佈局。
- 表單觀察控制項，並具有處理控制項引發的有趣事件的處理程序方法。
- 簡單的數據編輯通過數據綁定處理。
- 複雜的更改在表單的事件處理方法中完成。

## Model View Controller

Probably the widest quoted pattern in UI development is Model View Controller (MVC) - it's also the most misquoted. I've lost count of the times I've seen something described as MVC which turned out to be nothing like it. Frankly a lot of the reason for this is that parts of classic MVC don't really make sense for rich clients these days. But for the moment we'll take a look at its origins.

在UI開發中，可能被引用最廣泛的模式是模型-視圖-控制器（MVC）——它也是最被誤解的。我無法計算看到多少次某些被描述為MVC的東西與實際的MVC完全不同。坦率地說，這很大程度上是因為經典MVC的一部分對於如今的豐富客戶端來說並不太有意義。但目前我們先來看看它的起源。

As we look at MVC it's important to remember that this was one of the first attempts to do serious UI work on any kind of scale. Graphical User Interfaces were not exactly common in the 70's. The Forms and Controls model I've just described came after MVC - I've described it first because it's simpler, not always in a good way. Again I'll discuss Smalltalk 80's MVC using the assessment example - but be aware that I am taking a few liberties with the actual details of Smalltalk 80 to do this - for start it was a monochrome system.

當我們看MVC時，重要的是要記住這是首次在任何規模上進行嚴肅UI工作的嘗試之一。在70年代，圖形用戶界面還不是很常見。我剛剛描述的表單與控制項模型是在MVC之後出現的——我首先描述它是因為它更簡單，雖然這並不總是件好事。再次提醒，使用評估示例來討論Smalltalk 80的MVC——但請注意，為了做到這一點，我對Smalltalk 80的實際細節進行了一些調整——首先它是一個單色系統。

At the heart of MVC, and the idea that was the most influential to later frameworks, is what I call [Separated Presentation](https://martinfowler.com/eaaDev/SeparatedPresentation.html). The idea behind [Separated Presentation](https://martinfowler.com/eaaDev/SeparatedPresentation.html) is to make a clear division between domain objects that model our perception of the real world, and presentation objects that are the GUI elements we see on the screen. Domain objects should be completely self contained and work without reference to the presentation, they should also be able to support multiple presentations, possibly simultaneously. This approach was also an important part of the Unix culture, and continues today allowing many applications to be manipulated through both a graphical and command-line interface.

MVC的核心，以及對後來框架影響最大的概念，我稱之為[分離表示](https://martinfowler.com/eaaDev/SeparatedPresentation.html)。[分離表示](https://martinfowler.com/eaaDev/SeparatedPresentation.html)背後的想法是，將我們對現實世界的感知建模的域對象，與我們在螢幕上看到的GUI元素的表示對象之間劃出明確的界線。域對象應該是完全自包含的，並且可以在不參考表示的情況下工作，它們還應該能夠支持多個表示，甚至可以同時支持多個表示。這種方法也是Unix文化的一個重要組成部分，並且至今仍然存在，允許許多應用程式可以通過圖形界面和命令行界面進行操作。

In MVC, the domain element is referred to as the model. Model objects are completely ignorant of the UI. To begin discussing our assessment UI example we'll take the model as a reading, with fields for all the interesting data upon it. (As we'll see in a moment the presence of the list box makes this question of what is the model rather more complex, but we'll ignore that list box for a little bit.)

在MVC中，領域元素稱為模型。模型對象完全不會知道UI的存在。為了開始討論我們的評估UI範例，我們將模型視為一個包含所有相關數據字段的讀數對象。（正如我們稍後將看到的，列表框的存在使得這個什麼是模型的問題更加複雜，但我們會暫時忽略列表框。）

In MVC I'm assuming a [Domain Model](https://martinfowler.com/eaaCatalog/domainModel.html) of regular objects, rather than the [Record Set](https://martinfowler.com/eaaCatalog/recordSet.html) notion that I had in Forms and Controls. This reflects the general assumption behind the design. Forms and Controls assumed that most people wanted to easily manipulate data from a relational database, MVC assumes we are manipulating regular Smalltalk objects.

在MVC中，我假設一個[領域模型](https://martinfowler.com/eaaCatalog/domainModel.html)是由常規對象組成，而不是像在表單與控制項中那樣的[記錄集](https://martinfowler.com/eaaCatalog/recordSet.html)概念。這反映了設計背後的一般假設。表單與控制項假設大多數人希望輕鬆操控關聯式數據庫中的數據，而MVC則假設我們在操控常規的Smalltalk對象。

The presentation part of MVC is made of the two remaining elements: view and controller. The controller's job is to take the user's input and figure out what to do with it.

MVC的表示部分由剩餘的兩個元素組成：視圖和控制器。控制器的工作是接受用戶的輸入並決定如何處理它。

At this point I should stress that there's not just one view and controller, you have a view-controller pair for each element of the screen, each of the controls and the screen as a whole. So the first part of reacting to the user's input is the various controllers collaborating to see who got edited. In this case that's the actuals text field so that text field controller would now handle what happens next.

此時我應該強調，不只有一個視圖和控制器，每個螢幕元素都有一對視圖-控制器，包含每個控制項和整個螢幕。因此，對用戶輸入的首次反應是各個控制器協作以確定誰被編輯了。在這個例子中，實際的文本字段就是這樣，該文本字段的控制器將處理接下來的事情。


![Figure 4: Essential dependencies between model, view, and controller. (I call this essential because in fact the view and controller do link to each other directly, but developers mostly don't use this fact.)](https://martinfowler.com/eaaDev/uiArchs/mvc-deps.gif)


Like later environments, Smalltalk figured out that you wanted generic UI components that could be reused. In this case the component would be the view-controller pair. Both were generic classes, so needed to be plugged into the application specific behavior. There would be an assessment view that would represent the whole screen and define the layout of the lower level controls, in that sense similar to a form in Forms and Controllers. Unlike the form, however, MVC has no event handlers on the assessment controller for the lower level components.

像後來的環境一樣，Smalltalk發現你想要可以重用的通用UI組件。在這種情況下，組件將是視圖-控制器對。兩者都是通用類，因此需要插入應用程式特定的行為。將有一個評估視圖來表示整個螢幕，並定義較低級別控制項的佈局，在這一點上類似於表單與控制項中的表單。然而，與表單不同的是，MVC在較低級別組件的評估控制器上沒有事件處理程序。


![Figure 5: Classes for an MVC version of an ice-cream monitor display](https://martinfowler.com/eaaDev/uiArchs/mvc-cd.gif)

The configuration of the text field comes from giving it a link to its model, the reading, and telling it what what method to invoke when the text changes. This is set to '#actual:' when the screen is initialized (a leading '#' indicates a symbol, or interned string, in Smalltalk). The text field controller then makes a reflective invocation of that method on the reading to make the change. Essentially this is the same mechanism as occurs for [Data Binding](https://martinfowler.com/eaaDev/DataBinding.html), the control is linked to the underlying object (row) and told which method (column) it manipulates.

文本字段的配置來自於給它一個鏈接到其模型的讀數，並告訴它當文本更改時要調用的方法。這在螢幕初始化時設置為`#actual:`（在Smalltalk中，前導`#`表示一個符號或內部字符串）。文本字段控制器然後在讀數上反射性地調用該方法來進行更改。本質上，這與[數據綁定](https://martinfowler.com/eaaDev/DataBinding.html)的機制相同，控制項鏈接到底層對象（行）並告知其操作的方法（列）。


![Figure 6: Changing the actual value for MVC.](https://martinfowler.com/eaaDev/uiArchs/mvc-seq.gif)

So there is no overall object observing low level widgets, instead the low level widgets observe the model, which itself handles many of the decision that would be made by the form. In this case, when it comes to figuring out the variance, the reading object itself is the natural place to do that.

因此，沒有總體對象觀察低級小部件，反而是低級小部件觀察模型，模型本身處理許多表單會做出的決定。在這種情況下，當涉及到計算差異時，讀數對象本身就是做這件事的自然地方。

Observers do occur in MVC, indeed it's one of the ideas credited to MVC. In this case all the views and controllers observe the model. When the model changes, the views react. In this case the actual text field view is notified that the reading object has changed, and invokes the method defined as the aspect for that text field - in this case #actual - and sets its value to the result. (It does something similar for the color, but this raises its own specters that I'll get to in a moment.)

觀察者確實在MVC中出現，事實上，它是被歸功於MVC的一個概念。在這種情況下，所有的視圖和控制器都觀察模型。當模型發生變化時，視圖會做出反應。在這個例子中，實際文本字段視圖被通知到讀數對象發生了變化，並調用作為該文本字段方面的定義方法——在這種情況下是`#actual`——並將其值設置為結果。（它對顏色也做了類似的事情，但這引發了我稍後會討論的問題。）

You'll notice that the text field controller didn't set the value in the view itself, it updated the model and then just let the observer mechanism take care of the updates. This is quite different to the forms and controls approach where the form updates the control and relies on data binding to update the underlying record-set. These two styles I describe as patterns: [Flow Synchronization](https://martinfowler.com/eaaDev/FlowSynchronization.html) and [Observer Synchronization](https://martinfowler.com/eaaDev/MediatedSynchronization.html). These two patterns describe alternative ways of handling the triggering of synchronization between screen state and session state. Forms and Controls do it through the flow of the application manipulating the various controls that need to be updated directly. MVC does it by making updates on the model and then relying of the observer relationship to update the views that are observing that model.

您會注意到，文本字段控制器並沒有在視圖本身中設置值，它更新了模型，然後僅依賴觀察者機制來處理更新。這與表單與控制項方法完全不同，表單更新控制項並依賴數據綁定來更新底層的記錄集。這兩種樣式我描述為模式：[流程同步](https://martinfowler.com/eaaDev/FlowSynchronization.html)和[觀察者同步](https://martinfowler.com/eaaDev/MediatedSynchronization.html)。這兩種模式描述了在螢幕狀態與會話狀態之間觸發同步的替代方式。表單與控制項通過應用程式流來直接操控需要更新的各個控制項進行同步。MVC則通過對模型進行更新，然後依賴觀察者關係來更新觀察該模型的視圖。

[Flow Synchronization](https://martinfowler.com/eaaDev/FlowSynchronization.html) is even more apparent when data binding isn't present. If the application needs to do synchronization itself, then it was typically done at important point in the application flow - such as when opening a screen or hitting the save button.

當數據綁定不存在時，[流程同步](https://martinfowler.com/eaaDev/FlowSynchronization.html)更加明顯。如果應用程式需要自己進行同步，通常會在應用程式流中的重要點——例如打開螢幕或按下保存按鈕時進行同步。

One of the consequences of [Observer Synchronization](https://martinfowler.com/eaaDev/MediatedSynchronization.html) is that the controller is very ignorant of what other widgets need to change when the user manipulates a particular widget. While the form needs to keep tabs on things and make sure the overall screen state is consistent on a change, which can get pretty involved with complex screens, the controller in [Observer Synchronization](https://martinfowler.com/eaaDev/MediatedSynchronization.html) can ignore all this.

[觀察者同步](https://martinfowler.com/eaaDev/MediatedSynchronization.html)的一個後果是控制器對用戶操控特定小部件時其他小部件需要更改的情況知之甚少。表單需要跟踪情況並確保在更改時整個螢幕狀態一致，這在復雜螢幕上可能會變得非常複雜。而[觀察者同步](https://martinfowler.com/eaaDev/MediatedSynchronization.html)中的控制器則可以忽略這些。

This useful ignorance becomes particularly handy if there are multiple screens open viewing the same model objects. The classic MVC example was a spreadsheet like screen of data with a couple of different graphs of that data in separate windows. The spreadsheet window didn't need to be aware of what other windows were open, it just changed the model and [Observer Synchronization](https://martinfowler.com/eaaDev/MediatedSynchronization.html) took care of the rest. With [Flow Synchronization](https://martinfowler.com/eaaDev/FlowSynchronization.html) it would need some way of knowing which other windows were open so it tell them to refresh.

這種有用的無知在有多個螢幕打開觀看同一模型對象時特別方便。經典的MVC例子是一個類似於數據電子表格的螢幕，在單獨的窗口中有幾個不同的該數據圖表。電子表格窗口不需要知道打開了哪些其他窗口，它只需更改模型，[觀察者同步](https://martinfowler.com/eaaDev/MediatedSynchronization.html)負責處理其餘部分。而使用[流程同步](https://martinfowler.com/eaaDev/FlowSynchronization.html)則需要某種方式知道哪些其他窗口打開了，以便告訴它們刷新。

While [Observer Synchronization](https://martinfowler.com/eaaDev/MediatedSynchronization.html) is nice it does have a downside. The problem with [Observer Synchronization](https://martinfowler.com/eaaDev/MediatedSynchronization.html) is the core problem of the observer pattern itself - you can't tell what is happening by reading the code. I was reminded of this very forcefully when trying to figure out how some Smalltalk 80 screens worked. I could get so far by reading the code, but once the observer mechanism kicked in the only way I could see what was going on was via a debugger and trace statements. Observer behavior is hard to understand and debug because it's implicit behavior.

雖然[觀察者同步](https://martinfowler.com/eaaDev/MediatedSynchronization.html)很不錯，但它也有一些缺點。觀察者同步的問題就是觀察者模式本身的核心問題——你無法通過閱讀代碼來了解發生了什麼事。我在嘗試理解一些Smalltalk 80螢幕如何工作的時候，深刻體會到了這一點。我可以通過閱讀代碼走得很遠，但一旦觀察者機制啟動，唯一能看清楚發生了什麼的辦法就是通過調試器和追蹤語句。觀察者行為難以理解和調試，因為它是隱式的行為。

While the different approaches to synchronization are particularly noticeable from looking at the sequence diagram, the most important, and most influential, difference is MVC's use of [Separated Presentation](https://martinfowler.com/eaaDev/SeparatedPresentation.html). Calculating the variance between actual and target is domain behavior, it is nothing to do with the UI. As a result following [Separated Presentation](https://martinfowler.com/eaaDev/SeparatedPresentation.html) says we should place this in the domain layer of the system - which is exactly what the reading object represents. When we look at the reading object, the variance feature makes complete sense without any notion of the user interface.

從查看序列圖來看，不同的同步方法特別明顯，但最重要且最具影響力的區別是MVC使用了[分離表示](https://martinfowler.com/eaaDev/SeparatedPresentation.html)。計算實際值與目標值之間的變異是領域行為，與UI無關。因此，遵循[分離表示](https://martinfowler.com/eaaDev/SeparatedPresentation.html)的原則，我們應該將這部分放在系統的領域層中——這正是讀數對象所代表的。當我們查看讀數對象時，變異功能在沒有任何UI概念的情況下就能完全理解。

At this point, however, we can begin to look at some complications. There's two areas where I've skipped over some awkward points that get in the way of MVC theory. The first problem area is to deal with setting the color of the variance. This shouldn't really fit into a domain object, as the color by which we display a value isn't part of the domain. The first step in dealing with this is to realize that part of the logic is domain logic. What we are doing here is making a qualitative statement about the variance, which we could term as good (over by more than 5%), bad (under by more than 10%), and normal (the rest). Making that assessment is certainly domain language, mapping that to colors and altering the variance field is view logic. The problem lies in where we put this view logic - it's not part of our standard text field.

然而，現在我們可以開始看看一些複雜的地方。有兩個區域我跳過了一些尷尬的地方，這些地方會妨礙MVC理論的應用。第一個問題區域是處理變異值顏色的設定。這不應該放在領域對象中，因為顯示值的顏色並不是領域的一部分。處理這個問題的第一步是認識到其中部分邏輯是領域邏輯。我們在這裡做的是對變異進行定性描述，可以稱之為“良好”（超過5%）、“不良”（少於10%）和“正常”（其他情況）。進行這種評估無疑是領域語言，而將其映射到顏色並更改變異字段則是視圖邏輯。問題在於我們將這些視圖邏輯放在哪裡——它不是我們標準文本字段的一部分。

This kind of problem was faced by early smalltalkers and they came up with some solutions. The solution I've shown above is the dirty one - compromise some of the purity of the domain in order to make things work. I'll admit to the occasional impure act - but I try not to make a habit of it.

早期的Smalltalk使用者也面臨了這樣的問題，並提出了一些解決方案。我上面展示的解決方案是妥協的——為了使事情能夠運行，犧牲了部分領域的純粹性。我承認偶爾會有不純潔的行為——但我盡量避免養成這種習慣。

We could do pretty much what Forms and Controls does - have the assessment screen view observe the variance field view, when the variance field changes the assessment screen could react and set the variance field's text color. Problems here include yet more use of the observer mechanism - which gets exponentially more complicated the more you use it - and extra coupling between the various views.

我們可以做的事情幾乎與表單與控制項相同——讓評估螢幕視圖觀察變異字段視圖，當變異字段發生變化時，評估螢幕可以做出反應並設置變異字段的文本顏色。這裡的問題包括更多使用觀察者機制——它會隨著使用的增加而變得越來越複雜——以及各個視圖之間的額外耦合。

A way I would prefer is to build a new type of the UI control. Essentially what we need is a UI control that asks the domain for a qualitative value, compares it to some internal table of values and colors, and sets the font color accordingly. Both the table and message to ask the domain object would be set by the assessment view as it's assembling itself, just as it sets the aspect for the field to monitor. This approach could work very well if I can easily subclass text field to just add the extra behavior. This obviously depends on how well the components are designed to enable sub-classing - Smalltalk made it very easy - other environments can make it more difficult.

我更喜歡的一種方式是構建一種類型的UI控制項。本質上，我們需要的是一個UI控制項，它向領域請求定性值，將其與某個內部的值和顏色表進行比較，然後根據結果設置字體顏色。該表和向領域對象發送消息的設置將由評估視圖在組裝自己時完成，就像它為字段設置監控的方面一樣。如果我可以輕鬆地將文本字段子類化來添加額外的行為，那麼這種方法可以很好地工作。這顯然取決於組件的設計是否支持子類化——Smalltalk使得這非常容易，而其他環境則可能會變得更困難。


![Figure 7: Using a special subclass of text field that can be configured to determine the color.](https://martinfowler.com/eaaDev/uiArchs/mvc-withSubWidget-seq.gif)

The final route is to make a new kind of model object, one that's oriented around around the screen, but is still independent of the widgets. It would be the model for the screen. Methods that were the same as those on the reading object would just be delegated to the reading, but it would add methods that supported behavior relevant only to the UI, such as the text color.

最後的路徑是創建一種類型的新模型對象，它圍繞螢幕構建，但仍然與小部件無關。它將成為螢幕的模型。那些與讀數對象相同的方法將被委託給讀數對象，但它將添加支持僅與UI相關的行為的方法，例如文本顏色。


![](https://martinfowler.com/eaaDev/uiArchs/mvc-withPM-seq.gif)

This last option works well for a number of cases and, as we'll see, became a common route for Smalltalkers to follow - I call this a [Presentation Model](https://martinfowler.com/eaaDev/PresentationModel.html) because it's a model that is really designed for and thus part of the presentation layer. (It's also known as MVVM - “Model-View-ViewModel”.)

這最後一個選項在許多情況下都很有效，正如我們將看到的，這成為Smalltalk使用者常用的路徑——我稱之為[表示模型](https://martinfowler.com/eaaDev/PresentationModel.html)，因為它是一個真正為表示層設計的模型，並且是表示層的一部分。（它也被稱為MVVM——“Model-View-ViewModel”）。

The [Presentation Model](https://martinfowler.com/eaaDev/PresentationModel.html) works well also for another presentation logic problem - presentation state. The basic MVC notion assumes that all the state of the view can be derived from the state of the model. In this case how do we figure out which station is selected in the list box? The [Presentation Model](https://martinfowler.com/eaaDev/PresentationModel.html) solves this for us by giving us a place to put this kind of state. A similar problem occurs if we have save buttons that are only enabled if data has changed - again that's state about our interaction with the model, not the model itself.

[表示模型](https://martinfowler.com/eaaDev/PresentationModel.html)也很好地解決了另一個展示邏輯問題——展示狀態。基本的MVC概念假設視圖的所有狀態都可以從模型的狀態推導出來。在這個例子中，我們如何知道列表框中選擇了哪個項目？[表示模型](https://martinfowler.com/eaaDev/PresentationModel.html)通過為我們提供一個地方來存放這類狀態，解決了這個問題。如果我們有只有在數據發生變更時才啟用的保存按鈕，這也是一個類似的問題——這是關於我們與模型的交互的狀態，而不是模型本身。

So now I think it's time for some soundbites on MVC.

現在，我認為是時候分享一些MVC的要點了。

- Make a strong separation between presentation (view & controller) and domain (model) - [Separated Presentation](https://martinfowler.com/eaaDev/SeparatedPresentation.html).
- Divide GUI widgets into a controller (for reacting to user stimulus) and view (for displaying the state of the model). Controller and view should (mostly) not communicate directly but through the model.
- Have views (and controllers) observe the model to allow multiple widgets to update without needed to communicate directly - [Observer Synchronization](https://martinfowler.com/eaaDev/MediatedSynchronization.html).

- 強烈區分表示（視圖與控制器）與領域（模型）——[分離表示](https://martinfowler.com/eaaDev/SeparatedPresentation.html)。
- 將GUI小部件分為控制器（用來對用戶刺激作出反應）和視圖（用來顯示模型的狀態）。控制器和視圖（大多數情況下）應該通過模型進行間接通信，而不是直接通信。
- 讓視圖（和控制器）觀察模型，從而允許多個小部件更新，而無需直接通信——[觀察者同步](https://martinfowler.com/eaaDev/MediatedSynchronization.html)。

## VisualWorks Application Model

As I've discussed above, Smalltalk 80's MVC was very influential and had some excellent features, but also some faults. As Smalltalk developed in the 80's and 90's this led to some significant variations on the classic MVC model. Indeed one could almost say that MVC disappeared, if you consider the view/controller separation to be an essential part of MVC - which the name does imply.

如我之前所討論的，Smalltalk 80的MVC具有很大的影響力，並且擁有一些優秀的特性，但也有一些缺點。隨著Smalltalk在80年代和90年代的發展，這導致了經典MVC模型的幾個重大變化。事實上，如果你認為視圖/控制器的分離是MVC的基本組成部分——這是名稱所暗示的——那麼可以說MVC幾乎消失了。

The things that clearly worked from MVC were [Separated Presentation](https://martinfowler.com/eaaDev/SeparatedPresentation.html) and [Observer Synchronization](https://martinfowler.com/eaaDev/MediatedSynchronization.html). So these stayed as Smalltalk developed - indeed for many people they were the key element of MVC.

MVC中明確有效的部分是[分離表示](https://martinfowler.com/eaaDev/SeparatedPresentation.html)和[觀察者同步](https://martinfowler.com/eaaDev/MediatedSynchronization.html)。因此，這些特性在Smalltalk發展過程中得以保留——事實上，對許多人來說，它們是MVC的關鍵元素。

Smalltalk also fragmented in these years. The basic ideas of Smalltalk, including the (minimal) language definition remained the same, but we saw multiple Smalltalks develop with different libraries. From a UI perspective this became important as several libraries started using native widgets, the controls used by the Forms and Controls style.

在這些年中，Smalltalk也出現了碎片化。Smalltalk的基本思想，包括（最小的）語言定義保持不變，但我們看到了多個Smalltalk版本的發展，並且擁有不同的庫。從UI的角度來看，這變得很重要，因為幾個庫開始使用本地小部件，即Forms和Controls風格中使用的控制項。

Smalltalk was originally developed by Xerox Parc labs and they span off a separate company, ParcPlace, to market and develop Smalltalk. ParcPlace Smalltalk was called VisualWorks and made a point of being a cross-platform system. Long before Java you could take a Smalltalk program written in Windows and run it right away on Solaris. As a result VisualWorks didn't use native widgets and kept the GUI completely within Smalltalk.

Smalltalk最初是由Xerox Parc實驗室開發的，並且他們創立了一家公司ParcPlace來推廣和開發Smalltalk。ParcPlace Smalltalk被稱為VisualWorks，並強調它是一個跨平台系統。在Java出現之前，你就可以將一個在Windows上編寫的Smalltalk程序，立刻在Solaris上運行。因此，VisualWorks沒有使用本地小部件，而是將GUI完全保留在Smalltalk中。

In my discussion of MVC I finished with some problems of MVC - particularly how to deal with view logic and view state. VisualWorks refined its framework to deal with this by coming up with a construct called the Application Model - a construct that moves towards [Presentation Model](https://martinfowler.com/eaaDev/PresentationModel.html). The idea of using something like a [Presentation Model](https://martinfowler.com/eaaDev/PresentationModel.html) wasn't new to VisualWorks - the original Smalltalk 80 code browser was very similar, but the VisualWorks Application Model baked it fully into the framework.

在我對MVC的討論中，我結束時提到了一些MVC的問題——尤其是如何處理視圖邏輯和視圖狀態。VisualWorks通過提出一個叫做應用程序模型（Application Model）的結構來改進其框架，這是一個接近[表示模型](https://martinfowler.com/eaaDev/PresentationModel.html)的概念。使用類似[表示模型](https://martinfowler.com/eaaDev/PresentationModel.html)的想法對VisualWorks來說並不新鮮——原始的Smalltalk 80代碼瀏覽器非常相似，但VisualWorks的應用程序模型將它完全嵌入到框架中。

A key element of this kind of Smalltalk was the idea of turning properties into objects. In our usual notion of objects with properties we think of a Person object having properties for name and address. These properties may be fields, but could be something else. There is usually a standard convention for accessing the properties: in Java we would see `temp = aPerson.getName()` and `aPerson.setName(“martin”)`, in C# it would `temp = aPerson.name` and `aPerson.name = “martin”`.

這種Smalltalk的一個關鍵元素是將屬性轉換為對象的想法。在我們通常對具有屬性的對象的概念中，我們認為一個Person對象有name和address屬性。這些屬性可能是字段，但也可能是其他東西。通常會有一個標準的慣例來訪問這些屬性：在Java中，我們會看到`temp = aPerson.getName()`和`aPerson.setName("martin")`，在C#中則是`temp = aPerson.name`和`aPerson.name = "martin"`。

A **Property Object** changes this by having the property return an object that wraps the actual value. So in VisualWorks when we ask for a name we get back a wrapping object. We then get the actual value by asking the wrapping object for its value. So accessing a person's name would use `temp = aPerson name value` and `aPerson name value: 'martin'`

**屬性對象（Property Object）** 改變了這一點，將屬性返回一個包裝實際值的對象。因此，在VisualWorks中，當我們請求一個名字時，我們得到的是一個包裝對象。然後，我們通過向包裝對象請求其值來獲得實際值。所以，訪問一個人的名字會使用`temp = aPerson name value`和`aPerson name value: 'martin'`。

Property objects make the mapping between widgets and model a little easier. We just have to tell the widget what message to send to get the corresponding property, and the widget knows to access the proper value using `value` and `value:`. VisualWorks's property objects also allow you to set up observers with the message onChangeSend: aMessage to: anObserver.

屬性對象使得小部件和模型之間的映射變得稍微容易一些。我們只需要告訴小部件發送什麼消息來獲取對應的屬性，而小部件知道如何使用`value`和`value:`來訪問正確的值。VisualWorks的屬性對象還允許你設置觀察者，通過消息`onChangeSend: aMessage to: anObserver`。

You won't actually find a class called property object in Visual Works. Instead there were a number of classes that followed the value/value:/onChangeSend: protocol. The simplest is the ValueHolder - which just contains its value. More relevant to this discussion is the AspectAdaptor. The AspectAdaptor allowed a property object to wrap a property of another object completely. This way you could define a property object on a PersonUI class that wrapped a property on a Person object by code like

你實際上不會在VisualWorks中找到一個叫做屬性對象的類。相反，有很多遵循`value/value:/onChangeSend:`協議的類。最簡單的是ValueHolder——它只包含其值。與這次討論最相關的是AspectAdaptor。AspectAdaptor允許一個屬性對象完全包裝另一個對象的屬性。這樣，你可以定義一個在PersonUI類中的屬性對象，它包裝了一個Person對象的屬性，例如以下代碼：

```
adaptor := AspectAdaptor subject: person
adaptor forAspect: #name
adaptor onChangeSend: #redisplay to: self
```

So let's see how the application model fits into our running example.

現在，讓我們看看應用程序模型如何適用於我們的示例。


![](https://martinfowler.com/eaaDev/uiArchs/appModel-cd.gif)

The main difference between using an application model and classic MVC is that we now have an intermediate class between the domain model class (Reader) and the widget - this is the application model class. The widgets don't access the domain objects directly - their model is the application model. Widgets are still broken down into views and controllers, but unless you're building new widgets that distinction isn't important.

使用應用程序模型與經典MVC的主要區別在於，我們現在有一個介於領域模型類（Reader）和小部件之間的中介類——這就是應用程序模型類。小部件不直接訪問領域對象——它們的模型是應用程序模型。小部件仍然被劃分為視圖和控制器，但除非你正在構建新的小部件，否則這種區分並不重要。

When you assemble the UI you do so in a UI painter, while in that painter you set the aspect for each widget. The aspect corresponds to a method on the application model that returns a property object.

當你組裝UI時，你是在UI畫圖工具中進行的，在那個畫圖工具中你為每個小部件設置屬性。該屬性對應於應用程序模型中的一個方法，該方法返回一個屬性對象。


![](https://martinfowler.com/eaaDev/uiArchs/appModelNoColor-seq.gif)

[Figure 10](https://martinfowler.com/eaaDev/uiArchs.html#uiArchs_appModelNoColor-seq.gif) shows how the basic update sequence works. When I change a value in text field, that field then updates the value in the property object inside the application model. That update follows through to the underlying domain object, updating its actual value.

[圖10](https://martinfowler.com/eaaDev/uiArchs.html#uiArchs_appModelNoColor-seq.gif)展示了基本的更新序列如何工作。當我在文本框中更改一個值時，該字段會更新應用程序模型中屬性對象的值。這個更新會傳遞到底層的領域對象，更新其實際值。

At this point the observer relationships kick in. We need to set things up so that updating the actual value causes the reading to indicate that it has changed. We do this by putting a call in the modifier for actual to indicate that the reading object has changed - in particular that the variance aspect has changed. When setting up the aspect adaptor for variance it's easy to tell it to observe the reader, so it picks up the update message which it then forwards to its text field. The text field then initiates getting a new value, again through the aspect adaptor.

此時，觀察者關係會啟動。我們需要設置，使得更新實際值後，讀數顯示它已經改變。我們通過在修改器中加入一個調用來指示讀數對象已經改變——特別是變異屬性已經改變。在設置變異的屬性適配器時，很容易告訴它觀察讀數對象，這樣它就會收到更新消息，然後將該消息轉發給其文本框。文本框然後啟動獲取新值的過程，這也是通過屬性適配器進行的。

Using the application model and property objects like this helps us wire up the updates without having to write much code. It also supports fine-grained synchronization (which I don't think is a good thing).

像這樣使用應用程序模型和屬性對象幫助我們將更新進行連接，而無需編寫太多代碼。它還支持細粒度同步（我認為這並不是一個好事）。

Application models allow us to separate behavior and state that's particular to the UI from real domain logic. So one of the problems I mentioned earlier, holding the currently selected item in a list, can be solved by using a particular kind of aspect adaptor that wraps the domain model's list and also stores the currently selected item.

應用程序模型讓我們將UI特有的行為和狀態與真實的領域邏輯分開。所以，我之前提到的一個問題，即保持當前選擇的項目在列表中的位置，可以通過使用一種類型的屬性適配器來解決，它包裝了領域模型的列表，並且還存儲當前選擇的項目。

The limitation of all this, however, is that for more complex behavior you need to construct special widgets and property objects. As an example the provided set of objects don't provide a way to link the text color of the variance to the degree of variance. Separating the application and domain models does allow us to separate the decision making in the right way, but then to use widgets observing aspect adapters we need to make some new classes. Often this was seen as too much work, so we could make this kind of thing easier by allowing the application model to access the widgets directly, as in [Figure 11](https://martinfowler.com/eaaDev/uiArchs.html#uiArchs_appModelColoring-seq.gif).

然而，所有這些的限制在於，對於更複雜的行為，你需要構建特殊的小部件和屬性對象。舉個例子，提供的對象集合並沒有提供一種將變異的文本顏色與變異程度關聯的方法。分離應用程序模型和領域模型確實讓我們能夠以正確的方式分開決策，但為了使用觀察屬性適配器的小部件，我們需要創建一些新的類。通常這被認為是太繁瑣的工作，所以我們可以通過允許應用程序模型直接訪問小部件來簡化這個過程，就像在[圖11](https://martinfowler.com/eaaDev/uiArchs.html#uiArchs_appModelColoring-seq.gif)中所示的那樣。


![](https://martinfowler.com/eaaDev/uiArchs/appModelColoring-seq.gif)

Directly updating the widgets like this is not part of [Presentation Model](https://martinfowler.com/eaaDev/PresentationModel.html), which is why the visual works application model isn't truly a [Presentation Model](https://martinfowler.com/eaaDev/PresentationModel.html). This need to manipulate the widgets directly was seen by many as a bit of a dirty workaround and helped develop the Model-View-Presenter approach.

直接更新小部件並非[表示模型](https://martinfowler.com/eaaDev/PresentationModel.html)的一部分，因此VisualWorks的應用程序模型並不真正是[表示模型](https://martinfowler.com/eaaDev/PresentationModel.html)。許多人將直接操作小部件視為一種變通方法，並促進了模型-視圖-呈現者（MVP）方法的發展。

So now the soundbites on Application Model

現在，來看看有關應用程序模型的總結：

- Followed MVC in using [Separated Presentation](https://martinfowler.com/eaaDev/SeparatedPresentation.html) and [Observer Synchronization](https://martinfowler.com/eaaDev/MediatedSynchronization.html).
- Introduced an intermediate application model as a home for presentation logic and state - a partial development of [Presentation Model](https://martinfowler.com/eaaDev/PresentationModel.html).
- Widgets do not observe domain objects directly, instead they observe the application model.
- Made extensive use of Property Objects to help connect the various layers and to support the fine grained synchronization using observers.
- It wasn't the default behavior for the application model to manipulate widgets, but it was commonly done for complicated cases.

- 在使用[分離表示](https://martinfowler.com/eaaDev/SeparatedPresentation.html)和[觀察者同步](https://martinfowler.com/eaaDev/MediatedSynchronization.html)方面遵循MVC。
- 引入了一個中介的應用程序模型，用作表示邏輯和狀態的容器——這是[表示模型](https://martinfowler.com/eaaDev/PresentationModel.html)的一部分發展。
- 小部件不直接觀察領域對象，而是觀察應用程序模型。
- 廣泛使用屬性對象來幫助連接各個層，並使用觀察者來支持細粒度的同步。
- 應用程序模型並不預設操作小部件，但在複雜情況下經常這樣做。

## Model-View-Presenter (MVP)

MVP is an architecture that first appeared in IBM and more visibly at Taligent during the 1990's. It's most commonly referred via the [Potel](http://www.wildcrest.com/Potel/Portfolio/mvp.pdf) paper. The idea was further popularized and described by the developers of [Dolphin Smalltalk](http://www.object-arts.com/papers/TwistingTheTriad.PDF). As we'll see the two descriptions don't entirely mesh but the basic idea underneath it has become popular.

MVP是一種架構，最早出現在IBM，並在1990年代更明顯地出現在Taligent。它最常見的描述來自[Potel](http://www.wildcrest.com/Potel/Portfolio/mvp.pdf)的論文。這個想法進一步被[Dolphin Smalltalk](http://www.object-arts.com/papers/TwistingTheTriad.PDF)的開發者推廣並描述。正如我們所看到的，這兩種描述並不完全契合，但其基本想法已經變得流行。

To approach MVP I find it helpful to think about a significant mismatch between two strands of UI thinking. On the one hand is the Forms and Controller architecture which was the mainstream approach to UI design, on the other is MVC and its derivatives. The Forms and Controls model provides a design that is easy to understand and makes a good separation between reusable widgets and application specific code. What it lacks, and MVC has so strongly, is [Separated Presentation](https://martinfowler.com/eaaDev/SeparatedPresentation.html) and indeed the context of programming using a [Domain Model](https://martinfowler.com/eaaCatalog/domainModel.html). I see MVP as a step towards uniting these streams, trying to take the best from each.

要理解MVP，我發現思考UI思想中兩條線的重大不匹配很有幫助。一方面是Forms和Controller架構，它是UI設計的主流方法；另一方面是MVC及其衍生物。Forms和Controls模型提供了一種易於理解的設計，並很好地區分了可重用的小部件和應用程序特定的代碼。它所缺少的是[分離表示](https://martinfowler.com/eaaDev/SeparatedPresentation.html)和使用[領域模型](https://martinfowler.com/eaaCatalog/domainModel.html)編程的上下文，而MVC則強調了這一點。我認為MVP是將這兩個流派融合的一步，嘗試從每個流派中吸取精華。

The first element of [Potel](http://www.wildcrest.com/Potel/Portfolio/mvp.pdf) is to treat the view as a structure of widgets, widgets that correspond to the controls of the Forms and Controls model and remove any view/controller separation. The view of MVP is a structure of these widgets. It doesn't contain any behavior that describes how the widgets react to user interaction.

[Potel](http://www.wildcrest.com/Potel/Portfolio/mvp.pdf)的第一個要素是將視圖視為一組小部件，這些小部件對應於Forms和Controls模型中的控件，並移除了視圖/控制器的分離。MVP的視圖是一組小部件結構。它不包含描述小部件如何對用戶交互作出反應的行為。

The active reaction to user acts lives in a separate presenter object. The fundamental handlers for user gestures still exist in the widgets, but these handlers merely pass control to the presenter.

用戶行為的實際反應存在於一個獨立的呈現者對象中。用戶手勢的基本處理程序仍然存在於小部件中，但這些處理程序僅將控制權傳遞給呈現者。

The presenter then decides how to react to the event. [Potel](http://www.wildcrest.com/Potel/Portfolio/mvp.pdf) discusses this interaction primarily in terms of actions on the model, which it does by a system of commands and selections. A useful thing to highlight here is the approach of packaging all the edits to the model in a command - this provides a good foundation for providing undo/redo behavior.

然後，呈現者決定如何對事件作出反應。[Potel](http://www.wildcrest.com/Potel/Portfolio/mvp.pdf)主要通過命令和選擇系統來討論這一交互，這些系統作用於模型。值得注意的是，所有對模型的編輯都包裝在一個命令中——這為提供撤銷/重做行為奠定了良好的基礎。

As the Presenter updates the model, the view is updated through the same [Observer Synchronization](https://martinfowler.com/eaaDev/MediatedSynchronization.html) approach that MVC uses.

當呈現者更新模型時，視圖會通過與MVC相同的[觀察者同步](https://martinfowler.com/eaaDev/MediatedSynchronization.html)方法進行更新。

The [Dolphin](http://www.object-arts.com/papers/TwistingTheTriad.PDF) description is similar. Again the main similarity is the presence of the presenter. In the Dolphin description there isn't the structure of the presenter acting on the model through commands and selections. There is also explicit discussion of the presenter manipulating the view directly. Potel doesn't talk about whether presenters should do this or not, but for Dolphin this ability was essential to overcoming the kind of flaw in Application Model that made it awkward for me to color the text in the variation field.

[Dolphin](http://www.object-arts.com/papers/TwistingTheTriad.PDF)的描述類似。主要相似之處是有呈現者。在Dolphin的描述中，沒有通過命令和選擇作用於模型的呈現者結構。還明確討論了呈現者直接操作視圖的情況。Potel並沒有討論呈現者是否應該這麼做，但對於Dolphin來說，這種能力對於克服應用程序模型中的缺陷（使我無法在變異字段中上色）至關重要。

One of the variations in thinking about MVP is the degree to which the presenter controls the widgets in the view. On one hand there is the case where all view logic is left in the view and the presenter doesn't get involved in deciding how to render the model. This style is the one implied by [Potel](http://www.wildcrest.com/Potel/Portfolio/mvp.pdf). The direction behind [Bower and McGlashan](http://www.object-arts.com/papers/TwistingTheTriad.PDF) was what I'm calling [Supervising Controller](https://martinfowler.com/eaaDev/SupervisingPresenter.html), where the view handles a good deal of the view logic that can be described declaratively and the presenter then comes in to handle more complex cases.

關於MVP的思考存在一些變化，其中之一是呈現者控制視圖中小部件的程度。一方面，所有視圖邏輯留在視圖中，呈現者不參與決定如何呈現模型。這種風格是由[Potel](http://www.wildcrest.com/Potel/Portfolio/mvp.pdf)暗示的。由[Bower和McGlashan](http://www.object-arts.com/papers/TwistingTheTriad.PDF)提出的方向是我所稱的[監控控制器](https://martinfowler.com/eaaDev/SupervisingPresenter.html)，即視圖處理可以聲明性描述的大部分視圖邏輯，然後呈現者進來處理更複雜的情況。

You can also move all the way to having the presenter do all the manipulation of the widgets. This style, which I call [Passive View](https://martinfowler.com/eaaDev/PassiveScreen.html) isn't part of the original descriptions of MVP but got developed as people explored testability issues. I'm going to talk about that style later, but that style is one of the flavors of MVP.

你還可以將呈現者完全控制所有小部件操作。這種風格，我稱之為[被動視圖](https://martinfowler.com/eaaDev/PassiveScreen.html)，並不是MVP原始描述的一部分，但隨著人們探索可測試性問題而發展出來。我稍後會談到這種風格，但它是MVP的一種變體。

Before I contrast MVP with what I've discussed before I should mention that both MVP papers here do this too - but not quite with the same interpretation I have. Potel implies that MVC controllers were overall coordinators - which isn't how I see them. Dolphin talks a lot about issues in MVC, but by MVC they mean the VisualWorks Application Model design rather than classic MVC that I've described (I don't blame them for that - trying to get information on classic MVC isn't easy now let alone then.)

在我將MVP與我之前討論的內容進行對比之前，我應該提到，這兩篇MVP的論文也有這樣的做法——但與我理解的略有不同。Potel暗示MVC控制器是整體協調器——這不是我所看到的情況。Dolphin談到了很多MVC中的問題，但他們所指的MVC是VisualWorks應用程序模型設計，而不是我所描述的經典MVC（我不怪他們，因為現在很難獲得經典MVC的信息，更不用說當時了）。

So now it's time for some contrasts:

現在是時候來做一些對比了：

- Forms and Controls: MVP has a model and the presenter is expected to manipulate this model with [Observer Synchronization](https://martinfowler.com/eaaDev/MediatedSynchronization.html) then updating the view. Although direct access to the widgets is allowed, this should be in addition to using the model not the first choice.
- MVC: MVP uses a [Supervising Controller](https://martinfowler.com/eaaDev/SupervisingPresenter.html) to manipulate the model. Widgets hand off user gestures to the [Supervising Controller](https://martinfowler.com/eaaDev/SupervisingPresenter.html). Widgets aren't separated into views and controllers. You can think of presenters as being like controllers but without the initial handling of the user gesture. However it's also important to note that presenters are typically at the form level, rather than the widget level - this is perhaps an even bigger difference.
- Application Model: Views hand off events to the presenter as they do to the application model. However the view may update itself directly from the domain model, the presenter doesn't act as a [Presentation Model](https://martinfowler.com/eaaDev/PresentationModel.html). Furthermore the presenter is welcome to directly access widgets for behaviors that don't fit into the [Observer Synchronization](https://martinfowler.com/eaaDev/MediatedSynchronization.html).

- Forms 和 Controls: 在 MVP 中，模型存在，且預期由呈現者使用[觀察者同步](https://martinfowler.com/eaaDev/MediatedSynchronization.html)來操作該模型，然後更新視圖。雖然可以直接訪問小部件，但這應該是在使用模型的基礎上，而不是首選方法。
- MVC: 在 MVP 中，使用[監控控制器](https://martinfowler.com/eaaDev/SupervisingPresenter.html)來操作模型。小部件將用戶手勢交給[監控控制器](https://martinfowler.com/eaaDev/SupervisingPresenter.html)。小部件不會分為視圖和控制器。你可以把呈現者視為控制器，只是它不負責最初的用戶手勢處理。然而，還要注意的是，呈現者通常處於表單層級，而不是小部件層級——這也許是一個更大的區別。
- 應用程序模型: 視圖將事件交給呈現者，就像交給應用程序模型一樣。然而，視圖可以直接從領域模型更新自己，呈現者並不充當[表示模型](https://martinfowler.com/eaaDev/PresentationModel.html)。此外，呈現者可以直接訪問小部件，處理那些不適合[觀察者同步](https://martinfowler.com/eaaDev/MediatedSynchronization.html)的行為。

There are obvious similarities between MVP presenters and MVC controllers, and presenters are a loose form of MVC controller. As a result a lot of designs will follow the MVP style but use 'controller' as a synonym for presenter. There's a reasonable argument for using controller generally when we are talking about handling user input.

MVP的呈現者和MVC的控制器之間有明顯的相似之處，呈現者是MVC控制器的一種鬆散形式。因此，許多設計會遵循MVP風格，但使用“控制器”作為呈現者的同義詞。在談論處理用戶輸入時，使用“控制器”是有道理的。

![](https://martinfowler.com/eaaDev/uiArchs/mvp-seq.gif)

Let's look at an MVP ([Supervising Controller](https://martinfowler.com/eaaDev/SupervisingPresenter.html)) version of the ice-cream monitor ( [Figure 12](https://martinfowler.com/eaaDev/uiArchs.html#uiArchs_mvp-seq.gif)). It starts much the same as the Forms and Controls version - the actual text field raises an event when its text is changed, the presenter listens to this event and gets the new value of the field. At this point the presenter updates the reading domain object, which the variance field observes and updates its text with. The last part is the setting of the color for the variance field, which is done by the presenter. It gets the category from the reading and then updates the color of the variance field.

讓我們來看看冰淇淋監視器的MVP（[監控控制器](https://martinfowler.com/eaaDev/SupervisingPresenter.html)）版本（[圖12](https://martinfowler.com/eaaDev/uiArchs.html#uiArchs_mvp-seq.gif)）。它的開始方式與Forms和Controls版本非常相似——當文本框中的文本發生變化時，文本框會觸發事件，呈現者會監聽此事件並獲取字段的新值。此時，呈現者會更新讀取領域對象，變異字段觀察並更新其文本。最後一部分是設置變異字段的顏色，這是由呈現者完成的。它從讀取中獲取類別，然後更新變異字段的顏色。

Here are the MVP soundbites:

以下是MVP的總結：

- User gestures are handed off by the widgets to a [Supervising Controller](https://martinfowler.com/eaaDev/SupervisingPresenter.html).
- The presenter coordinates changes in a domain model.
- Different variants of MVP handle view updates differently. These vary from using [Observer Synchronization](https://martinfowler.com/eaaDev/MediatedSynchronization.html) to having the presenter doing all the updates with a lot of ground in-between.

- 用戶手勢由小部件交給[監控控制器](https://martinfowler.com/eaaDev/SupervisingPresenter.html)。
- 呈現者協調領域模型的變更。
- 不同變體的MVP以不同方式處理視圖更新。這些方式從使用[觀察者同步](https://martinfowler.com/eaaDev/MediatedSynchronization.html)到讓呈現者處理所有更新，其中間有很多不同。

## Humble View

In the past few years there's been a strong fashion for writing self-testing code. Despite being the last person to ask about fashion sense, this is a movement that I'm thoroughly immersed in. Many of my colleagues are big fans of xUnit frameworks, automated regression tests, Test-Driven Development, Continuous Integration and similar buzzwords.

在過去幾年中，寫自測試代碼成為了一個強烈的潮流。儘管我並不是最適合談論時尚的人，但這是我完全沉浸其中的一個運動。我的許多同事是xUnit框架、自动化回归测试、測試驅動開發、持續集成等術語的忠實粉絲。

When people talk about self-testing code user-interfaces quickly raise their head as a problem. Many people find that testing GUIs to be somewhere between tough and impossible. This is largely because UIs are tightly coupled into the overall UI environment and difficult to tease apart and test in pieces.

當人們談論自測試代碼時，用户界面（GUI）很快會成為一個問題。許多人發現測試GUI有時既困難又不可能。這主要是因為UI與整體UI環境緊密耦合，很難拆解並單獨測試。

Sometimes this test difficulty is over-stated. You can often get surprisingly far by creating widgets and manipulating them in test code. But there are occasions where this is impossible, you miss important interactions, there are threading issues, and the tests are too slow to run.

有時這種測試困難被過度誇大了。你經常可以通過創建小部件並在測試代碼中操作它們來取得意外的進展。但也有一些情況是這不可能的，你會錯過重要的交互、出現線程問題，並且測試運行速度過慢。

As a result there's been a steady movement to design UIs in such a way that minimizes the behavior in objects that are awkward to test. Michael Feathers crisply summed up this approach in [The Humble Dialog Box](http://www.objectmentor.com/resources/articles/TheHumbleDialogBox.pdf). [Gerard Meszaros](http://xunitpatterns.com/) generalized this notion to idea of a **Humble Object** - any object that is difficult to test should have minimal behavior. That way if we are unable to include it in our test suites we minimize the chances of an undetected failure.

因此，一直有一個穩定的運動，旨在以最小化難以測試的對象行為的方式來設計UI。Michael Feathers在[謙虛對話框](http://www.objectmentor.com/resources/articles/TheHumbleDialogBox.pdf)中簡潔地總結了這一方法。[Gerard Meszaros](http://xunitpatterns.com/)將這一概念概括為**謙虛對象**——任何難以測試的對象應該有最小的行為。這樣，即使我們無法將其包含在測試套件中，我們也可以最大程度地減少未檢測到故障的機會。

[The Humble Dialog Box](http://www.objectmentor.com/resources/articles/TheHumbleDialogBox.pdf) paper uses a presenter, but in a much deeper way than the original MVP. Not just does the presenter decide how to react to user events, it also handles the population of data in the UI widgets themselves. As a result the widgets no longer have, nor need, visibility to the model; they form a [Passive View](https://martinfowler.com/eaaDev/PassiveScreen.html), manipulated by the presenter.

[謙虛對話框](http://www.objectmentor.com/resources/articles/TheHumbleDialogBox.pdf)這篇文章使用了呈現者，但它比最初的MVP更深入。呈現者不僅決定如何對用戶事件做出反應，還處理UI小部件本身的數據填充。因此，小部件不再需要或有必要了解模型；它們形成了一個[被動視圖](https://martinfowler.com/eaaDev/PassiveScreen.html)，由呈現者操作。

This isn't the only way to make the UI humble. Another approach is to use [Presentation Model](https://martinfowler.com/eaaDev/PresentationModel.html), although then you do need a bit more behavior in the widgets, enough for the widgets to know how to map themselves to the [Presentation Model](https://martinfowler.com/eaaDev/PresentationModel.html).

這並不是使UI謙虛的唯一方法。另一種方法是使用[表示模型](https://martinfowler.com/eaaDev/PresentationModel.html)，但這樣你確實需要在小部件中有更多的行為，足夠讓小部件知道如何將自己映射到[表示模型](https://martinfowler.com/eaaDev/PresentationModel.html)。

The key to both approaches is that by testing the presenter or by testing the presentation model, you test most of the risk of the UI without having to touch the hard-to-test widgets.

這兩種方法的關鍵是，通過測試呈現者或測試表示模型，你可以在不接觸難以測試的小部件的情況下測試UI的主要風險。

With [Presentation Model](https://martinfowler.com/eaaDev/PresentationModel.html) you do this by having all the actual decision making made by the [Presentation Model](https://martinfowler.com/eaaDev/PresentationModel.html). All user events and display logic is routed to the [Presentation Model](https://martinfowler.com/eaaDev/PresentationModel.html), so that all the widgets have to do is map themselves to properties of the [Presentation Model](https://martinfowler.com/eaaDev/PresentationModel.html). You can then test most of the behavior of the [Presentation Model](https://martinfowler.com/eaaDev/PresentationModel.html) without any widgets being present - the only remaining risk lies in the widget mapping. Provided that this is simple you can live with not testing it. In this case the screen isn't quite as humble as with the [Passive View](https://martinfowler.com/eaaDev/PassiveScreen.html) approach, but the difference is small.

使用[表示模型](https://martinfowler.com/eaaDev/PresentationModel.html)，你通過讓所有的決策都由[表示模型](https://martinfowler.com/eaaDev/PresentationModel.html)來做來實現這一點。所有用戶事件和顯示邏輯都會路由到[表示模型](https://martinfowler.com/eaaDev/PresentationModel.html)，所以小部件只需將自己映射到[表示模型](https://martinfowler.com/eaaDev/PresentationModel.html)的屬性。這樣，你可以在沒有任何小部件的情況下測試[表示模型](https://martinfowler.com/eaaDev/PresentationModel.html)的大部分行為——唯一剩下的風險在於小部件映射。只要這部分簡單，你就可以接受不測試它。在這種情況下，與[被動視圖](https://martinfowler.com/eaaDev/PassiveScreen.html)方法相比，屏幕並不像那麼謙虛，但差別很小。

Since [Passive View](https://martinfowler.com/eaaDev/PassiveScreen.html) makes the widgets entirely humble, without even a mapping present, [Passive View](https://martinfowler.com/eaaDev/PassiveScreen.html) eliminates even the small risk present with [Presentation Model](https://martinfowler.com/eaaDev/PresentationModel.html). The cost however is that you need a [Test Double](http://xunitpatterns.com/Test%20Double.html) to mimic the screen during your test runs - which is extra machinery you need to build.

由於[被動視圖](https://martinfowler.com/eaaDev/PassiveScreen.html)使小部件完全謙虛，甚至沒有映射，[被動視圖](https://martinfowler.com/eaaDev/PassiveScreen.html)消除了[表示模型](https://martinfowler.com/eaaDev/PresentationModel.html)中存在的微小風險。然而，代價是你需要一個[Test Double](http://xunitpatterns.com/Test%20Double.html)來模擬測試過程中的屏幕——這需要額外構建一些機制。

A similar trade-off exists with [Supervising Controller](https://martinfowler.com/eaaDev/SupervisingPresenter.html). Having the view do simple mappings introduces some risk but with the benefit (as with [Presentation Model](https://martinfowler.com/eaaDev/PresentationModel.html)) of being able to specify simple mapping declaratively. Mappings will tend to be smaller for [Supervising Controller](https://martinfowler.com/eaaDev/SupervisingPresenter.html) than for [Presentation Model](https://martinfowler.com/eaaDev/PresentationModel.html) as even complex updates will be determined by the [Presentation Model](https://martinfowler.com/eaaDev/PresentationModel.html) and mapped, while a [Supervising Controller](https://martinfowler.com/eaaDev/SupervisingPresenter.html) will manipulate the widgets for complex cases without any mapping involved.

[監控控制器](https://martinfowler.com/eaaDev/SupervisingPresenter.html)也有類似的權衡。讓視圖做簡單映射會引入一些風險，但好處是（像[表示模型](https://martinfowler.com/eaaDev/PresentationModel.html)一樣）可以聲明性地指定簡單映射。對於[監控控制器](https://martinfowler.com/eaaDev/SupervisingPresenter.html)來說，映射通常會比[表示模型](https://martinfowler.com/eaaDev/PresentationModel.html)小，因為即使是複雜的更新也會由[表示模型](https://martinfowler.com/eaaDev/PresentationModel.html)來確定並映射，而[監控控制器](https://martinfowler.com/eaaDev/SupervisingPresenter.html)則會直接操作小部件來處理複雜情況，無需映射。