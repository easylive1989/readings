---
date: '2025-04-05T23:08:11+08:00'
title: 'Oh my bad boolean'
tags: ["Terry Yin"]
---

## [Why are boolean parameters bad?](https://www.linkedin.com/posts/terryyin_coding-softwaredevelopment-boolean-activity-7058616373849034752-_wMc/)

Hello, welcome to the OhMyBat Boolean series. 

In my game program, there is a powerful tool called a blaster. 
在我的遊戲程式中，有一個強大的工具叫做爆破槍。

```java
class Blaster {
	action(isLoad) {
		if(isLoad) {
			this.load();
		}
		this.fire();
	}

	fire() {
		// ...
	}

	private load() {
		// ...
	}
}
```

You can fire the blaster whenever you want. 
您可以隨時使用爆破槍開火。

But if you load it first, you can fire more powerful shots. 
但如果您先為其加載，則可以發射更強大的射擊。

The function action is called from many places in the code.
這個動作函數在程式中被多個地方呼叫。

```java
blaster.action(true); // Caller 1
blaster.action(false); // Caller 2
blaster.action(true); // Caller 3
```

Using a Boolean parameter reduces the number of API calls users have to remember. 
使用布林參數可以減少用戶需要記住的 API 呼叫數量。

So what's the problem with using a Boolean parameter? 
那麼使用布林參數的問題是什麼？

As we can see, every caller knows exactly what they want. 
正如我們所見，每個呼叫者都確切知道自己想要什麼。

It means the condition each load in the action function is redundant. 
這意味著每次在動作函數中的加載條件都是多餘的。

I've been inventing countering logics in the callees and the callers to keep myself busy. 
我不斷在被呼叫者和呼叫者之間發明對抗邏輯，讓自己忙碌起來。

Say now caller 1 is proposing an interface change, so they want to pass the load amount to action. 
假設現在呼叫者 1 提議更改介面，他們希望將加載量傳遞給動作函數。

```java
blaster.action(1000); // Caller 1
```

So we can change the API to take the parameter load amount. 
因此，我們可以更改 API，使其接受加載量作為參數。

```java
class Blaster {
	action(loadAcmount) {
		if(loadAcmount > 0) {
			this.load(loadAcmount);
		}
		this.fire();
	}

	fire() {
		// ...
	}

	private load(loadAcmount) {
		// ...
	}
}
```

Caller 3 is happy with the new API, this is exactly what they want. 
呼叫者 3 對新的 API 感到滿意，這正是他們想要的。

```java
blaster.action(999999); // Caller 3: Woohoo! Just what I needed
```

But caller 2 is confused about why they are involved in this change. 
但呼叫者 2 對於為什麼要捲入這次變更感到困惑。

```java
blaster.action(false); // Caller 2: What?! Why change it? What amount?
```

The Boolean parameter makes the callers and callees tightly coupled. 
布林參數使得呼叫者與被呼叫者之間緊密耦合。

This tight coupling introduces unnecessary complexity, makes the changes harder, and complicates collaboration. 
這種緊密耦合引入了不必要的複雜性，增加了變更的難度，並使合作變得更為複雜。

Just let callers call the method they want. 
只需讓呼叫者呼叫他們想要的方法即可。

```java
blaster.load(1000); // Caller 1
blaster.fire();

blaster.fire(); // Caller 2

blaster.load(999999); // Caller 3
blaster.fire();
```

This reduces complexity and dependencies, resulting in lower coupling. 
這可以降低複雜性和依賴性，從而降低耦合度。

However, there's a pitfall that might bring more significant problems than maintaining tight coupling. 
然而，這裡存在一個陷阱，可能帶來比維持緊密耦合更嚴重的問題。

After load, the blaster must be fired. 
在加載後，爆破槍必須開火。

```java
blaster.load(999999); // Caller 3
blaster.survey(); // error: should call fire immediately
```

Otherwise, it will explode. Well, no worries, our current code is doing exactly that. 
否則，它會爆炸。別擔心，我們目前的程式碼正是這樣做的。

High cohesion means that things belong together are together. 
高內聚意味著應該在一起的東西是放在一起的。

My code doesn't seem to have that, and it makes the cohesion low. 
但我的程式碼似乎並沒有做到這一點，這使得內聚性很低。

Say caller 1 found a bug caused by the now more powerful loaded fire. 
假設呼叫者 1 發現了一個由於更強大的加載射擊引起的錯誤。

After loading, the blaster should be re-aimed before fire, so as not to hurt teammates. 
在加載後，爆破槍應該重新瞄準後再開火，以免傷到隊友。

