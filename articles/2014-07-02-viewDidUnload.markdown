---
layout: post
title: "iOS 6 後 viewDidUnload 的替代方法"
date: 2014-07-02 10:09:19 +0800
comments: true
categories: ["iOS", "UIViewController", "viewDidUnload"]
keywords: iOS, UIViewController, viewDidUnload, view life cycle, ui
description: "關於 viewDidUnload 在 iOS 6 之後已經被 Deprecated 了，現在該怎麼做？"
references: [{title: "iOS6 viewDidUnload Deprecated - stackoverflow", link: "http://stackoverflow.com/a/12603549"}]
hero: "https://farm3.staticflickr.com/2904/14554126312_71a45ce073_o.png"
---

這兩天在翻新一個比較舊版本的 UI 3rd party library 成自己需要的版本，裡面用到一些 iOS 5 SDK 的 methods，其中注意到的就是 `viewDidUnload` 。之前在寫一些東西的時候，有發現這個 method 被 deprecated 了，現在有機會就把這個 method 寫個文章。

<!-- more -->

## 原先的用法

根據官方的文件，這個方法會在 low memory 的時候被 call 到，開發者就可以在者裡釋放一些物件；在 view controller 的 property `view` （也就是常見的 `self.view` ）被釋放成 `nil` 的時候，也會被 call 到。

## 用 didRecieveMemoryWarning 和 dealloc 

就像我在之前的文章（ [我對 Swift 的看法]({{ root_url }}/2014/06/30/what-I-see-while-swift-came-out/) ）中提到，有用 ARC 並不代表可以不用管記憶體用了多少。當使用太多就必須透過釋放來解決 memory pressure 。

所以 `viewDidUnload` 的替代方法就是 `didRecieveMemoryWarning` 和 `dealloc` ，可以發現比原先的作法在字面上更容易察覺：是要在有 memory warning 還是在 view 釋放的時候做的動作。
