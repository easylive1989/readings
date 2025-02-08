---
date: '2025-02-08T09:25:28+08:00'
title: 'Tell Dont Ask'
tags: ["Martin Fowler"]
---

[Source](https://martinfowler.com/bliki/TellDontAsk.html)

Tell-Don't-Ask is a principle that helps people remember that object-orientation is about bundling data with the functions that operate on that data. It reminds us that rather than asking an object for data and acting on that data, we should instead tell an object what to do. This encourages to move behavior into an object to go with the data.

Tell-Don't-Ask 是一個原則，幫助人們記住物件導向是關於將資料與操作該資料的函式捆綁在一起。它提醒我們，與其詢問物件的資料並對該資料進行操作，不如告訴物件要做什麼。這鼓勵將行為移動到與資料相關的物件中。


![](https://martinfowler.com/bliki/images/tellDontAsk/sketch.png)

Let's clarify with an example. Let's imagine we need to monitor certain values, signaling an alarm should the value rise above a certain limit. If we write this in an “ask” style, we might have a data structure to represent these things…

讓我們用一個例子來闡明。假設我們需要監控某些值，當值超過特定限制時發出警報。如果我們用"ask"（詢問）風格編寫程式，我們可能會有一個表示這些事物的資料結構…

```java
class AskMonitor {

  private int value;
  private int limit;
  private boolean isTooHigh;
  private String name;
  private Alarm alarm;

  public AskMonitor (String name, int limit, Alarm alarm) {
    this.name = name;
    this.limit = limit;
    this.alarm = alarm;
  }
}
```

…and combine this with some accessors to get at this data

…並結合一些存取器來獲取這些資料

```java
class AskMonitor {

  public int getValue() {return value;}
  public void setValue(int arg) {value = arg;}
  public int getLimit() {return limit;}
  public String getName()  {return name;}
  public Alarm getAlarm() {return alarm;}
}
```

We would then use the data structure like this

我們會這樣使用這個資料結構

```java
AskMonitor am = new AskMonitor("Time Vortex Hocus", 2, alarm);
am.setValue(3);
if (am.getValue() > am.getLimit()) 
  am.getAlarm().warn(am.getName() + " too high");
```


“Tell Don't Ask” reminds us to instead put the behavior inside the monitor object itself (using the same fields).

"Tell Don't Ask"提醒我們應該將行為放在監控物件本身內部（使用相同的欄位）。

```dart
class TellMonitor {

  public void setValue(int arg) {
    value = arg;
    if (value > limit) alarm.warn(name + " too high");
  }
}
```

Which would be used like this

這將被如此使用

```java
TellMonitor tm = new TellMonitor("Time Vortex Hocus", 2, alarm);
tm.setValue(3);
```


Many people find tell-don't-ask to be a useful principle. One of the fundamental principles of object-oriented design is to combine data and behavior, so that the basic elements of our system (objects) combine both together. This is often a good thing because this data and the behavior that manipulates them are tightly coupled: changes in one cause changes in the other, understanding one helps you understand the other. Things that are tightly coupled should be in the same component. Thinking of tell-don't-ask is a way to help programmers to see how they can increase this co-location.

許多人認為 Tell-Don't-Ask 是一個有用的原則。物件導向設計的基本原則之一是將資料和行為結合起來，使系統的基本元素（物件）同時包含這兩者。這通常是一件好事，因為這些資料及其操作資料的行為是緊密耦合的：一方的變化會導致另一方的變化，理解一方有助於理解另一方。緊密耦合的事物應該位於同一個元件中。思考 Tell-Don't-Ask 是幫助程式設計師看到如何增加這種共同定位的一種方式。

But personally, I don't use tell-dont-ask. I do look to co-locate data and behavior, which often leads to similar results. One thing I find troubling about tell-dont-ask is that I've seen it encourage people to become [GetterEradicators](https://martinfowler.com/bliki/GetterEradicator.html), seeking to get rid of all query methods. But there are times when objects collaborate effectively by providing information. A good example are objects that take input information and transform it to simplify their clients, such as using [EmbeddedDocument](https://martinfowler.com/bliki/EmbeddedDocument.html). I've seen code get into convolutions of only telling where suitably responsible query methods would simplify matters 1. For me, tell-don't-ask is a stepping stone towards co-locating behavior and data, but I don't find it a point worth highlighting.

但就個人而言，我並不使用 Tell-Don't-Ask。我確實會尋求將資料和行為共同定位，這往往會導致類似的結果。我對 Tell-Don't-Ask 感到困擾的一件事是，我看到它鼓勵人們成為 [GetterEradicators](https://martinfowler.com/bliki/GetterEradicator.html)，試圖消除所有查詢方法。但有些時候，物件通過提供資訊來有效地協作。一個很好的例子是那些接收輸入資訊並將其轉換以簡化其客戶端的物件，比如使用 [EmbeddedDocument](https://martinfowler.com/bliki/EmbeddedDocument.html)。我見過一些程式碼為了只"告訴"而變得彎彎繞繞，而適當的查詢方法本可以簡化事情。對我來說，Tell-Don't-Ask 只是朝著共同定位行為和資料邁出的一步，但不值得特別強調。

## Further Reading

This principle is most often associated with Andy Hunt and “Prag” Dave Thomas (The Pragmatic Programmers). They describe it in a [IEEE Software column](http://media.pragprog.com/articles/jan_03_enbug.pdf) and a [post on their website](http://pragprog.com/articles/tell-dont-ask).
