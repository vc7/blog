---
layout: post
title: "Swift 神奇轉型 （ Xcode beta 3 ）"
date: 2014-07-10 22:27:10 +0800
comments: true
categories: ["iOS", "Swift", "beta", "bug"]
keywords: iOS, Swift, funny, beta 3, Xcode, Apple
description: "在 Swift 中，當變數沒有指定型別， Swift 會自己抓一個適當的型別來用。今天在 7 月的 CocoaHeads 聚會的時候看到 zonble 分享一個神奇（？）的指定型別。"
references: 
hero: "https://farm4.staticflickr.com/3905/14597366856_7e1acb5341_o.png"
---

在 Swift 中，當變數沒有指定型別， Swift 會自己抓一個適當的型別來用。

今天在 7 月的 CocoaHeads 聚會的時候看到 zonble 分享一個神奇（？）的指定型別。

<!-- more -->

## 指定型別錯誤（？）

這次主角是 array 和 dictionary 。在 Swift 中，沒有分 mutable 和 immutable ，都可以針對他們做修改。

但是在沒有手動指定型別之下，在某些情形下會指定成 `NSArray` 和 `NSDictionary` ：

### Array

{% img center https://farm3.staticflickr.com/2899/14433862187_239b7379a2_o.png %}

### Dictionary

{% img center https://farm4.staticflickr.com/3908/14617046821_365bb178df_o.png %}

### 會爆掉

變成 `NSArray` 和 `NSDictionary` 之後這個 Swift 中的變數再也不是可以任意 mutable ，對他做加減就會報錯掛掉。

## 結論

這應該算是 bug 吧？

不過到這邊我還是覺得，型別該宣告就要養成習慣好好宣告，可以避免不可預期的結果。