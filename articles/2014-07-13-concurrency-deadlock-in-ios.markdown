---
layout: post
title: "Concurrency - Deadlock in iOS"
date: 2014-07-13 18:08:11 +0800
comments: true
categories: ["iOS", "Concurrency", "Deadlock"]
keywords: iOS, concurrency, gcd, deadlock, operations
description: "iOS 中造成 deadlock 的原因。"
references: []
hero: "https://farm4.staticflickr.com/3884/14457587070_c02467f067_o.png"
---

在 iOS 裡面可以自行創建 `NSOperation` 並且可以自行設定 operation 可以在另外一個 operation 執行完之後才執行。

<!-- more -->

## Deadlock

但是這個時候就要小心不要造成 interdependent （相互依賴）的情形發生：

> Operation B 已經依賴於 Operation A 完成之後才執行，但是又設定 Operation A 必須等 Operation B 執行完才可以動作。

這樣就會讓這兩個 operations 永遠等不到對方，而造成 deadlock 。除此之外這樣還會造成消耗記憶體以及可能讓 app 停在那邊不會動的情形發生。