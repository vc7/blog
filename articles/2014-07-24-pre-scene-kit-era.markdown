---
layout: post
title: "Scene Kit 前的世界"
date: 2014-07-24 20:12:11 +0800
comments: true
categories: ["iOS", "Scene Kit", "OpenGL"]
keywords: iOS, Scene Kit, OpenGL, Game Development, 遊戲開發
description: "在昨天的文章（初探 Scene Kit）提到基本上什麼是 Scene Kit ，他可以做什麼事情。那在這個 Framework 出來前，在 iOS 和 OSX 上要怎麼時做這些事情，用這篇文章簡單敘述一下。"
references: [{title: "WWDC 2012 Session 504 - Introduction to Scene Kit - Apple WWDC 2012", link: "https://developer.apple.com/devcenter/download.action?path=/videos/wwdc_2012__hd/session_504__introducing_scene_kit.mov"}]
hero: "https://farm4.staticflickr.com/3843/14547499549_8f32731424_o.png"
---

在昨天的文章（[初探 Scene Kit]({{ site.url }}/2014/07/23/scene-kit-a-glance/)）提到基本上什麼是 Scene Kit ，他可以做什麼事情。那在這個 Framework 出來前，在 iOS 和 OSX 上要怎麼時做這些事情，用這篇文章簡單敘述一下。

<!-- more -->

## Scene Kit

- 一個從 Moutain Lion 開始可以使用的一個新 Framework。
- 可以把 3D 更簡單的實作在 app 中。

## 常見使用 3D 的地方

- 3D 使用者介面
- 簡報，展示的畫面
- 資料可視化、科學上資料分析結果呈現
- 遊戲

## Scene Kit 想解決的問題

Scene Kit Framework 想解決的問題就是，在過去團隊要是不用第三方的遊戲軟體，就必須要學會許多艱深的電腦圖學知識，不只消耗時間也容易讓專案失敗。

### OpenGL 以及其他科技

講到圖學，當然會想到 OpenGL 。

他雖然是一個非常高效能的 rendering API 集合，相關的技術更有 GLSL 或是包裝起來的 GLKit ，以現有開發環境之下可說是非常豐富的技術支援。

但是其中各自有各種知識需要學習。想要達到一些簡單的事情，卻需要很多功夫去達成它。在 WWDC 2010 也有相當多的 sessions 在講 OpenGL ，教導開發者怎麼使用這些工具。

若不想要自己寫，上面還要自己再疊一層 OpenCV 做圖像處理；如果想要用到 scene graph 來架構場景的話， 也需要去找支援的 library 來做，甚至需要自己寫。總的以來，需要自己疊床架屋，非常耗時間，最後只好再買一套第三方軟體授權來用。

(GLSL： OpenGL Shading Language 是以 C 語言為基礎的高階 shading language 。可以比較容易得幫 OpenGL 裏的表面上色。之後也會有一篇文章說他可以怎麼用 :DDD )

## 解決問題

以目前為止，已經可以直接用 Scene Kit 就可以建立一個小小的遊戲（ [WWDC 2014 - Building a Game with SceneKit](http://devstreaming.apple.com/videos/wwdc/2014/610xxc04fgmv80x/610/610_hd_building_a_game_with_scenekit.mov?dl=1) ），基本上讓開發者省下很多麻煩。

使用原廠的 framework 基本上也代表可以得到比較完善的效能以及支援，也可以 **「直接」** 和 OpenGL/GLSL/GLKit/Core Animation 互動，可說是整合的非常好。

對需要做到第一小節提到的 3D 使用者介面、資料可視化等等功能，就可以以比較快速簡單的方法達成。若需要做到遊戲比較複雜的互動也是可以，但是就要端看個團隊對於新工具及跨平台的需求來評估了。

下一篇會講 scene graph 是什麼，以及 SceneKit 中的 scene graph 長什麼樣子。