Caller 1, a fixed bug. 
呼叫者 1 修復了這個錯誤。

```java
blaster.load(1000); // Caller 1
blaster.reAim();
blaster.fire();
```

However, caller 3 is still buggy, but the code started to adverse. 
然而，呼叫者 3 仍然存在錯誤，並且程式碼開始變得不穩定。

Again, it shows my code is not cohesive.
再次說明，我的程式碼缺乏內聚性。

Due to the lack of cohesion, my code lost important domain concepts, and the future changes will become error-proven. 
由於內聚性的缺乏，我的程式碼失去了重要的領域概念，未來的變更將更容易出錯。

The bug would have been avoided if I kept my code more cohesive. 
如果我讓程式碼更加內聚，這個錯誤本可以避免。

```java
class Blaster {
	fire() {
		// ...
	}

	loadedFire(loadAmount) {
		this.load(loadAmount);
		this.reAim();
		this.fire();
	}

	private load(loadAcmount) {
		// ...
	}
}
```

```java
blaster.loadedFire(1000); // Caller 1
blaster.fire(); // Caller 2
blaster.loadedFire(999999); // Caller 3
```

In conclusion, avoid boolean parameters, use meaningful names, and aim for high cohesion and loose coupling. 
總結：避免使用布林參數，使用有意義的命名，並追求高內聚和低耦合。

Thanks for watching until the end. Have a nice day.
感謝觀看到最後，祝您有美好的一天。

## [Why Are My Boolean Return Values Sometimes Bad?](https://www.linkedin.com/posts/terryyin_coding-softwaredevelopment-booleans-activity-7062618505778495488-QzTk/)

Welcome back to the Oh My Bad Boolean series. 

Today's question, why are my Boolean return values sometimes bad? 
今天的問題是：為什麼我的布林返回值有時會帶來壞處？

And how can we make our code reveal my style? 
以及我們該如何讓程式碼更好地表達我的風格？

Sorry, not my style. Should be its true intention. 
抱歉，不是我的風格，而是應該表達它的真正意圖。

Remember the blaster in my game program? 
還記得我的遊戲程式裡的爆破槍嗎？

You can pop up and fire, and then quickly hide again like a sneaky ninja. 
您可以彈出來開火，然後像偷偷摸摸的忍者一樣迅速隱藏。

This is called a pop-up attack. 
這被稱為彈出攻擊。

```java
popUpAttack() {
	this.popUp();
	this.blaster.fire();
	this.hide();
}
```

But wait, the blaster's fire function returned a Boolean value. 
但等等，爆破槍的 fire 函數返回了一個布林值。

```java
class Blaster {
	Boolean fire() {
		// ...
	}
}
```

What's up with that? 
這是怎麼回事呢？

Does it mean the shot was successful? It exploded? No ammo left? 
它是否意味著射擊成功？爆炸了？還是子彈用光了？

Talk about a mystery wrapped in an enigma. 
真是個謎團中的謎團。

What is its intention? 
它的意圖是什麼呢？

It's false actions on callers, like writing a log, and distracts from the main logic flow. 
它對呼叫者的行為產生了誤導，比如記錄日誌，從而分散了主要邏輯的注意力。

```java
popUpAttack() {
	this.popUp();
	if (this.blaster.fire()) {
		log("Somebody should look at this.");
	}
	this.hide();
}
```

Potential problems with a Boolean return value. 
布林返回值可能會帶來的潛在問題包括：

The meaning might not be very clear, and it forces the caller to do something and disrupt the main logic flow. 
其含義可能不夠明確，並強迫呼叫者做某些事，從而干擾主要邏輯的流程。

Let's fix this.
我們來修正這個問題吧。

You don't have to return anything if there's no meaningful things to return. 
如果沒有有意義的內容需要返回，您可以不返回任何值。

When failing to fire is a surprise, the function should throw an exception instead. 
如果開火失敗是個意外情況，那麼函數應該拋出例外（Exception）。

```java
class Blaster {
	void fire() {
		// ...
		throw new Error("...")
	}
}
```

Now caller can choose where to handle the exceptional situation. 
這樣，呼叫者可以選擇在哪裡處理這種例外情況。

```java
popUpAttack() {
	this.popUp();
	this.blaster.fire()
	this.hide();
}
```

We should do this more often in test automation. 
在測試自動化中，我們應該更頻繁地採用這種方式。

Some languages, like Go, prefer a more explicit approach for error situations. 
一些語言，比如 Go，偏好更為顯式的錯誤處理方式，但核心理念保持一致：

The idea stays the same. 
但核心理念保持一致：

