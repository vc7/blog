---
layout: post
title: "Autoresize Properties 和手動佈局"
date: 2014-09-21 14:54:57 +0800
comments: true
categories: ["iOS", "UI"]
keywords: iOS, view, orientation
description: "看了很多專案得 view 都有設定 `autoresizingMask` ，一直以來都沒有對他很了解。於是看了一下 Documentation ，看要怎麼使用它。最後會講到一些和手動佈局的差異及應用。"
references: [{ title: "UIView | autoresizesSubviews - Apple Documentation", link: "https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIView_class/index.html#//apple_ref/occ/instp/UIView/autoresizesSubviews"}, {title: "UIView | UIViewAutoresizing - Apple Documentation", link: "https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIView_class/index.html#//apple_ref/c/tdef/UIViewAutoresizing"}]
hero: "https://farm4.staticflickr.com/3926/15309094452_15c3379f50_o.png"
---

看了很多專案得 view 都有設定 `autoresizingMask` ，一直以來都沒有對他很了解。於是看了一下 documentation ，看要怎麼使用它。最後會講到一些和手動佈局的差異及應用。

<!-- more -->

## Autoresize Properties

自從 iOS 2.0 以來，當 view 的 bounds 有改變的時候，如果有設定可以 autoresize ，就會根據指定的縮放種類來縮放，相關的 properties 如下：

- `autoresizesSubviews`
- `autoresizingMask`

### autoresizesSubviews

變數型態為 `BOOL` 。這個參數用來指定所屬的 subviews 會不會跟著 super veiw （就是自己） 的 bounds 來縮放。 `YES` 為會。

### autoresizingMask

變數型態為 `UIViewAutoresizing` 。這個參數用 integer bit mask 來設定 autoresize 的種類，並用 C bitwise OR operator 合併起來。以下為其所屬的種類：

- <code>UIViewAutoresizing<span style="color:Crimson">None</span></code> - 指定自己 <span style="color:Crimson">*不要*</span> resize
- <code>UIViewAutoresizing<span style="color:Crimson">FlexibleLeftMargin</span></code> - 指定自己的 <span style="color:Crimson">*左邊界*</span> 位置可以變化
- <code>UIViewAutoresizing<span style="color:Crimson">FlexibleWidth</span></code> - 指定自己的 <span style="color:Crimson">*寬度*</span> 可以變化
- <code>UIViewAutoresizing<span style="color:Crimson">FlexibleRightMargin</span></code> - 指定自己的 <span style="color:Crimson">*右邊界*</span> 位置可以變化
- <code>UIViewAutoresizing<span style="color:Crimson">FlexibleTopMargin</span></code> - 指定自己的 <span style="color:Crimson">*頂部邊界*</span> 位置可以變化
- <code>UIViewAutoresizing<span style="color:Crimson">FlexibleHeight</span></code> - 指定自己的 <span style="color:Crimson">*高度*</span> 可以變化
- <code>UIViewAutoresizing<span style="color:Crimson">FlexibleBottomMargin</span></code> - 指定自己的 <span style="color:Crimson">*底部邊界*</span> 位置可以變化

#### 使用範例

``` objc
UIView *demoView = [[UIView alloc] initWithFrame:CGRectMake(10,10,10,10)];
demoView.autoresizeingMask = UIViewAutoresizingFlexibleWidth | UIViewAutoresizingFlexibleHeight;
```

## Auto Layout 以及 Manual Layout

在 subclass view 的時候，通常會用 `layoutSubviews` 來幫自己所屬的 subviews 來排列位置，當 `setNeedsLayout` 的時候也會呼叫這個 method。

在這裡 subview layout 方法就大致可以分成手動和自動：

自動的作法就是以各自 subviews 所設定的 autoresizingMask 來變化，當畫面的 bounds 改變的時候，就會自己做相對應的改變。

手動的方法則是可以覆寫 layoutSubviews 這個 method ，在裡面針對畫面變化之後，手動計算所屬的 subviews 該怎麼擺，並由 `setNeedsLayout` 觸發這些重新 layout 的動作。因此手動 layout 也適合用在當自動縮放無法做到的一些複雜縮放。

## 結論

對 auto layout 開始比較有印象是在 Xcode 5 出來的時候，當時在 interface builder 多了個 auto layout 的設定。不過當初都是用 code 來對 view 去縮放，因此也沒有太深入去了解，也不清楚這個功能其實在 iOS 2.0 就已經存在了。

而手動和自動佈局各有優缺點，有時候自動佈局無法掌控他要怎麼縮放，就要用手動去設定 frame 的位置。因此該使用哪種方式讓畫面去做移動以及縮放，就要看各專案及畫面的需求是最方便的。

若使用手動佈局的話，則別忘了也要呼叫 `setNeedsLayout` 來觸發重新排版。

*本篇文章撰寫時使用 iOS SDK 8.0, Xcode 6.0.1 on Mac OSX 10.10 Preview 7 1.0 ，如果發現相關內容有更新或不適用，請在下方留言，謝謝。*
