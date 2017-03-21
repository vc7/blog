---
layout: post
title: "怎麼判斷是使用 iOS Simulator"
date: 2014-07-11 18:52:10 +0800
comments: true
categories: ["iOS", "basic"]
keywords: iOS, basic, simulator
description: "如何避免使用 Simulator 沒有的東西造成的 crash"
references: 
hero: "https://farm4.staticflickr.com/3889/14603839136_f771c581da_o.png"
---

這應該是大家都會碰到的問題：寫個拍照的 app ，用模擬器測拍照，相機打開就 crash 了。

<!-- more -->

## TARGET_IPHONE_SIMULATOR

這時候就要判斷現在使用的是否為模擬器 `TARGET_IPHONE_SIMULATOR` ，是的話就不要執行相關的動作：

``` objc
#if TARGET_IPHONE_SIMULATOR
	NSLog(@"You're in simulator");
#endif
```