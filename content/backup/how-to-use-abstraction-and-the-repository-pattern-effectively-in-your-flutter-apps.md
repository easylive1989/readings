---
date: '2025-04-05T23:18:28+08:00'
title: 'How to Use Abstraction and the Repository Pattern Effectively in Your Flutter Apps'
tags: ["Andrea Bizzotto"]
---

[Source](https://codewithandrea.com/articles/abstraction-repository-pattern-flutter/?utm_source=canopas-stack-weekly)

If your Flutter app talks to a backend or some server-side APIs, you’ll want to have a good **separation of concerns** between the UI code and your data-access layer.

如果你的 Flutter 應用程式需要與後端或某些伺服器端 API 通訊，你會希望在 UI 代碼和資料存取層之間有良好的**責任分離**。

A very good way to do this is to use the [repository pattern](https://codewithandrea.com/articles/flutter-repository-pattern/), which lets you encapsulate all the data access logic (serialization, networking) inside a single class (the repository).

一個非常好的方法是使用[儲存庫模式 (repository pattern)](https://codewithandrea.com/articles/flutter-repository-pattern/)，這種模式讓你可以將所有資料存取邏輯（如序列化、網路請求）封裝在一個類別（儲存庫）中。

The benefit is that the rest of your app only communicates to the **public interface** of that repository and doesn’t need to know about all its implementation details (such as what 3rd party APIs are used under the hood).

其好處在於，應用程式的其他部分只需要與儲存庫的**公開介面**進行通訊，無需了解其所有實作細節（例如底層使用了哪些第三方 API）。

The repository pattern has several advantages:

儲存庫模式有以下幾個優點：

- if the implementation changes but the interface stays the same, the rest of the app is unaffected
- your code becomes more testable (you can mock the repository in your tests)
- you can scale horizontally by creating multiple repositories if needed

- 如果實作發生變化，但介面保持不變，應用程式的其他部分不會受到影響。
- 你的代碼更容易進行測試（在測試中可以模擬儲存庫）。
- 如果需要，你可以通過建立多個儲存庫來實現水平擴展。

And in this article, we’ll go further and learn how to write **backend-agnostic** code, by covering some useful **abstractions**.

在本文中，我們將更進一步，學習如何撰寫**與後端無關**的代碼，並介紹一些實用的**抽象概念**。

> Abstraction lets you decouple the implementation details of a piece of code (usually a class or function) from the rest of the application. When used correctly, abstraction makes our code easier to **understand**, **maintain**, and **test**, and also allows developers to work independently on different parts of the codebase.
> 
> 抽象可以讓你將代碼（通常是一個類別或函數）的實作細節與應用程式的其他部分分離開來。正確使用抽象可以讓代碼更容易**理解**、**維護**和**測試**，並且允許開發者在代碼庫的不同部分獨立工作。


As part of this, we’ll talk about “leaky” abstractions, which happen when the public interface of the repository “leaks” some implementation details that should remain hidden.

作為其中的一部分，我們會討論「洩漏性」抽象（leaky abstractions），這種情況發生在儲存庫的公開介面洩漏了一些應該保持隱藏的實作細節時。

We’ll also talk about tradeoffs and learn that some kinds of abstractions are worthwhile while others aren’t.

我們還會討論取捨問題，學習哪些抽象是值得的，哪些則不值得。

In covering all these concepts, I’ll show you some examples using the Firebase packages since they have a large API surface. But the same considerations apply if you use alternative backends (or, for that matter, any other local or remote storage packages your code depends on).

在涵蓋這些概念時，我會使用 Firebase 套件舉例，因為它們擁有豐富的 API。儘管如此，如果你使用的是其他後端（或任何本地或遠端的存儲套件），這些考量同樣適用。

By the end, you’ll know:

到本文結尾，你將學到：

- how to write repositories that are backend-agnostic (if you use a remote database)
- how to spot “leaky” abstractions in your code and how to fix them if desired
- which abstractions are good and make your code more testable, scalable, and maintainable
- which abstractions are overkill - or even counterproductive

- 如何撰寫與後端無關的儲存庫（如果你使用遠端資料庫）。
- 如何發現代碼中的「洩漏性」抽象，以及如何修復它們（如果需要）。
- 哪些抽象是好的，能讓代碼更容易測試、擴展和維護。
- 哪些抽象是過度的，甚至可能適得其反。

Ready? Let’s go!

準備好了嗎？開始吧！

> If you’re ever planning to migrate a live app to a different backend, there are many considerations to make, including **data migration strategies** and how to **keep downtime to a minimum**. These are complex topics that are beyond the scope of this article. But writing a backend-agnostic codebase is a good first step that will make the rest of the process easier.
> 
> 如果你計劃將一個線上應用程式遷移到不同的後端，需要考慮許多因素，包括**資料遷移策略**以及如何**將停機時間降至最低**。這些是超出本文範圍的複雜話題，但撰寫一個與後端無關的代碼庫是讓整個過程更輕鬆的第一步。


## How to write backend-agnostic code: a practical example

Suppose we want to read a collection of items from a remote database (such as Cloud Firestore) and show the result inside a list view.

假設我們希望從遠端資料庫（例如 Cloud Firestore）讀取一個項目集合，並將結果顯示在列表視圖中。

If we use the [FirestoreListView](https://pub.dev/documentation/firebase_ui_firestore/latest/firebase_ui_firestore/FirestoreListView-class.html) widget from the [firebase_ui_firestore](https://pub.dev/packages/firebase_ui_firestore) package, this can be easily accomplished by creating this simple widget:

如果我們使用來自 [firebase_ui_firestore](https://pub.dev/packages/firebase_ui_firestore) 套件的 [FirestoreListView](https://pub.dev/documentation/firebase_ui_firestore/latest/firebase_ui_firestore/FirestoreListView-class.html) 元件，可以透過以下簡單的元件輕鬆完成：

```dart
import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:firebase_ui_firestore/firebase_ui_firestore.dart';

// example code adapted from https://pub.dev/packages/firebase_ui_firestore
class ItemsList extends StatelessWidget {
  const ItemsList({super.key});

  @override
  Widget build(BuildContext context) {
    // get a query representing the data we want to get
    final itemsQuery =
        FirebaseFirestore.instance.collection('items').orderBy('name');
    // use it to show all the items in the UI
    return FirestoreListView<Map<String, dynamic>>(
      query: itemsQuery,
      itemBuilder: (context, snapshot) {
        Map<String, dynamic> item = snapshot.data();
        return Text('Item name is ${item['name']}');
      },
    );
  }
}
```

The code above will work, but it clumps together two things that should be separate:

上述代碼雖然可以運行，但它將兩個應該分離的部分結合在一起：

- getting the data from the remote backend
- showing the data in the UI

- 從遠端後端獲取資料
- 在 UI 中顯示資料

This has some drawbacks:

這樣的做法存在以下缺點：

- the code is not testable (because we use `FirebaseFirestore.instance` directly in the widget)
- the UI code needs to know about Firestore’s `QueryDocumentSnapshot` API to extract the data
- the UI code also needs to read key-value pairs inside the snapshot’s data (`Map<String, dynamic>`)

- 代碼不可測試（因為我們直接在元件中使用了 `FirebaseFirestore.instance`）。
- UI 代碼需要知道 Firestore 的 `QueryDocumentSnapshot` API，才能提取資料。
- UI 代碼還需要從快照資料的 key-value 對 (`Map<String, dynamic>`) 中讀取內容。

In other words, we have a poor separation of concerns because the UI code knows too many details about how to get the data and how it is structured.

換句話說，我們的責任分離做得不好，因為 UI 代碼知道太多有關於如何獲取資料及其結構的細節。

Can we do better?

我們可以改善嗎？

### Adding a Data Layer

Rather than keeping all the code inside the `ItemsList` widget, we can introduce a separate data layer:

與其將所有代碼都放在 `ItemsList` 元件內，不如引入一個單獨的資料層：

![Separation of concerns between the presentation and data layers](https://codewithandrea.com/articles/abstraction-repository-pattern-flutter/images/presentation-data-layer.webp)

This data layer could contain an `ItemsRepository` that is defined as follows:

這個資料層可以包含一個 `ItemsRepository`，其定義如下：

```dart
class ItemsRepository {
  ItemsRepository(this._firestore);
  final FirebaseFirestore _firestore;

  Query<Item> itemsQuery() {
    return _firestore
        .collection('items')
        .withConverter(
          fromFirestore: (snapshot, _) => Item.fromMap(snapshot.data()!),
          toFirestore: (item, _) => item.toMap(),
        )
        .orderBy('name');
  }
}
```

We can create the corresponding provider as well (using Riverpod):

我們還可以創建對應的提供者（使用 Riverpod）：

```dart
final itemsRepositoryProvider = Provider<ItemsRepository>((ref) {
  return ItemsRepository(FirebaseFirestore.instance);
});
```

> If you're not a Riverpod user, you could use a different dependency injection system like [get_it](https://pub.dev/packages/get_it), or even [flutter_bloc](https://pub.dev/packages/flutter_bloc). Read this for more details: [Singletons in Flutter: How to Avoid Them and What to do Instead](https://codewithandrea.com/articles/flutter-singletons/)
> 
> 如果你不是 Riverpod 的使用者，可以使用其他依賴注入系統，例如 [get_it](https://pub.dev/packages/get_it)，甚至是 [flutter_bloc](https://pub.dev/packages/flutter_bloc)。更多細節請參考：[Singletons in Flutter: How to Avoid Them and What to do Instead](https://codewithandrea.com/articles/flutter-singletons/)

And we could also define a type-safe `Item` class:

我們還可以定義一個類型安全的 `Item` 類別：

```dart
class Item {
  const Item({required this.name});
  final String name;

  factory Item.fromMap(Map<String, dynamic> map) {
    return Item(name: map['name'] as String);
  }

  Map<String, dynamic> toMap() => {'name': name};
}
```

Finally, we can update our widget code:

最後，我們可以更新元件代碼：

```dart
import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:firebase_ui_firestore/firebase_ui_firestore.dart';

class ItemsList extends ConsumerWidget {
  const ItemsList({super.key});

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final itemsQuery = ref.watch(itemsRepositoryProvider).itemsQuery();
    return FirestoreListView<Item>(
      query: itemsQuery,
      itemBuilder: (context, snapshot) {
        Item item = snapshot.data();
        return Text('Item name is ${item.name}');
      },
    );
  }
}
```

Clearly, we had to write more code to accomplish the same result (showing items inside a list).

顯然，為了完成同樣的目標（在列表中顯示項目），我們必須撰寫更多代碼。

But this was not in vain because all the data-access implementation details are now encapsulated in the `ItemsRepository`.

但這並非徒勞，因為現在所有的資料存取實作細節都封裝在 `ItemsRepository` 中。

And the UI code no longer needs to extract the item name from a `Map<String, dynamic>` (which is an implementation detail of the data-access layer). Instead, it can use the `Item` model class that **we have defined**.

並且，UI 代碼不再需要從 `Map<String, dynamic>` 中提取項目名稱（這是資料存取層的實作細節）。相反，UI 可以使用我們**自行定義**的 `Item` 模型類別。

However, our UI code still needs to use the (Firestore-specific) `QueryDocumentSnapshot` type when reading the data, and our widget class still depends on `cloud_firestore.dart` and `firebase_ui_firestore.dart`.

然而，UI 代碼仍然需要使用 Firestore 特定的 `QueryDocumentSnapshot` 類型來讀取資料，而我們的元件類別仍然依賴於 `cloud_firestore.dart` 和 `firebase_ui_firestore.dart`。

And this means that our UI code is still **not backend-agnostic**, and we would have to refactor it further if we wanted to move to a different backend in the future (e.g. Supabase).

這意味著，我們的 UI 代碼仍然**不是與後端無關的**，如果將來我們想切換到其他後端（例如 Supabase），仍需進一步重構。

### Leaky Abstractions

One thing we have overlooked is that the public interface of our `ItemsRepository` contains a Firestore-specific type: the `Query` class. 👇

我們忽略了一件事，就是我們的 `ItemsRepository` 的公開介面中包含了一個 Firestore 特定的類型：`Query`。👇

```dart
class ItemsRepository {
  ItemsRepository(this._firestore);
  final FirebaseFirestore _firestore;

  // Query is defined inside cloud_firestore.dart
  Query<Item> itemsQuery() { ... }
}
```

I call this a **leaky abstraction** because our repository exposes a type that is defined inside the [cloud_firestore](https://pub.dev/packages/cloud_firestore) package, as can be seen in this diagram:

我稱這為**洩漏性抽象**，因為我們的儲存庫暴露了一個在 [cloud_firestore](https://pub.dev/packages/cloud_firestore) 套件中定義的類型，如下圖所示：

![Can you spot the "leaky" abstraction?](https://codewithandrea.com/articles/abstraction-repository-pattern-flutter/images/presentation-data-layer-query.webp)

As a result, any code that **consumes** the `itemsQuery()` method will also depend on `cloud_firestore.dart`:

結果是，任何使用 `itemsQuery()` 方法的代碼都會依賴於 `cloud_firestore.dart`：

```dart
// [FirestoreListView] depends on cloud_firestore.dart
FirestoreListView<Item>(
  query: ref.watch(itemsRepositoryProvider).itemsQuery(),
  itemBuilder: (context, snapshot) {
    Item item = snapshot.data();
    return Text('Item name is ${item.name}');
  },
);
```

But hang on!

但等等！

If we want to use `FirestoreListView` in our code, we have no other choice but to give it a `Query` argument. And inside the `itemBuilder` callback, we have to use `QueryDocumentSnapshot` (which is also Firebase-specific).

如果我們希望在代碼中使用 `FirestoreListView`，別無選擇，只能為它提供一個 `Query` 參數。而在 `itemBuilder` 回調中，我們必須使用 `QueryDocumentSnapshot`（這也是 Firebase 特有的）。

Indeed, the whole point of using the [firebase_ui_firestore](https://pub.dev/packages/firebase_ui_firestore) package is that:

事實上，使用 [firebase_ui_firestore](https://pub.dev/packages/firebase_ui_firestore) 套件的核心目的在於：

> Firebase UI for Firestore enables you to easily integrate your application UI with your Cloud Firestore database.
> 
> Firebase UI for Firestore 讓你可以輕鬆地將應用程式的 UI 與 Cloud Firestore 資料庫整合。

So, whether we’ve done this intentionally or not, **we’ve made a tradeoff**.

因此，不管我們是否有意識到這點，**我們做了一個取捨**。

We’ve chosen to create a leaky abstraction (using `Query` in the public interface of our repository) in return for the ease of use of the `FirestoreListView` widget.

我們選擇建立一個洩漏性抽象（在儲存庫的公開介面中使用 `Query`），以換取 `FirestoreListView` 元件的易用性。

But this abstraction comes with some **tangible benefits** because by using `FirestoreListView`, [we get pagination for free](https://codewithandrea.com/articles/firestore-pagination-list-view/) and a convenient way to [handle loading and error states](https://pub.dev/packages/firebase_ui_firestore#loading-and-error-handling).

但這個抽象帶來了一些**實際的好處**，因為使用 `FirestoreListView`，[我們可以免費獲得分頁功能](https://codewithandrea.com/articles/firestore-pagination-list-view/)，以及一種方便的方式來[處理加載和錯誤狀態](https://pub.dev/packages/firebase_ui_firestore#loading-and-error-handling)。

> Implementing Firestore pagination manually can be a lot of work. So if there’s an official package that does a good job at it, we’d be silly not to use it. 😉
> 
> 手動實現 Firestore 的分頁功能可能需要大量工作。因此，如果有一個官方套件能很好地處理這些問題，我們當然應該用它。😉

### Revisiting the Leaky Abstraction

But let’s suppose that we’ve decided to move away from Cloud Firestore, and we want to make our code truly backend-agnostic.

但假設我們決定放棄使用 Cloud Firestore，並希望讓代碼真正做到與後端無關。

How can we do that?

我們該怎麼做？

Well, assuming we choose another remote database that **supports realtime listeners**, we can modify our repository to use a `Stream` and consume it in our widget using a regular `ListView`, like this:

假設我們選擇另一個支持**實時監聽**的遠端資料庫，可以修改儲存庫以使用 `Stream`，並在元件中透過普通的 `ListView` 使用它，例如：

![Updated implementation: the presentation layer is now backend-agnostic as it no longer depends on the Query type](https://codewithandrea.com/articles/abstraction-repository-pattern-flutter/images/presentation-data-layer-stream.webp)

Here’s what the **public interface** would look like:

以下是**公開介面**的樣子：

```dart
class ItemsRepository {
  Stream<List<Item>> itemsStream() { ... }
}
```

And here’s what the complete repository would look like:

完整的儲存庫代碼如下：

```dart
class ItemsRepository {
  ItemsRepository(this._firestore);
  final FirebaseFirestore _firestore;

  Stream<List<Item>> itemsStream() {
    return _firestore
        .collection('items')
        .withConverter(
          fromFirestore: (snapshot, _) => Item.fromMap(snapshot.data()!),
          toFirestore: (item, _) => item.toMap(),
        )
        .orderBy('name')
        // needed to transform a Query<User> to a Stream<List<Item>>
        .snapshots()
        .map((snapshot) =>
            snapshot.docs.map((snapshot) => snapshot.data()).toList());
  }
}
```

This new API is backend-agnostic because it only uses **built-in types** (such as `Stream`) and types **we have defined** (such as the `Item` class).

這個新的 API 是後端無關的，因為它只使用了**內建類型**（例如 `Stream`）和**我們自己定義的類型**（例如 `Item` 類別）。

If we want to consume the new Stream-based API in the UI, we can use `StreamBuilder` ([which is quite clunky](https://codewithandrea.com/articles/flutter-use-async-value-not-future-stream-builder/)) - or, better still - create a `StreamProvider`:

如果我們想在 UI 中使用新的基於 Stream 的 API，可以使用 `StreamBuilder`（[雖然有些繁瑣](https://codewithandrea.com/articles/flutter-use-async-value-not-future-stream-builder/)）——或者更好地，創建一個 `StreamProvider`：

```dart
final itemsStreamProvider = StreamProvider.autoDispose<List<Item>>((ref) {
  return ref.watch(itemsRepositoryProvider).itemsStream();
});
```

Then, in the UI, we can do this:

在 UI 中可以這樣使用：

```dart
class ItemsList extends ConsumerWidget {
  const ItemsList({super.key});

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    // get an AsyncValue<Item> by watching the stream provider
    final itemsAsync = ref.watch(itemsStreamProvider);
    // use pattern matching to handle the data, loading, error states
    return itemsAsync.when(
      data: (items) => ListView.builder(
        itemBuilder: (context, index) {
          final item = items[index];
          return Text('Item name is ${item.name}');
        },
        itemCount: items.length,
      ),
      loading: () => const CircularProgressIndicator(),
      error: (e, st) => Text(e.toString()),
    );
  }
}
```

Note that this is more code than the original implementation based on `FirestoreListView`. And **we no longer get the built-in pagination support**.

請注意，這比基於 `FirestoreListView` 的原始實現代碼更多。而且，**我們不再獲得內建的分頁支持**。

But we can confidently say that this implementation is **truly backend-agnostic**.

但我們可以自信地說，這種實現是**真正後端無關的**。

And if later on we decided to implement a `SupabaseItemsRepository`, the UI code **would remain the same**.

如果後來我們決定實現一個 `SupabaseItemsRepository`，UI 代碼**將保持不變**。

### When are abstractions worth it?

So far, we have considered these two abstractions:

到目前為止，我們考慮了以下兩種抽象：

1. Moving the data-access code into a repository
2. Replace `Query<Item>` with `Stream<List<Item>`
   
1. 將資料存取代碼移到一個儲存庫中
2. 將 `Query<Item>` 替換為 `Stream<List<Item>>`

The first abstraction is worth it, because leads to a better separation of concerns between UI and data-access logic.

第一種抽象是值得的，因為它在 UI 和資料存取邏輯之間實現了更好的責任分離。

But the second one isn't, as we had to write more code, while losing some useful functionality (pagination) along the way.

但第二種抽象則不值得，因為我們必須撰寫更多代碼，並且在此過程中失去了某些有用的功能（例如分頁）。

But there's one more thing we need to consider. 👇

但還有一件事需要考慮。👇

### Note about testing

One of the benefits of abstraction is that we can more easily test our code.

抽象的一個好處是我們可以更輕鬆地測試代碼。

So what can we say about testing with regards to the `Query` vs `Stream`-based implementations in the example above?

那麼，對於上述例子中的基於 `Query` 和基於 `Stream` 的實現，我們能說些什麼呢？

Let’s consider our `ItemsRepository` once again:

再看看我們的 `ItemsRepository`：

```dart
class ItemsRepository {
  ...

  // first solution using a query
  Query<Item> itemsQuery() { ... }

  // second solution using a stream
  Stream<List<Item>> itemsStream() { ... }
}
```

How can we mock the `ItemsRepository` so that it returns the data we want?

如何模擬 `ItemsRepository`，以便它返回我們想要的資料？

If we use [mocktail](https://pub.dev/packages/mocktail), we can create our mock like this:

如果我們使用 [mocktail](https://pub.dev/packages/mocktail)，可以這樣創建模擬：

```dart
import 'package:mocktail/mocktail.dart';

class MockItemsRepository extends Mock implements ItemsRepository {}
```

And then, we can stub the `itemsQuery()` and `itemsStream()` methods so they can respond when called.

接著，我們可以模擬 `itemsQuery()` 和 `itemsStream()` 方法，使它們在被調用時返回特定響應。

In the stream-based approach, we could setup our mock like this:

在基於 Stream 的方法中，我們可以這樣設置模擬：

```dart
final mockRepo = MockItemsRepository();
when(() => mockRepo.itemsStream()).thenAnswer(
 (invocation) => Stream.value([
   const Item(name: 'Beer'),
   const Item(name: 'Wine'),
 ]),
);
```

But in the query-based approach, we're out of luck:

但在基於 Query 的方法中，情況就不妙了：

```dart
final mockRepo = MockItemsRepository();
// ! Abstract classes can't be instantiated.
// ! Try creating an instance of a concrete subtype.        
when(() => mockRepo.itemsQuery()).thenReturn(Query());
```

That’s because [`Query`](https://pub.dev/documentation/cloud_firestore/latest/cloud_firestore/Query-class.html) is defined as an abstract class inside the [cloud_firestore](https://pub.dev/packages/cloud_firestore) package.

因為 [`Query`](https://pub.dev/documentation/cloud_firestore/latest/cloud_firestore/Query-class.html) 是在 [cloud_firestore](https://pub.dev/packages/cloud_firestore) 套件內定義的一個抽象類別。

But let’s not give up just yet, for we can create a `MockQuery` class:

但別急著放棄，我們還可以創建一個 `MockQuery` 類別：


```dart
class MockQuery<T> extends Mock implements Query<T> {}

final mockRepo = MockItemsRepository();
final mockQuery = MockQuery();

when(() => mockRepo.itemsQuery()).thenReturn(mockQuery);
```

But as it turns out, we get this warning:

不僅如此，我們會看到這樣的警告：

```
The class 'Query' shouldn't be extended, mixed in, or implemented because it's sealed.

Try composing instead of inheriting, or refer to the documentation of 'Query' for more information.
```

This is no good. And if we look at the [documentation for the Query class](https://pub.dev/documentation/cloud_firestore/latest/cloud_firestore/Query-class.html), we see that it contains nearly 20 methods. And it would be hard to figure out which ones we need to stub to get the test working.

這並不好。而且，查看 [Query 類別的文件](https://pub.dev/documentation/cloud_firestore/latest/cloud_firestore/Query-class.html) 後發現，它包含了近 20 個方法。要弄清楚需要模擬哪些方法才能讓測試正常運行，非常困難。

**Bottom line:** it’s really hard to write tests that use the `Query` class since it **is not meant to be mocked**.

**結論：** 使用 `Query` 類別的測試非常難撰寫，因為它**並不是為了被模擬而設計的**。

So when you design the API (public interface) of your repositories, keep testing in mind.

因此，在設計儲存庫的 API（公開介面）時，請考慮到測試需求。

Indeed, sometimes you have to make a trade between **ease of use** and **ease of testing**. And you can choose different abstractions depending on what's most important to you.

的確，有時你需要在**易用性**和**測試便利性**之間做出取捨。根據你認為最重要的點，可以選擇不同的抽象。

---

Learning to choose the right abstractions is a good skill to have.

學習選擇正確的抽象是一項非常有用的技能。

So let’s consider some additional examples so that we can build some more intuition. 👇

因此，我們來看一些額外的例子，藉此建立更多的直覺。👇

## Example: FirebaseAuth wrapper

When adding user authentication to your app, we need to work directly with the `FirebaseAuth` class (unless we use the Firebase UI packages).

當我們將用戶身份驗證功能添加到應用中時，通常需要直接與 `FirebaseAuth` 類進行交互（除非我們使用 Firebase UI 套件）。

Here are some of the methods we can find inside this class (along with many others):

以下是該類的一些方法（還有許多其他方法）：

```dart
// defined in firebase_auth.dart
class FirebaseAuth {
  Future<UserCredential> signInWithEmailAndPassword({
    required String email,
    required String password,
  });

  Future<UserCredential> createUserWithEmailAndPassword({
    required String email,
    required String password,
  });

  Future<void> signOut();

  Stream<User?> authStateChanges();

  User? get currentUser;
}
```

Some of these methods return `UserCredential` and `User`, which are types that are also defined in the same package.

其中一些方法會返回 `UserCredential` 和 `User`，這些類型也都定義在相同的套件中。

But if we use this class **directly** in our code, we have two problems:

但是，如果我們**直接**在代碼中使用這個類，就會遇到兩個問題：

- Using `FirebaseAuth.instance` all over the place makes our code less testable
- We have to import `firebase_auth.dart` every time we want to use the firebase_auth APIs

- 在代碼中到處使用 `FirebaseAuth.instance` 會讓代碼變得難以測試。
- 每次使用 `firebase_auth` 的 API 都必須導入 `firebase_auth.dart`。

Not great, because the codebase becomes **tightly coupled** to the firebase_auth package.

這樣的代碼不太理想，因為代碼庫會與 `firebase_auth` 套件**緊密耦合**。

Let's see if we can do better. 👇

看看有沒有更好的做法👇

### Step 1: Add a Provider

The first step is to create a provider:

第一步是創建一個 provider：

```dart
final firebaseAuthProvider = Provider<FirebaseAuth>((ref) {
  return FirebaseAuth.instance;
});
```

And then, we can **watch** or **read** the `firebaseAuthProvider` as needed.

然後，我們可以根據需要 **watch** 或 **read** `firebaseAuthProvider`。

> If you’re unfamiliar with the difference between `ref.watch` and `ref.read` with Riverpod, check the official documentation page about [Reading a Provider](https://riverpod.dev/docs/concepts/reading).
> 
> 如果不清楚 Riverpod 中 `ref.watch` 和 `ref.read` 的區別，可以參考官方文件：[Reading a Provider](https://riverpod.dev/docs/concepts/reading)。

This will make our code more testable (since we can override `firebaseAuthProvider` with a mock in our tests).

這將使代碼更具可測試性（因為我們可以在測試中用 mock 覆蓋 `firebaseAuthProvider`）。

But any code that uses the `firebaseAuthProvider` will still depend on `firebase_auth.dart`, which can lead to maintenance issues (for example, when upgrading to a new version of firebase_auth that contains breaking changes).

然而，任何使用 `firebaseAuthProvider` 的代碼仍然會依賴 `firebase_auth.dart`，這可能帶來維護問題（例如，升級到有重大變更的 `firebase_auth` 新版本時）。

### Step 2: Write an AuthRepository

The next step is to create a separate `AuthRepository` class that acts as a wrapper for `FirebaseAuth`.

下一步是創建一個獨立的 `AuthRepository` 類，作為 `FirebaseAuth` 的包裝器。

As a first attempt, we could write this code:

我們可以先寫出如下代碼：

```dart
class AuthRepository {
  AuthRepository(this._auth);
  final FirebaseAuth _auth;

  // leaks [UserCredential]
  Future<UserCredential> signInWithEmailAndPassword(String email, String password) {
    return _auth.signInWithEmailAndPassword(
      email: email,
      password: password,
    );
  }

  // leaks [UserCredential]
  Future<UserCredential> createUserWithEmailAndPassword(String email, String password) {
    return _auth.createUserWithEmailAndPassword(
      email: email,
      password: password,
    );
  }

  Future<void> signOut() => _auth.signOut();

  // leaks [User]
  Stream<User?> authStateChanges() => _auth.authStateChanges();

  // leaks [User]
  User? get currentUser => _auth.currentUser;
}
```

This is just a simple wrapper, but we can immediately notice that the public interface is leaking the `UserCredential` and `User` types (from firebase_auth).

這只是一個簡單的包裝器，但我們可以立即注意到，其公共介面洩露了 `UserCredential` 和 `User` 類型（來自 `firebase_auth`）。

**This is probably fine** as long as we don’t plan to migrate to a different authentication system, or don't care too much about testing (more on this below).

**這可能沒問題**，前提是我們不打算遷移到其他身份驗證系統，或不太在意測試（後續會詳述）。

With the methods we added, we can already:

通過添加的方法，我們已經可以做到以下幾點：

- route the user to a “logged in” or “logged out” page depending on the authentication state in `authStateChanges()`
- sign in once the user submits an email & password form, or sign out when the user clicks on a logout button
  
- 根據 `authStateChanges()` 的身份驗證狀態，將用戶路由到“已登入”或“已登出”頁面。
- 當用戶提交郵箱和密碼表單時登入，或點擊登出按鈕時登出。

If we need additional methods from the `FirebaseAuth` class, we can just add them to the `AuthRepository` and use them.

如果我們需要 `FirebaseAuth` 類中的其他方法，只需將它們添加到 `AuthRepository` 並使用即可。

But sometimes, the methods we need are not in the `AuthRepository`class. For example, if we want to send a verification email, we need to call a method that belongs to the `User` class:

但是，有時我們需要的方法並不在 `AuthRepository` 類中。例如，若我們想發送驗證郵件，就需要調用屬於 `User` 類的方法：

```dart
// part of firebase_auth
class User {
  Future<void> sendEmailVerification([
    ActionCodeSettings? actionCodeSettings,
  ]);
}
```

### Step 3 (optional): Write a wrapper for the User class

Now, let’s assume we really want to make our data layer backend-agnostic, such that the `AuthRepository` class doesn’t expose any types from the [firebase_auth](https://pub.dev/packages/firebase_auth) package.

現在，假設我們真的想讓數據層與後端無關，這樣 `AuthRepository` 類就不會暴露任何來自 [firebase_auth](https://pub.dev/packages/firebase_auth) 套件的類型。

Can we do that while retaining the ability to send a verification email or call any other methods from the `User` class?

我們是否能在保留發送驗證郵件或調用其他 `User` 類方法的能力的同時做到這一點？

Well, let’s try to write a wrapper for the `User` class.

我們可以試著為 `User` 類編寫一個包裝器。

We could start by creating an **abstract class** that contains the **methods and fields** we care about:

首先，我們可以創建一個**抽象類別**，其中包含我們關心的**方法和字段**：

```dart
abstract class AppUser {
  String get uid;
  String? get email;
  bool get emailVerified;

  Future<void> sendEmailVerification();
}
```

Then, we could implement a `FirebaseAppUser` that **implements** `AppUser`, by holding on to the underlying `User` **as a private variable**:

然後，我們可以實現一個 `FirebaseAppUser` 類，通過將底層的 `User` 作為**私有變數**來實現 `AppUser`：

```dart
class FirebaseAppUser implements AppUser {
  const FirebaseAppUser(this._user);
  final User _user; // private

  @override
  String get uid => _user.uid;

  @override
  String? get email => _user.email;

  @override
  bool get emailVerified => _user.emailVerified;

  @override
  Future<void> sendEmailVerification() => _user.sendEmailVerification();
}
```

Finally, we could update the `AuthRepository` class so that the relevant methods return the new `AppUser` type (rather than the `User` class from firebase_auth):

最後，我們可以更新 `AuthRepository` 類，讓相關方法返回新的 `AppUser` 類型（而不是來自 `firebase_auth` 的 `User` 類）：

```
class AuthRepository {

  ... 

  Stream<AppUser?> authStateChanges() {
    return _auth.authStateChanges().map(_convertUser);
  }

  AppUser? get currentUser => _convertUser(_auth.currentUser);

  /// Helper method to convert a [User] to an [AppUser]
  AppUser? _convertUser(User? user) =>
      user != null ? FirebaseAppUser(user) : null;
}
```

Here's a visual representation of what we have created:

以下是我們所創建內容的圖示：

![Abstracting away all details of the FirebaseAuth class behind an AuthRepository](https://codewithandrea.com/articles/abstraction-repository-pattern-flutter/images/presentation-data-layer-auth-repository.webp)

With these changes, all implementation details from firebase_auth are abstracted away, and the calling code will only ever use types and classes that **we have defined**.

通過這些更改，所有來自 `firebase_auth` 的實現細節都被抽象掉了，調用代碼只會使用**我們自己定義的類型和類**。

For example, the code for sending a verification email inside a button callback would look like this:

例如，在按鈕回調中發送驗證郵件的代碼將如下所示：

```dart
onPressed: () => ref.read(authRepositoryProvider)
    .currentUser!
    .sendEmailVerification();
```

Once again: since we're not talking to the `FirebaseAuth` APIs directly, the UI code no longer depends on the firebase_auth package.

再次強調：由於我們不再直接與 `FirebaseAuth` API 交互，UI 代碼不再依賴於 `firebase_auth` 套件。

---

But what about testing?

但測試呢？

### Testing with the AuthRepository class

Like we’ve done in the previous example, we could ask ourselves how to mock the `AuthRepository` class so that we can stub its methods and write tests using them.

如同之前的例子，我們可以思考如何 mock `AuthRepository` 類，以便能夠 stub 它的方法並編寫測試。

I won’t go into all the details. But suffice it to say that we have the same problem as before: we can’t create instances of the `User` class because - just like the `Query` class - it’s not meant to be mocked.

這裡就不詳細展開了，但可以肯定的是，我們面臨與之前相同的問題：我們無法創建 `User` 類的實例，因為就像 `Query` 類一樣，它並不是為了 mock 而設計的。

Instead, the right approach is to use composition, which is exactly what we have done by creating a wrapper `FirebaseAppUser` class (along with a base `AppUser` abstract class). This makes it much easier to write mocks without worrying about all the (complex) implementation details of the `User` class.

正確的方法是使用**組合模式**，這正是我們透過創建 `FirebaseAppUser` 包裝器類（以及基礎的 `AppUser` 抽象類）所做的事情。這讓我們能更輕鬆地編寫 mock，而不必擔心 `User` 類的所有（複雜）實現細節。

---

In summary, we’ve learned how to design domain-specific APIs (such as `AuthRepository` and `AppUser`) that only expose the functionality we need while ensuring that the rest of the code doesn’t depend on 3rd party APIs (`FirebaseAuth` and `User` from the firebase_auth package).

我們學習了如何設計特定於領域的 API（例如 `AuthRepository` 和 `AppUser`），這些 API 只暴露我們需要的功能，並確保其餘代碼不依賴第三方 API（例如 `firebase_auth` 套件中的 `FirebaseAuth` 和 `User`）。

Without a doubt, this extra level of indirection forces us to write more code (that needs to be maintained and understood by other developers). And for small projects, it may not be worthwhile.

毫無疑問，這一額外的間接層次迫使我們編寫更多代碼（需要其他開發者理解和維護）。對於小型項目來說，這可能不值得。

But it also makes testing easier, and this is valuable further down the line (or if you have bigger projects).

但這也讓測試變得更加簡單，而這在後期（或對於大型項目）非常有價值。

With this knowledge, let’s take a look at one last example. 👇

了解了這些，我們來看看最後一個例子👇。

## Example: Transactions in Firestore

A common feature in many modern databases is the ability to execute [transactions or batched writes](https://firebase.google.com/docs/firestore/manage-data/transactions).

許多現代數據庫的一個常見功能是執行[交易或批量寫入](https://firebase.google.com/docs/firestore/manage-data/transactions)。

For example, suppose we wanted to implement a method that withdraws money from a bank account, but only if there are available funds.

例如，假設我們希望實現一個方法來從銀行帳戶提款，但只有在有足夠資金時才進行操作。

To avoid race conditions (e.g. two withdrawals happening at the same time), we may implement this using a **transaction**:

為了避免競態條件（例如同時進行的兩次提款），我們可以使用**交易**來實現：

```dart
class BankBalanceRepository {
  const BankBalanceRepository(this._firestore);
  final FirebaseFirestore _firestore;

  Future<void> withdraw(String uid, double amount) {
    return _firestore.runTransaction((transaction) async {
      final accountRef = _firestore.doc('accounts/$uid');
      final snapshot = await transaction.get(accountRef);
      final balance = snapshot.get('balance');
      if (balance > amount) {
        transaction.update(accountRef, {'balance': balance > amount});
      } else {
        throw StateError('Not enough funds');
      }
    });
  }
}
```

In this case, the public interface of the `BankBalanceRepository` doesn’t leak any Firestore-specific types.

在這個例子中，`BankBalanceRepository` 的公共介面不會洩露任何與 Firestore 相關的類型。

But what if we need to call multiple methods that are defined in separate repositories and run all the code inside a single transaction?

但是，如果我們需要調用多個定義在不同存儲庫中的方法，並將所有代碼都放在一個單一的交易中執行，該怎麼辦？

Should we try to somehow mimic the Firestore transaction APIs by creating our own wrappers?

我們是否應該嘗試模擬 Firestore 的交易 API，創建自己的包裝器？

```dart
// a wrapper class for running Cloud Firestore transactions
class FirestoreTransaction {
  const FirestoreTransaction(this._firestore);
  final FirebaseFirestore _firestore;

  // a wrapper for the [runTransaction] method
  // strictly speaking, we should write a wrapper for [TransactionHandler] too
  Future<T> runTransaction<T>(TransactionHandler<T> transactionHandler) {
    return _firestore.runTransaction(transactionHandler);
  }
}
```

This kind of abstraction quickly takes us down a rabbit hole, and I’d argue that it’s overkill.

這類抽象很容易讓我們陷入過於複雜的境地，而我認為這是沒有必要的。

Instead, it may be best to keep things simple by bringing all the logic inside a single method (such as the `withdraw` method above) that doesn’t leak any Firestore-specific APIs to the outside.

相反，最好保持簡單，將所有邏輯放在單一方法中（例如上面的 `withdraw` 方法），而不向外部洩露任何與 Firestore 相關的 API。

## Warning against “lowest common denominator” API design

Initially, we set out on a quest to discover how to write backend-agnostic APIs.

最初，我們試圖探索如何編寫與後端無關的 API。

Along the way, we’ve learned how to write repositories that encapsulate the implementation details of 3rd party APIs.

在過程中，我們學會了如何編寫封裝第三方 API 實現細節的存儲庫。

We’ve also seen that some abstractions can “leak” external types (such as the `itemsQuery()` method that returns a `Query<Item>`):

我們也看到某些抽象會「洩露」外部類型（例如返回 `Query<Item>` 的 `itemsQuery()` 方法）：

![A basic abstraction that separates the presentation and data layers (backend-specific)](https://codewithandrea.com/articles/abstraction-repository-pattern-flutter/images/presentation-data-layer-query.webp)

And we said that if we really wanted to, we could have replaced `Query<T>` with `Stream<List<T>>`. But in doing so, we ended up with a less powerful API that doesn’t support pagination:

我們提到，如果真的需要，可以將 `Query<T>` 替換為 `Stream<List<T>>`。但這麼做會導致一個不夠強大的 API，因為它不支持分頁：

![Updated implementation: the presentation layer is now backend-agnostic as it no longer depends on the Query type](https://codewithandrea.com/articles/abstraction-repository-pattern-flutter/images/presentation-data-layer-stream.webp)

Indeed, designing APIs with abstraction in mind can lead to the “lowest common denominator” syndrome.

實際上，專注於抽象設計 API 可能會導致「最低公分母」的問題。

In other words, by writing backend-agnostic code, we risk creating APIs that are too generic and can’t make the most of what our backend has to offer.

換句話說，透過編寫與後端無關的代碼，我們可能創建出過於通用的 API，無法充分利用後端的特性。

For example, Cloud Firestore offers features such as **realtime listeners**, **caching**, **transactions**, and **offline mode**.

例如，Cloud Firestore 提供了**實時監聽**、**緩存**、**交易**和**離線模式**等功能。

On the other hand, a simple REST API gives you only one feature: the ability to make a request and get a response (as a one-time read).

另一方面，一個簡單的 REST API 只提供一個功能：發送請求並獲取響應（僅限一次性讀取）。

But it would be a mistake to write a Cloud Firestore wrapper that can only read data using futures (rather than streams or queries) because one day, we **might** move to a REST API that doesn’t support realtime listeners.

但如果我們出於未來可能切換到不支持實時監聽的 REST API 而編寫只能使用 future（而不是 stream 或 query）讀取數據的 Firestore 包裝器，那就錯了。

So try to design APIs that **let you make the most** of all the features your backend offers **today**, and don’t compromise functionality “for the sake of abstraction” or try to make your code “future-proof”.

所以，試著設計 API，**充分利用後端目前所提供的所有功能**，不要為了「抽象」而妥協功能，或者試圖讓代碼「未來-proof」。

**Remember: abstraction is a tool, not a goal.**

**記住：抽象是一種工具，而不是目的。**

## Conclusion

When building an app, it’s important to use API design principles, design patterns, and app architecture to ensure your code is testable, scalable, and maintainable.

在構建應用時，使用 API 設計原則、設計模式和應用架構來確保代碼可測試、可擴展和可維護非常重要。

But these are all skills that you will acquire by practising over time.

但這些都是需要通過時間不斷練習才能掌握的技能。

Whether you’re starting out or you have a few years of experience, remember this: **don’t overcomplicate things and keep it simple**.

無論你是剛起步還是已經有一些經驗，請記住：**不要過於複雜化，保持簡單**。

I hope this article has helped you build some intuition about when to use abstractions (and when not to), so you can make the right tradeoffs depending on what's most important to you.

希望這篇文章能幫助你建立對於何時使用抽象（以及何時不使用）的直覺，讓你能根據最重要的需求做出正確的取捨。