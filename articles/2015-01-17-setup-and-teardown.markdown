---
layout: post
title: "測試中的 setUp 與 tearDown"
date: 2015-01-17 23:51:45 +0800
comments: true
categories: ['iOS', 'unit test', 'xctest'] 
keywords: unit test, 單元測試, XCTest, TDD
description: '在各種的 test framework 中都會看到這兩個 methods 的影子： setUp 以及 tearDown 。最後也會提到 Xcode 中有什麼額外的 method 可以使用。'
references: []
hero: 'https://farm9.staticflickr.com/8583/16132718417_6af85e7f7f_o.png'
---

在各種的 test framework 中都會看到這兩個 methods 的影子： `setUp` 以及 `tearDown` 。最後也會提到 Xcode 中有什麼額外的 method 可以使用。

<!-- more -->

# 流程

這兩個 methods ， `setUp` 以及 `tearDown` ，前者會在每個 test case 執行前呼叫，後者則是在 test case 結束、紀錄測試結果後被呼叫。

<center><p><img src="https://farm9.staticflickr.com/8677/16130808648_bcd45ed271_o.png"><br><small>基本的測試執行流程</small></p></center>

# 說明

- `setUp` - 設置環境。可以準備給該 class 中所有 test cases 使用的物件。
- `tearDown` - 清理環境。釋放物件、清除設定等。

使用 `setUp` 優點在於可以確保每次測試使用到的共同物件都是新的之外，也可以確保每次使用的新物件初始化的方式也是一樣的。

# XCTest with Xcode

<small><i>in Xcode 5 and later</i></small>

Apple 當然在這個部分也加了鹽。在 XCTest Framework 中，這兩個 methods 除了以 instance method 的方式存在之外，開發者還可以加入 class method 形式的 `+ (void)setUp` 以及 `+ (void)tearDown` 。這兩個 methods 就會在該 class 中**所有的** test cases 的最前以及最後被執行。

流程圖如下：

<center><p><img src="https://farm8.staticflickr.com/7538/16317492432_fd531c8bda_o.png"><br><small>在 Objective-C 中加上 class method 後的測試執行流程</small></p></center>

# 小結

- 在測試中使用 `setUp` 和 `tearDown` 的特性來重複使用程式碼，例如新建和初始化所有測試都要用到的物件
- 不要在 `setUp` 和 `tearDown` 中初始化或清除 **並非** 所有測試都在使用的物件，否則會讓測試難以理解。這樣做的話會讓閱讀程式碼的人不知道哪些測試使用了 `setUp` 裡面的邏輯或物件，哪些測試沒有使用。

# 參考資料
- [Testing with Xcode - Apple Developer](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/testing_3_writing_test_classes/testing_3_writing_test_classes.html#//apple_ref/doc/uid/TP40014132-CH4-SW2)
- [26.3. unittest — Unit testing framework — Python 3.4.2 documentation](https://docs.python.org/3/library/unittest.html#unittest.TestCase.setUp)