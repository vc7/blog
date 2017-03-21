---
layout: post
title: "Mobile Safari 的幾個特有 tags"
date: 2014-07-07 13:29:11 +0800
comments: true
categories: ["Web", "Mobile Web"]
keywords: Web
description: "自從 iPhone 出來之後， mobile web 越發盛行，而 Safari 自己也提供自有的 meta tags ，可以讓開發者使用，在 iPhone 上瀏覽網站時才有更多變化，或更符合 iOS 的 Design Guidelines。"
references: [{title: "Supported Meta Tags - Safari HTML References", link: "https://developer.apple.com/library/safari/documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html#//apple_ref/doc/uid/TP40008193-SW1"}]
hero: "https://farm6.staticflickr.com/5546/14594374775_53385bac6e_o.png"
---

自從 iPhone 出來之後， mobile web 越發盛行，而 Safari 自己也提供自有的 meta tags ，可以讓開發者使用，在 iPhone 上瀏覽網站時才有更多變化，或更符合 iOS 的 Design Guidelines。

<!-- more -->

這裡先不會提到 viewport 的設定，因為篇幅會比較長，這裡就把 viewport 以外的 meta 講一次。

## 這些 Meta Tags 是要做什麼用？

這些 meta 主要可以設定你的網頁在 Mobile Safari 上的時候，可以讓 Safari 知道在顯示網頁的時候、把網頁加入 iPhone 主畫面之後開啟的時候，可以為你做什麼事情。

當然， Apple 也提供了其他自有的參數可以讓你可以設定加入 iPhone home screen 時，可以設定自己的 app icon 、 launch images 等，可以讓你的 web app 看起來真的很像是一個 native app ；甚至還可以加入 App Store affiliate ，可以讓使用者直接在網頁裡直接可以導向到 App Store，下載你自己的 app 。這個部分會再另外寫一篇文章教學，就不在這邊說了。

## Apple 提供的特有 Meta Tag Keys

從 [文件](https://developer.apple.com/library/safari/documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html#//apple_ref/doc/uid/TP40008193-SW1) 中看到，除了 viewport 之外，還有三個 tags 可以用：

- apple-mobile-web-app-capable
- apple-mobile-web-app-status-bar-style
- format-detection

### apple-mobile-web-app-capable

#### 用法

加在 header 標籤之間

``` html
<meta name="apple-mobile-web-app-capable" content="yes">
```

#### 參數

當 content 設定為 `yes` 的時候，會進入全螢幕模式，不會顯示 Mobile Safari 的 navigation bar。

設定為 `no` 的時候：

{% img center https://farm6.staticflickr.com/5569/14614077633_66aa982802.jpg %}

設定為 `yes` 的時候，從 home screen 打開時就可以把網址列藏起來：

{% img center https://farm4.staticflickr.com/3851/14407455448_01b5001bd2.jpg %}

但是這時候，狀態列的字都是黑色的，會都看不到，設定下一個 tag 就可以讓時間變成白色的顯示出來了。

### apple-mobile-web-app-status-bar-style

這個 tag key 依賴於上一個參數 `apple-mobile-web-app-capable`，當沒有設定成全螢幕的話，這個 tag 是不會有作用的。

#### 用法

加在 header 標籤之間

``` html
<meta name="apple-mobile-web-app-status-bar-style" content="black">
```

#### 參數

在這裡 content 可以有三個值： `default` 、 `black` 以及 `black-translucent`。

##### defualt

設定成 default 的話，status bar 的字樣則不會顯示出來，就像上一節的最後一張圖一樣。

##### black

設定成 black 就會在 status bar 加上一個黑色的背景，讓 status bar 的字變成白色顯示出來：

{% img center https://farm4.staticflickr.com/3925/14407573969_934800309a.jpg %}

##### black-translucent

這個參數設定下去，就會讓網頁的畫面，從手機畫面左上角 (0, 0) 開始畫，所以 status bar 就會蓋在網頁的畫面上，如下圖：

{% img center https://farm4.staticflickr.com/3895/14407540240_e72ff3ce3d.jpg %}

這個圖出來的之後，應該就有人會直覺想到這不就是 iOS 7 開始之後對 native app 中的 view 座標移到 (0, 0) 一樣的做法嗎？

因此應用上可以把 status bar style 的樣式設定為 `black-translucent` ，加一些 padding-top 或 margin-top 把圖中的 nav-bar 移動一下，就可以自訂 status bar 的背景。

### format-detection

#### 用法

加在 header 標籤之間

``` html
<meta name="format-detection" content="telephone=no">
```

#### 參數

在預設的行為中， Mobile Safari 會去讀內容，並判定網頁中的數字有沒有需要轉成可以用它來打電話的超連結，點擊進而打開電話的 app 撥出。

設定成 `telephone=no` 就可以讓 Mobile Safari 不要去解析網頁中的數字，就不會讓網頁上的一些呈現出來的數據變成電話號碼，造成不好的使用者經驗。

## 結語

很可惜的，在 status bar 的部分，沒有辦法在 translucent 模式下讓字變成黑色，這樣網站淺背景的話，就可以搭配黑色的狀態文字就可以比較清楚了。

透過這些方法，可以讓 web app 在使用者把網頁加入 home screen 之後，加上一些變化，並屏除像網址列之類的干擾，讓使用者更能專注於網站的內容。

*本篇文章撰寫時 iPhone 版本為 iOS 7.1 ，不同版本畫面可能會有所不同*

*本篇文章撰寫時基於 2013-10-22 最後更新的 Safari Documentation 版本，如果發現相關內容有更新，請在下方留言，謝謝。*