Separate exceptional situations from the main logic. 
將例外情況與主要邏輯分離。

Avoiding forced action on callers improves code readability. 
避免對呼叫者施加強制行為能提升程式碼的可讀性。

Now let's look at another example with the blaster's safety feature. 
現在我們來看看另一個與爆破槍安全功能相關的例子。

```java
class Blaster {
	void fire() {
		// ...
		throw new Error("...")
	}
	Boolean isSafetyOn() {...}
	void setSafetyOff(fingerprint) {...}
}
```

The blaster has an isSafetyOn function, and you can set the safety on or off as well. 
爆破槍有一個 `isSafetyOn` 函數，您可以將安全功能設置為開啟或關閉。

A soldier needs to check if the safety is on, and if it's so, switch it off before a fire. 
一名士兵需要檢查安全功能是否開啟，如果是的話，在開火前將其關閉。

```java
popUpAttack() {
	if (this.blaster.isSafetyOn()) {
		this.blaster.setSafetyOff(this.fingerprint)
	}
	this.popUp();
	this.blaster.fire()
	this.hide();
}
```

The soldier class likes the feature of their blaster a lot, and is so focused on the blaster's functionality, which makes the blaster code less cohesive and blurs soldier's attention. 
士兵類非常喜歡爆破槍的這項功能，但過度關注於爆破槍的功能性，導致爆破槍的程式碼變得不夠內聚，也模糊了士兵類的核心職責。

This is called a feature envy. 
這被稱為「功能嫉妒」（Feature Envy）。

We could extract part of it to a method of the soldier class first, 
我們可以先將部分邏輯提取到士兵類的方法中，

```java
popUpAttack() {
	this.endsureSafetyOff(this.fingerprint);
	this.popUp();
	this.blaster.fire()
	this.hide();
}

void endsureSafetyoff(fingerprint) {
	if (this.blaster.isSafetyOn()) {
		this.blaster.setSafetyOff(this.fingerprint)
	}
}
```

then move the new method to the blaster class to stop the feature envy. 
然後再將新方法移動到爆破槍類中，以解決功能嫉妒問題。

```java
class Blaster {
	void endsureSafetyoff(fingerprint) {
		if (this.blaster.isSafetyOn()) {
			this.blaster.setSafetyOff(this.fingerprint)
		}
	}
	void fire() {
		// ...
		throw new Error("...")
	}
	Boolean isSafetyOn() {...}
	void setSafetyOff(fingerprint) {...}
}
```

```java
popUpAttack() {
	this.blaster.endsureSafetyOff(this.fingerprint);
	this.popUp();
	this.blaster.fire()
	this.hide();
}
```

But before we finish the moving, you might have noticed that the method just becomes slimmer. 
但在完成移動之前，您可能已經注意到，這些方法變得更簡潔了。

This is because now the operation of the data is closer to the source of the data. 
這是因為現在資料的操作更接近於資料的來源。

This feature envy problem is not unique to Boolean return values. 
功能嫉妒問題並不限於布林返回值，

But I feel a Boolean return value is more likely to leak internal state, lead to lower cohesion. 
但我覺得布林返回值更容易洩漏內部狀態，從而導致更低的內聚性。

In conclusion, Boolean return values are not inherently bad. 
總結一下，布林返回值本身並不是壞事，

But be cautious with the ambiguous ones. 
但需要謹慎處理那些含義模糊的返回值。

Use meaningful return values. 
使用有意義的返回值。

Follow tell, don't ask, and adhere to command query separation. 
遵循「告訴別人，而不是詢問」（Tell, Don't Ask）的原則，並堅持指令-查詢分離（Command Query Separation）。

Thanks for watching. 
感謝您的觀看。

I still have one more Oh My Bat Boolean episode to share. 

And as always, happy coding.

## [Why Do I Have To Reconsider my Boolean Data?](https://www.linkedin.com/posts/terryyin_programming-coding-booleans-activity-7068834832918736897-9Pe-/?utm_source=share&utm_medium=member_android)

Welcome to the third and final part of my Oh My Bat Boolean series. 

After exploring pitfalls with Boolean parameters and the return values,  let's delve into why should we reconsider our Boolean data. 
在探討了 Boolean 參數和回傳值的陷阱之後，讓我們深入探討為什麼我們應該重新考慮我們的 Boolean 資料。

Our first example is on deck. 
我們的第一個範例即將登場。

Picture an infinite two-dimensional grid of square cells. 
想像一個無限的二維方格網格。

Each cell is either dead or alive. 
每個單元格要麼是死的，要麼是活的。

