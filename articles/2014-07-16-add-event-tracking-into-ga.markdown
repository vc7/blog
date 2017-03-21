---
layout: post
title: "在 Google Analytics 中追蹤事件"
date: 2014-07-16 23:50:15 +0800
comments: true
categories: ["Web", "Google Analytics"]
keywords: web, google analytics, user activities, ux
description: "在 Google Analytics 可以追蹤一些網頁中的事件，進而可以簡單對使用者行為模式分析，或是判定是否有達到目標的方法。"
references: [{title: "Setting Up Event Tracking - Google Analytics", link: "https://developers.google.com/analytics/devguides/collection/gajs/eventTrackerGuide#SettingUpEventTracking"}]
hero: "https://farm4.staticflickr.com/3882/14669729142_d28971ea43_o.png"
---

在 Google Analytics 可以追蹤一些網頁中的事件，進而可以簡單對使用者行為模式分析，或是判定是否有達到目標的方法。

<!-- more -->

原本的 code 長這樣：

``` js
var _gaq = _gaq || [];
_gaq.push(['_setAccount', 'YOUR_GA_ID']);
_gaq.push(['_trackPageview']);

(function() {
	var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
	// ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
	// Change to new way of ga
	ga.src = ('https:' == document.location.protocol ? 'https://' : 'http://') + 'stats.g.doubleclick.net/dc.js';
	var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
})();
```

這邊加完之後，其實可以發現有宣告一個 `_gaq` 變數來放 Google Analytics 的設定，目前就只有設定帳號和紀錄 pageviews 而已。

## 加入事件

假如要去看某個連結有多少人點，放在哪個位置比較會有人點的情形。就可以在 `onclick` 的事件觸發之後，發送 event 的訊息給 ga。

### Submit 事件的方法

所以這個時候也是要把 event 推進 `_gaq` 裡面去：

``` js
_gaq.push(['_trackEvent', CATEGORY_NAME, ACTION_NAME, LABEL_NAME, VALUE])
```

除了 `_trackEvent` 、 `CATEGORY_NAME` 和 `ACTION_NAME` 之外都是 optional 的。

### 實際使用

除了可以直接在 HTML inline 直接插入 onclick 事件，提出來寫也可以：

``` js
document.getElementById("ID").onclick = function() {
	_gaq.push(['_trackEvent', 'Kumaya Home Link', 'Click');
};
```

找到目標元件之後，設定 onclick 要做什麼事情，塞一個 function 給他、裡面再放要執行的 event 設定就可以了。

## 看報表

當設定完 event 之後，觸發事件當下，就可以在 ga 的頁面看到結果了。
