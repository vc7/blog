---
layout: post
title: "XCTest Assertions 及其種類"
date: 2014-07-10 09:47:12 +0800
comments: true
categories: ["iOS", "Unit Test", "XCTest"]
keywords: iOS, Unit Test, XCTest
description: "XCTest Assertions 是來自於 XCTest framework ，雖然在 2013 年 Xcode 5 就出來了，目前還是沒有正式 document ，只有 2014/6/12 出的 pre-release documents 有詳加敘述，不過還是看看他有哪些 assertions 可以用吧！"
references: [{title: "XCTest Assertions - Apple", link: "https://developer.apple.com/library/prerelease/ios/documentation/DeveloperTools/Conceptual/testing_with_xcode/testing_3_writing_test_classes/testing_3_writing_test_classes.html#//apple_ref/doc/uid/TP40014132-CH4-SW34"}, {title: "Scalar Variables", link: "http://theory.uwinnipeg.ca/programming/node63.html"}, {title: "How to use XCTAssertThrowsSpecific - Stake Overflow", link: "http://stackoverflow.com/a/20415749"}]
hero: "https://farm4.staticflickr.com/3873/14431962230_526b9849b5_o.png"
---

XCTest Assertions 是來自於 XCTest framework ，雖然在 2013 年 Xcode 5 就出來了，目前還是沒有正式 document ，只有 2014/6/12 出的 pre-release documents 有詳加敘述，不過還是看看他有哪些 assertions 可以用吧！

<!-- more -->

## XCTest Assertions 自訂訊息

XCTest Assertions 中每一個 methods 中最後一個參數都是 format... ，可以自訂錯誤訊息，就可以顯示在預設的錯誤訊息後段，不過也可以省略。如下圖：

