---
layout: post
title: "我對 Swift 的看法"
date: 2014-06-30 15:17:46 +0800
comments: true
categories: ["iOS", "Swift", "easier"]
keywords: iOS, Swift
description: "自從 WWDC 2014 公開 Swift 這個語言之後，許多人問我對 Swift 的想法如何，在經過約一個月之後，我自己也大略得出了一些想法"
references: [{title: "", link: ""}]
hero: "https://farm4.staticflickr.com/3922/14518080296_9928a6faf4_o.png"
---

自從 WWDC 2014 公開 Swift 這個語言之後，許多人問我對 Swift 的想法如何。經過一個月不論國內外都翻雲覆雨的討論這個語言怎麼樣怎麼樣，也有人說你們寫 Objective-C 的人就沒工作了云云等等現象，我自己也大略得出了一些想法。

<!-- more -->

# 前言

{% pullquote %}

首先， Swift 之於 Objective-C 是相對上一個比較簡單的語言。再來，在 TIOBE Index 6 月的 Headline 提到，雖然 Swift 在六月還沒有辦法計算排行，但是{" 在七月的 index report，基本上會在前 20 名內 "}；所以 Swift 來勢洶洶。最後，蘋果的破壞性創新也不是史無前例，他們可以為了推 Swift 而宣布不支援 Objective-C 撰寫的 App 上傳到 App Store 來販賣。

{% endpullquote %}

# 我覺得 Swift 是什麼

我覺得他是自 Objective-C 從 Reference Counting 進化到 ARC (Automatic Reference Counting) ， Xcode 5 開始都預設開啟 ARC 之後，產生的更簡化的版本：我不太需要 C 的那些指標東西，我只是要 build 個 app 而已，我的 syntax 不要太複雜。

## 簡單的語言的風險

今天和主管在吃午餐的時候，聊到 Swift 是個很簡單的語言，我們都同意簡單的語言就有風險：更簡單的使用一個語言，更容易不知道他背後到底做了什麼事情。

這其實在開發的世界也不是毫無前例的，就像 PHP 到 Ruby on Rails 一樣，我要用 Rails 做一個網站更簡單、用套件更容易，因此更少人去探究這個套件到底幫你做了什麼事情。之前在面試 PHP 工程師的時候，甚至還有寫了好幾年，不知道什麼是 SQL injection 或是一些 PHP 基本的安全控管（因為有些 Framework 會做掉，但是很少人會去看文件）。

{% pullquote %}

因此得出的小結論是，有可能為 iOS 雖然創造了更大的開發者群眾，但是 {" Swift 反而可能會創造出很多 bad quality code "}。

我的這個想法其實也跟 ARC 剛出來的時候一樣：當 ARC 出來的時候，大家就直覺，我不用再控管記憶體了， memory usage 就可以不用去管他。但是這個想法是錯的，就算 ARC 會幫忙收集 non-retained memory ，並不代表可以不用控制 memory allocation 的用量。

{% endpullquote %}

## Swift 在軟體開發界非常洶湧

不可否認的， Swift 在軟體開發界投下一顆震撼彈，儼然在對所有的語言招手說快來用我開發。而他的 syntax 異常的簡單、異常的有活性， 而 Ruby Taiwan 在這個月也有辦 Swift 的課程講座。感覺這個月大家開口閉口都在講 Swift ，好成一股風氣。

> 那 Objective-C 什麼時候會掰掉？

其實也有人問我這個問題，一開始我覺得蘋果應該不會那麼容易讓他掰掰；但是現在一個月以來的風氣，我覺得至少兩年到三年，藉由 iOS, OSX 版本的迭代，把 Objective-C 的蹤跡清掉。而蘋果下一步應該就是把 Cocoa/CocoaTouch Framework 做成 Swift 版本。

最近台灣的開發者也有人反應寫 Swift 時，錯誤訊息卻是 NS-based 的 Objective-C 訊息，有點怪怪的。


# 所以 ?

我覺得，對於原有 Objective-C 的開發者而言，去學 Swift 應該不是什麼困難的事情。再加上 Swift 套用 Cocoa/CocoaTouch Framework 之後的撰寫行為，和原先的 Objective-C 其實相差不會太多，所以不需要太懼怕。

> 為了蘋果這個巨大的時代輪子，一定要學 Swift

不學的話，未來被其他語言跳入的開發者替代率極高。

再來就是要證明自己寫過 Objective-C 的價值是什麼（其實我也還在尋找如何證明），例如：比較好的記憶體管理觀念，或是比較好的架構能力等等。

不過接觸 Objective-C 快兩年以來，也剛好經歷這個大震盪的時期，我倒還是滿喜歡這個語言的，也讓我可以學到很多在普通寫網頁比較不會碰到的問題：如記憶體管理、執行緒管理、自行架構 Custome UI 等等。