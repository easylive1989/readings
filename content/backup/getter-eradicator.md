---
date: '2025-02-28T02:15:31+08:00'
title: 'Getter Eradicator'
tags: ["Martin Fowler"]
---

[Source](https://martinfowler.com/bliki/GetterEradicator.html)

You can tell them by the twitch in the left hand side of the mouth when they see a getter method, there's swift pull on their battleaxe and a satisfied cry as another getter is hewn unmercifully from a class which immediately swoons in an ecstasy of gratefulness at the manly Getter Eradicator's feet.

你可以透過他們看到 getter 方法時，左嘴角微微抽動來辨識他們。接著他們會迅速拔出戰斧，並隨著又一個 getter 無情地從類別中被砍除時，發出滿足的呼喊。而這個類別會立即陶醉在感激之情中，拜倒在這位充滿男子氣概的 Getter 消滅者的腳下。

Alright, maybe my return to English beer is affecting me a bit too much, but Chris's [gentle tweak](http://chrisstevenson.me/development/2006/01/10/assertions-on-domain-objects.html) struck a pet peevlet of mine. I've often come across people who tell you to avoid having getting methods on classes, treating such things as a violation of encapsulation, [Allen Holub's article](https://www.infoworld.com/article/2073723/why-getter-and-setter-methods-are-evil.html) is one of the best known.

好吧，可能是我回歸英式啤酒後影響有點太大了，但 Chris 的 [溫和調整](https://www.google.com/url?sa=E&q=http%3A%2F%2Fchrisstevenson.me%2Fdevelopment%2F2006%2F01%2F10%2Fassertions-on-domain-objects.html) 觸發了我一個小小的厭惡感。我經常遇到有人告訴你應該避免在類別中使用 getter 方法，他們認為這種行為違反了封裝，[Allen Holub 的文章](https://www.google.com/url?sa=E&q=https%3A%2F%2Fwww.infoworld.com%2Farticle%2F2073723%2Fwhy-getter-and-setter-methods-are-evil.html) 就是其中最為人所知的例子之一。

The general justification is that getters violate encapsulation. If I've got a bowler class with fields for overs, runs and wickets, then adding getters (getOvers, getRuns, getWickets) is little better than just making the fields public.

普遍的理由是 getter 違反了封裝。如果我有一個保齡球手類別，其中包含局數、分數和三柱門的欄位，那麼添加 getter（getOvers、getRuns、getWickets）實際上與直接將欄位設為 public 沒有太大區別。

There's some sense to this argument, and I certainly suggest that you shouldn't write accessors until you really need them, but it also brings in the danger of missing the point of encapsulation. For me, the point of encapsulation isn't really about hiding the data, but in hiding design decisions, particularly in areas where those decisions may have to change. The internal data representation is one example of this, but not the only one and not always the best one. The protocol used to communicate with an external data store is a good example of encapsulation - one that's more about the messages to that store than it is about any data representation. When you are thinking about encapsulation I think it's better to ask yourself “what piece of variability are you hiding and why” rather than “am I exposing data”. (Craig Larman wrote a [nice column](https://martinfowler.com/ieeeSoftware/protectedVariation.pdf) for me on this.)

這個論點有些道理，我當然建議你不要在真正需要之前編寫訪問器，但它也帶來了忽略封裝重點的危險。對我來說，封裝的重點並不在於隱藏資料，而是隱藏設計決策，尤其是在那些決策可能必須改變的領域。內部資料表示就是一個例子，但不是唯一的例子，也並非總是最好的例子。用於與外部資料儲存溝通的協定是封裝的一個好例子——它更多地與發送到該儲存的消息有關，而不是與任何資料表示有關。當你思考封裝時，我認為最好問自己「你隱藏了什麼樣的可變性以及為什麼」，而不是「我是否暴露了資料」。 (Craig Larman 為我寫了一篇 [不錯的專欄文章](https://www.google.com/url?sa=E&q=https%3A%2F%2Fmartinfowler.com%2FieeeSoftware%2FprotectedVariation.pdf) 關於這個。)

Although the defense of encapsulation is the common rallying cry for getter eradicators, I think the real motivation for them is rather more reasonable and pragmatic. There's a hell of a lot of code out there in OO languages that is procedural in design. The OO community may have 'won' in the sense that modern languages are dominated by objects, but they are still yet to win in that OO programming is still not widely used. As a result it's still common to see procedures that pull data out of an object to do something, when that behavior would better fit in the object itself - a violation of the pragmatic programmers principle of “[Tell Don't Ask](http://www.pragmaticprogrammer.com/ppllc/papers/1998_05.html)“. You can only do this kind of procedural programming if you have getters, so telling people to get rid of getters helps push them to move behavior into the right place.

雖然對封裝的捍衛是 getter 消滅者常用的口號，但我認為他們真正的動機更為合理和務實。在物件導向語言中，存在著大量程式碼在設計上是程序式的。物件導向社群可能已經「贏了」，因為現代語言以物件為主，但他們尚未贏得物件導向程式設計尚未被廣泛使用的事實。因此，仍然常見的是看到程序從物件中提取資料來執行某些操作，而這種行為更適合放在物件本身中——這違反了務實程式設計師的「[Tell Don't Ask](https://www.google.com/url?sa=E&q=http%3A%2F%2Fwww.pragmaticprogrammer.com%2Fppllc%2Fpapers%2F1998_05.html)「 原則。只有在擁有 getter 的情況下才能進行這種程序式程式設計，因此告訴人們擺脫 getter 有助於推動他們將行為移動到正確的位置。

I have a lot of sympathy with this motivation, but I fear that just telling people to avoid getters is a rather blunt tool. There are too many times when objects do need to collaborate by exchanging data, which leads to genuine needs for getters.

我非常同情這種動機，但我擔心僅僅告訴人們避免使用 getter 是一種相當笨拙的工具。太多時候物件需要透過交換資料來協作，這導致對 getter 的真正需求。

If we're looking for a simple rule of thumb, the one I prefer is one that I first heard from Kent Beck, which is to always beware of cases where some code invokes more than one method on the same object. This occurs with accessors and more reasonable commands. If you ask an object for two bits of data, can you replace this with a single request for the bit of data you're calculating? If you tell an object to do two things, can you replace them with a single command? Of course there are plenty of cases where you can't, but it's always worth asking yourself the question.

如果我們正在尋找一個簡單的經驗法則，我更喜歡我第一次從 Kent Beck 聽到的那種，即始終警惕某些程式碼在同一個物件上調用多個方法的情況。這發生在存取器和更合理的命令中。如果你向物件請求兩個資料片段，你是否可以用對你正在計算的資料片段的單個請求替換它？如果你告訴物件做兩件事，你可以用一個命令來替換它們嗎？當然，在很多情況下你不能，但總是值得問自己這個問題。

Another good warning sign of trouble is the Data Class - a class that has only fields and accessors. That's almost always a sign of trouble because it's devoid of behavior. If you see one of those you should always be suspicious. Look for who uses the data and try to see if some of this behavior can be moved into the object. In these cases it can be useful to ask yourself 'can I get rid of this getter?' Even if you can't, asking the question may lead to some good movements of behavior.

另一個麻煩的好徵兆是資料類別——一個只有欄位和存取器的類別。這幾乎總是一個麻煩的跡象，因為它缺乏行為。如果你看到其中一個，你應該始終持懷疑態度。尋找誰使用這些資料，並嘗試看看是否可以將一些這種行為移動到物件中。在這些情況下，問自己「我可以擺脫這個 getter 嗎？」可能會很有用。即使你不能，提出這個問題可能會導致一些良好的行為移動。

Allocation of behavior between objects is the essence of object-oriented design, so like any design, there isn't a hard and fast rule - rather a judging of trade-offs. Putting the behavior in the same class as the data, what Craig Larman calls “Information Expert”, is the first choice to consider. But it isn't the only route. Layering often trumps this, many of the Gang of Four patterns separate data from behavior for particular needs. A good rule of thumb is that things that change together should be together. Data and the behavior that uses it often change together, but often you see other groupings that matter more.

行為在物件之間的分配是物件導向設計的本質，因此與任何設計一樣，沒有硬性規定——而是對權衡的判斷。將行為放在與資料相同的類別中，Craig Larman 稱之為「資訊專家」，是首先要考慮的選擇。但這不是唯一的途徑。分層經常勝過這一點，許多四人幫模式將資料與行為分開以滿足特定需求。一個好的經驗法則是，一起變化的東西應該在一起。資料和使用它的行為通常會一起變化，但你經常會看到其他更重要的分組。