{% img center https://farm6.staticflickr.com/5562/14618150485_03093134c3_o.png %}

## Assertions 列表

主要分成以下幾個種類：

- **Unconditional Fail:** 不管怎樣就是讓你中斷
- **Equality Tests:** 測試相等性
- **Nil Tests:** 測試物件是否為 nil
- **Boolean Tests:** 測試真值
- **Exception Tests:** 測試例外

### Unconditional Fail

#### XCTFail

``` objc
XCTFail(format...)
```

無條件產生錯誤。

### Equality Tests

這些 assertions 提供可以檢查各種等式的 methods

#### XCTAssertEqualObjects

``` objc
XCTAssertEqualObjects(expression1, expression2, format...)
```

當 `expression1` 不等於 `expression2` 或其中一個物件是 nil 的時候產生錯誤。

#### XCTAssertNotEqualObjects

``` objc
XCTAssertNotEqualObjects(expression1, expression2, format...)
```

當 `expression1` 等於 `expression2` 的時候產生錯誤。

#### XCTAssertEqual

``` objc
XCTAssertEqual(expression1, expression2, format...)
```

當 `expression1` 不等於 `expression2` 的時候產生錯誤。用於 C 的 scalar variables 。 Scalar variables 包含 int 、 float 、 double 及 char。

#### XCTAssertNotEqual

``` objc
XCTAssertNotEqual(expression1, expression2, format...)
```

當 `expression1` 等於 `expression2` 的時候產生錯誤。用於 C 的 scalar variables 。


#### XCTAssertEqualWithAccuracy

``` objc
XCTAssertEqualWithAccuracy(expression1, expression2, accuracy, format...)
```

這個和下一個 method 多出了一個 accuracy 的參數，當 `expression1` 和 `expression2` 的差別 **大於** `accuracy` 的時候產生錯誤。而這類的檢查可以用來確認較為精確的 scalar variable types ，如 float 以及 double 。不過其他的 scalar variable 種類也是可以使用。

##### 範例

這一行的檢查，由於 0.1 和 0.2 的差 0.01 大於 0.01 ，所以這時候會產生錯誤：

``` objc
XCTAssertEqualWithAccuracy(0.1, 0.2, 0.01);
```

#### XCTAssertNotEqualWithAccuracy

``` objc
XCTAssertNotEqualWithAccuracy(expression1, expression2, accuracy, format...)
```

當 `expression1` 和 `expression2` 的差別 **小於及等於** `accuracy` 的時候產生錯誤。用法則和上前一個 method 一樣：適合用於檢查精確度比較高的 scalar variable types - float 以及 double 。也適用其他種類的 scalar variables 。

#### XCTAssertGreaterThan

``` objc
XCTAssertGreaterThan(expression1, expression2, format...)
```

適用 scalar values。當 `expression1` **不大於** ，即小於或等於， `expression2` 時產生錯誤。

#### XCTAssertGreaterThanOrEqual

``` objc
XCTAssertGreaterThanOrEqual(expression1, expression2, format...)
```

適用 scalar values。當 `expression1` **不大於及不等於** ，即小於， `expression2` 時產生錯誤。

#### XCTAssertLessThan

``` objc
XCTAssertLessThan(expression1, expression2, format...)
```

適用 scalar values。當 `expression1` **不小於** ，即大於或等於， `expression2` 時產生錯誤。

#### XCTAssertLessThanOrEqual

``` objc
XCTAssertLessThanOrEqual(expression1, expression2, format...)
```

適用 scalar values。當 `expression1` **不小於及不等於** ，即大於， `expression2` 時產生錯誤。

### Nil Tests

#### XCTAssertNil

``` objc
XCTAssertNil(expression, format...)
```

當 `expression` 傳入的物件不是 `nil` 時產生錯誤。

#### XCTAssertNotNil

``` objc
XCTAssertNotNil(expression, format...)
``` 

當 `expression` 傳入的物件是 `nil` 時產生錯誤。

### Boolean Tests

#### XCTAssertTrue

``` objc
XCTAssertTrue(expression, format...)
```

當 `expression` 判定為 `false` 時產生錯誤。

#### XCTAssert

``` objc
XCTAssert(expression, format...)
```

當 `expression` 判定為 `false` 時產生錯誤。與 `XCTAssertTrue` 的用法相同。

#### XCTAssertFalse

``` objc
XCTAssertFalse(expression, format...)
```

當 `expression` 判定為 `true` 時產生錯誤。

### Exception Tests

#### XCTAssertThrows

``` objc
XCTAssertThrows(expression, format...)
```

當 `expression` 沒有丟出 exception 時產生錯誤。

#### XCTAssertThrowsSpecific

``` objc
XCTAssertThrowsSpecific(expression, exception_class, format...)
```

當 `expression` 沒有丟出特定 class 的 exception 時產生錯誤。

##### 範例

``` objc
XCTAssertThrowsSpecific([object methodThatShouldThrow], NSException, @"Should throw a NSException")
```

這個範例做的測試是：當 `[object methodThatSholdThrow]` 丟的 exception 不是 `NSException` 的話，就會產生錯誤。

#### XCTAssertThrowsSpecificNamed

``` objc
XCTAssertThrowsSpecificNamed(expression, exception_class, exception_name, format...)
```

當 `expression` 沒有丟出特定 class 以及其中沒有包含特定 exception 名稱時產生錯誤。對像 `AppKit` 或 `Foundation` 這類會丟特定 exception name 的 frameworks 還滿有用的，如 `NSInvalidArgumentException` 等。

##### 範例

```objc
XCTAssertThrowsSpecificNamed([object methodThatShouldThrow], NSException, NSInvalidArgumentException, @"Should throw NSInvalidArgumentException");
```

這個範例做的測試是：當 `[object methodThatSholdThrow]` 丟的 exception 不是 `NSException` 以及是 `NSException` ，他的 `name` 卻不是 `NSInvalidArgumentException` 的話，就會產生錯誤。

#### XCTAssertNoThrow

``` objc
XCTAssertNoThrow(expression, format...)
```

當 `expression` 有丟出 exception 時產生錯誤。

#### XCTAssertNoThrowSpecific

``` objc
XCTAssertNoThrowSpecific(expression, exception_class, format...)
```

當 `expression` 有丟出特定 class 的 exception 時產生錯誤。丟出其他 class 的 exception 則不會產生錯誤。

#### XCTAssertNoThrowSpecificNamed

``` objc
XCTAssertNoThrowSpecificNamed(expression, exception_class, exception_name, format...)
```

當 `expression` 有丟出特定 class 以及其中有包含特定 exception 名稱時產生錯誤。對像 `AppKit` 或 `Foundation` 這類會丟特定 exception name 的 frameworks 還滿有用的，如 `NSInvalidArgumentException` 等。


*本篇文章撰寫時基於 2014-6-12 最後更新的 Pre-Release Documentation 版本，如果發現相關內容有更新，請在下方留言，謝謝。*