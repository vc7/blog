---
layout: post
title: "在客製化的 Cell 取得自己的高度"
date: 2014-07-08 23:35:15 +0800
comments: true
categories: ["iOS", "UIView", "UITableView"]
keywords: iOS, UIView, UITableView, draw
description: "這算是有段時間之前的疑問，最近突然通了。自 2012 年 9 月多，藉由比賽的緣故，開始接觸 iOS 的開發。因為還不了解 UIView 的 drawing life cycle ，想要像 CSS 一樣自由操作一個元件的排版似乎沒有想像中的簡單。不過近一年來不斷在做客製化 UI ，也對什麼時候可以做什麼事情、在哪邊繪製會比較適合就比較清楚了。"
references: 
hero: "https://farm4.staticflickr.com/3858/14625468533_7c15ebb261_o.png"
---

這算是有段時間之前的疑問，最近突然通了。

自 2012 年 9 月多，藉由比賽的緣故，開始接觸 iOS 的開發。因為還不了解 UIView 的 drawing life cycle ，想要像 CSS 一樣自由操作一個元件的排版似乎沒有想像中的簡單。不過近一年來不斷在做客製化 UI ，也對什麼時候可以做什麼事情、在哪邊繪製會比較適合就比較清楚了。

<!-- more -->

## 前情提要

在剛開始學的時候， UITableView 可以指定 cell 的高度是什麼，那麼我在 cell 裡面要怎麼知道自己的高是什麼？當時在 `init` 之類的 methods 去取 frame 或 bounds 都不會是正確的數值。當時沒有什麼人問，所以問題就這樣留在心裡一段時間。

最近突然想到這個問題，就決定來想一下。一開始的思維是：在 UIView drawing UI 的時候，一定會 call 到 `layoutSubviews` 這個 method ，這時候會重繪 subviews 的 的位置配置，那，應該可以在這裡取得由 UITableView 給的 frame 吧！

## 解法

一試之下就發現想法是對的：

``` objc
- (void)layoutSubviews
{
	// 在這裡就可以做根據 cell 寬及高的排版了
}
```

## 應用

像是 Pinterest 的 app 有不等同高度的 cell ，就可以這樣對 cell 內部的元件根據自己 frame 的寬高來排版了。