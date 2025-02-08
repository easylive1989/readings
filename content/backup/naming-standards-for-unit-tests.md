---
date: '2025-02-08T21:30:20+08:00'
title: 'Naming Standards for Unit Tests'
tags: ["Roy Osherove"]
---
[Source](https://osherove.com/blog/2005/4/3/naming-standards-for-unit-tests.html)

_The basic naming of a test comprises of three main parts:_

測試名稱的基本結構包含三個主要部分：

 **[UnitOfWork_StateUnderTest_ExpectedBehavior]**

A unit of work is a use case in the system that startes with a public method and ends up with one of three types of results: a return value/exception, a state change to the system which changes its behavior, or a call to a third party (when we use mocks). so a unit of work can be a small as a method, or as large as a class, or even multiple classes. as long is it all runs in memory, and is fully under our control.

「工作單元」（Unit of Work）是系統中的一個使用案例，它從一個公開方法開始，並最終產生三種類型的結果之一：回傳一個值或拋出異常、造成系統狀態變更，進而影響其行為、呼叫第三方服務（當我們使用 mock 時）。因此，工作單元的範圍可以小至一個方法，大至一個類別，甚至橫跨多個類別。只要它能夠完全在記憶體內執行並受我們掌控，就可視為一個工作單元。

Examples:

```csharp
Public void Sum_NegativeNumberAs1stParam_ExceptionThrown()

Public void Sum_NegativeNumberAs2ndParam_ExceptionThrown ()

Public void Sum_simpleValues_Calculated ()
```

_**Reasons:**_

_**Test name should express a specific requirement**_

***測試名稱應表達明確的需求***

Your unit test name should express a specific requirement. This requirement should be somehow derived from either a business requirement or a technical requirement. In any case, that requirement has been broken down into small enough pieces, each of which represents a test case. If your test is not representing a requirement, why are you writing it? Why is that code even there?

測試名稱應該能表達一個具體的需求，而這些需求通常來自於業務需求或技術需求。這些需求會被拆解成較小的單元，每個單元對應一個測試案例。如果測試無法對應到任何需求，那麼我們需要思考，這個測試的目的為何？這段程式碼是否真的有必要存在？

Also, developers who are on a role are happy to give meaningless or numbered names to their test just so they can continue on with what they are doing. This leads down a narrow slope where quickly no one has the time or energy to fix the names of the tests. Also, most developers don t bother too much with writing _good assert messages_ in their tests. A good assert message (which shows up if the test fails) is jey to understanding what went wrong. Assuming writing good assert messages is one of the key places where most unit test developers fall short, the only thing left to save us then is the name of the test. If both the name of the test and the error messages in it are not expressive enough, the poor maintenance developer (often the same one who developed the tests) is left to wonder what was this joker thinking? looking at a year old code base with unit tests.

許多開發人員在快速開發時，會隨意命名測試，例如使用數字編號或簡單名稱。這種做法會導致後續維護困難，因為當測試失敗時，開發者很難透過名稱快速理解測試的目的。此外，大部分開發者不會特別花心思撰寫**良好的斷言訊息（assert message）**，但這卻是讓測試可讀性提升的關鍵之一。如果測試名稱與錯誤訊息都不夠清楚，未來維護的開發者（可能是自己）將不得不重新閱讀舊程式碼，猜測當初的測試目的。

_**Test name should include the expected input or state and the expected result for that input or state**_

***測試名稱應包含預期的輸入或狀態與預期結果***

To express the specific requirement clearly you should think about two things you need to express: the input values/current state during which the test takes place, and the expected result from that state.

為了清楚表達需求，測試名稱應涵蓋以下兩點：輸入值或測試時的狀態，預期的結果

###### Example:

Given this method :

**Public int Sum(params int[] values)**

You have a requirement that numbers larger than 1000 that are passed in should not be considered for calculation (that is a number bigger than 1000 is equal to zero).

你有一個需求，傳入的數字如果大於 1000，則不應納入計算（也就是說，大於 1000 的數字應視為 0）。

**What about this name: Sum_NumberBiggerThan1000**

That is only half way there. As a reader of the test I can only assume that some input in your test is bigger than 1000, but I do not understand what the _requirement being tested_ is.

但這只完成了一半。作為測試的閱讀者，我只能假設你的測試中某些輸入大於 1000，但我並不清楚被測試的需求是什麼。

**How about this name: Sum_NumberIsIgnored**

Still not good enough. While it does tell me the expected result, it does not tell me under which circumstances it needs to be ignored. I would have to go into the test code and read through it to find out what the expected input is.

這仍然不夠好。雖然它告訴我預期的結果，但並未說明在哪些情況下該數字應被忽略。我必須進入測試代碼並仔細閱讀，才能找到預期的輸入條件。

**How about this name: Sum_NumberIgnoredIfBiggerThan1000**

That s much better. I can tell just by looking at the test name what It means if the test fails. I don t need to look at the code, and I can tell exactly what requirement this test tries to solve.

這樣就好多了。僅從測試名稱，我就能理解測試失敗的意義。我不需要查看代碼，也能準確知道這個測試試圖解決的需求。

**Test name should be presented as a statement or fact of life that expresses workflows and outputs**

**測試名稱應該是陳述句或事實，表達工作流程和輸出結果**

Unit tests are just as much about documentation as they are about testing functionality. You should always assume that someone else is going to read your tests in a year, and that they should not have a hard time understanding your intent.

單元測試不僅是為了測試功能，也是文件化的一部分。你應該假設，未來一年內會有其他人閱讀你的測試，他們不應該難以理解你的意圖。

Test names which are written as short declarative statements are much more expressive that names which are machine readable that is they only make sense if you have an understanding of a hidden code language to dissect their meaning.

測試名稱應該以簡短的陳述句表達需求，這樣比機器可讀但需要理解某種隱藏編碼規則的名稱更具表達力。

People might first complain that the name of the test method appears way too long for them, but in the end it s more about readability. Longer test names do not imply longer or less optimized code, and even if they did, we are not looking to optimize test code performance. If anything, test code should always be optimized for readability.

有人可能會抱怨測試方法名稱太長，但最終這關乎可讀性。較長的測試名稱並不意味著代碼更長或效率更低，即使如此，我們的目標也不是優化測試代碼的效能。相反，測試代碼應始終優先優化可讀性。

Here s an example of a bad an d good test name

以下是壞的與好的測試名稱示例：

Public void SumNegativeNumber2()

And here s a better way to express your intent

這是一種更好的方式來表達你的意圖：

Public void Sum_ExceptionThrownOnNegativeNumberAs2ndParam ()

**Test Name should only begin with Test if it is required by the testing framework or if it eases development and maintenance of the unit tests in some way.**

**測試名稱只有在測試框架要求，或有助於單元測試的開發與維護時，才應該以 "Test" 開頭。**

Since most tests today reside in a specific class or module that is logically defined as a test container (fixture), there are only a handful of reasons why you would want to prefix you test methods with the word Test :

由於現今大多數測試都位於特定的類別或模組中，而這些類別或模組已被邏輯上定義為測試容器（測試裝置，fixture），所以只有少數幾個理由可以讓你選擇在測試方法名稱前加上 "Test"：

- Your unit testing framework requires this
- You have coding standards in place and they include this
- You want to make it easier to navigate to the methods using Intellisense or some other search mechanism.

- 你的單元測試框架要求這樣做
- 你的程式碼標準規範了這一點
- 你希望透過 IntelliSense 或其他搜尋機制更容易找到這些方法

**Test name should include name of tested method or class**

**測試名稱應包含被測試的方法或類別的名稱**

You usually have at least a number of tests for each method you are testing, and you are usually testing a number of methods in one test fixture (since you usually have a test fixture per class tested). This leads to the requirement that your test should also reflect the name of the method begin tested, not only the requirements from it.

通常，每個被測試的方法至少會有多個測試案例，而在一個測試組（Test Fixture）中，通常會測試多個方法（因為通常每個類別都有對應的測試組）。因此，測試名稱除了應該反映測試的需求外，也應該包含被測試的方法名稱，以確保可讀性和可維護性。

For example:

   **[MethodName_StateUnderTest_ExpectedBehavior]**

```csharp
Public void Sum_NegativeNumberAs1stParam_ExceptionThrown()

Public void Sum_NegativeNumberAs2ndParam_ExceptionThrown ()

Public void Sum_simpleValues_Calculated ()

Public void Parse_OnEmptyString_ExceptionThrown()

Public void Parse_SingleToken_ReturnsEqualToeknValue ()
```

**Naming Variables in a test:**

Variable names should express the expected input and state

變數名稱應該表達預期的輸入與狀態。

It s easy to name variables going into a method with generic names, but with unit tests you have to be twice as careful to name them as what they represent, i.e. BAD_DATA or EMPTY_ARRAY or NON_INITIALIZED_PERSON

在一般程式中，為方法的參數命名時，使用通用名稱可能很容易，但在單元測試中，必須更加謹慎，確保變數名稱能清楚表達其代表的意義，例如：`BAD_DATA`、`EMPTY_ARRAY` 或 `NON_INITIALIZED_PERSON`。

You do not need to name them with a CONST casing , but the name does have to represent the intent of the test author.

變數名稱不一定要使用全大寫（CONST 命名方式），但應該能夠清楚傳達測試作者的意圖。

A good way to check if your names are good enough is to look at the ASSERT at the end of the test method. If the ASSERT line is expressing what your requirement is, or comes close, you re probably there.

檢查變數名稱是否恰當的一個好方法，是查看測試方法最後的 `ASSERT` 語句。如果 `ASSERT` 行能夠準確或接近地表達測試需求，那麼你的變數命名很可能是合適的。

For example:

```csharp
Assert.AreEqual(-1, val)
```

Vs.

```csharp
Assert.AreEqual(BAD_INITIALIZATION_CODE, ReturnCode, Method shold have returned a bad initialization code )
```