Each cell is surrounded by eight neighbors. 
每個單元格都被八個鄰近的單元格環繞。

A living cell dies if it has only one or no living neighbors, as if in isolation. 
一個活著的單元格如果只有一個或沒有活鄰居，就會死亡，彷彿陷入孤立。

It survives if there are two or three neighbors. 
如果有兩到三個鄰居，它就會存活。

It dies if there are four or more neighbors due to overcrowding. 
如果有四個或更多鄰居，則會因為過度擁擠而死亡。

A dead cell springs to life if exactly three neighbors are alive, as if through reproduction. 
一個死單元格如果恰好有三個活鄰居，就會復活，彷彿經歷了繁殖。

These simple rules yield a complex dance of life and death. 
這些簡單的規則產生了生命與死亡的複雜舞蹈。

Welcome to the Conway's Game of Life. 
歡迎來到康威的生命遊戲（Conway's Game of Life）。

It's a popular programming exercise to calculate the next state based on the current state. 
計算基於當前狀態的下一個狀態是一個頗受歡迎的程式設計練習。

My first thought was, why not use a 2D array of Booleans? 
我的第一個想法是，為什麼不使用二維的 Boolean 陣列？

Living cells are true, dead cells are false. 
活的單元格是 true，死的單元格是 false。

Simple. To simplify further, I opt for a smaller world, a 3x3 cell grid. 
很簡單。為了進一步簡化，我選擇了一個較小的世界，一個 3x3 的單元格網格。

```java
[
	[false, true, false],
	[false, true, false],
	[true, true, false]
]
```

But this is where I stumbled into the trap of illusory simplicity. 
但這就是我陷入虛假簡單性陷阱的地方。

This approach, seemingly simple, unexpectedly added complexity by forcing me to handle the edge cases. 
這種看似簡單的方法，意外地增加了處理邊緣情況的複雜性。

A more elegant solution, though, is to track the coordinates of living cells. 
然而，更優雅的解決方案是跟蹤活單元格的座標。

```java
[
	{x: 1, y: 0},
	{x: 1, y: 1},
	{x: 0, y: 2},
	{x: 1, y: 2},
	{x: -2, y: -3},
	{x: -200, y: 999999},
]
```

This sidesteps the boundary problem, and boundaries can be added later if needed. 
這避開了邊界問題，而且邊界可以稍後再添加。

This is where Einstein's razor, a principle akin to Occam's razor, comes into play. 
這就是愛因斯坦的剃刀原理，類似於奧卡姆剃刀原理。

It advises us to make everything as simple as possible, but not simpler. 
它建議我們要盡可能地簡單，但不要過度簡化。

In this case, using Booleans might oversimplify and inevitably add complexity.
在這種情況下，使用 Boolean 可能會過度簡化並不可避免地增加複雜性。

Moving on to our second example. 
移步到我們的第二個範例。

Consider a course management system where courses can be public and carry price information, but when a course is private, the price becomes irrelevant. 
考慮一個課程管理系統，課程可以是公開的並攜帶價格資訊，但當課程是私人的時候，價格就變得無關緊要。

```java
publicCourse = {
	name: "Boolean Master",
	isPrivate: false,
	price: Money(100, "USD"),
	earlyBirdPrice: Money(80, "USD"),
	earlyBirdDeadline: Date("2020-01-01"),
}

privateCourse = {
	name: "Boolean Deep Dive",
	isPrivate: true,
}
```

This inconsistency contravenes the principle of least astonishment. 
這種不一致違背了最少驚訝原則。

Whether it's private encapsulates a concrete business concept depends heavily on the business domain. 
私人課程的概念是否屬於具體的商業概念，很大程度上取決於業務領域。

But with only Boolean, it is not enough to maintain the consistency. 
但僅憑 Boolean，不足以維持一致性。

In conclusion, while Boolean data is the simplest data type, it can sometimes be deceptively simple. 
總結來說，雖然 Boolean 資料是最簡單的資料類型，但有時可能會令人誤解。

If it fails to effectively represent the problem at hand, other complexities creep in. 
如果它無法有效地表示手頭的問題，其他複雜性就會悄然而生。

Remember Einstein's razor and the principle of least astonishment. 
記住愛因斯坦的剃刀原理和最少驚訝原則。

Strive to make illegal statements unrepresentable. 
努力使非法陳述無法表示。

Thanks for watching. 
感謝您的觀看。

There's so much more about Booleans I'd love to share, maybe another time. 
關於 Boolean 還有很多我想分享的，也許改天吧。

Next, let's explore the art of copypasting.
接下來，讓我們探索複製貼上的藝術。