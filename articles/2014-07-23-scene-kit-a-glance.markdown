---
layout: post
title: "初探 Scene Kit"
date: 2014-07-23 20:12:11 +0800
comments: true
categories: ["iOS", "Scene Kit", "OpenGL"]
keywords: iOS, Scene Kit, OpenGL, Game Development, 遊戲開發
description: "Scene Kit Framework 是在 WWDC 2012 所發佈的一個 framework ，他是架構在 OpenGL 上層的高階 API ，用來更方便操控場景物件。在今年 WWDC 2014 宣布開始可以在 iOS 8 上使用。"
references: [{title: "Introduction to Scene Kit - Apple Document", link: "https://developer.apple.com/library/mac/documentation/3DDrawing/Conceptual/SceneKit_PG/Introduction/Introduction.html"}]
hero: "https://farm4.staticflickr.com/3878/14539941748_f0dc9b540e_o.png"
---

Scene Kit Framework 是在 WWDC 2012 所發佈的一個 framework ，他是架構在 OpenGL 上層的高階 API ，用來更方便操控場景物件。在今年 WWDC 2014 宣布開始可以在 iOS 8 上使用。

<!-- more -->

## 他可以做什麼？

- 可以載入主要軟體支援的 COLLADA 交換格式，副檔名為 .dae 。 （ [COLLADA 是什麼]({{ site.url }}/2014/07/18/collada/) ）
- 匯入 COLLADA 檔案的物件，內含場景、攝影機、燈光以及構成物體的面。
- 操控場景中物件的包覆空間（ Bounding Volume ）、表面及材質。
- 對 3D 物件加入即時互動
- 可以把匯入的物件和的 Core Animation 以及 GLKit 等 Cocoa/Cocoa Touch 的 Framework 做整合應用。
- 用 Xcode 就可以預覽和調整 DAE 檔案，以便整合進 app 中。

## 架構

-> {% img https://farm3.staticflickr.com/2928/14722839661_d81768aef9_o.png 500 %} <-

Scene Kit 在繪圖架構中是在 OpenGL 和 Cocoa/Cocoa Touch 之間，和 Core Animation 、 GLKit 和 Quartz 則在同一層中。

另外， Scene Kit 和 Image Kit 和 Core Animation 有做緊密整合，因此在使用時有時甚至可以不用知道 3D 相關圖學知識就可以實作。

## Workflow

在 Scene Kit 出來之後，可行的工作流程如下圖：

-> {% img https://farm6.staticflickr.com/5589/14539833448_56d9b0eef2_o.png %} <-

在 Design Team 的部分，設計師們可以用 Maya 等軟體做出 app 中會用到的模型、場景等素材，並匯出 DAE 檔案供 Engineer Team 使用。

Engineer Team 拿到這些檔案之後，就可以用 Scene Kit 把拿到的 DAE 檔案匯入 app 中，最後則產出一個 app 上架。

## 結論

Scene Kit 出來之後，可以讓團隊減少不同團隊之間整合內容所需要花的時間。而 Scene Kit 也提供與原有 frameworks 之間最緊密、最直接的整合，效能及工作效率也會比過去提升許多。

目前 Scene Kit 雖然在 Interface Builder 中有提供相對應的調整工具可以使用，但是以目前還沒有那麼強大的場景編輯的前提之下，應該還沒有到可以替代 Unity 這一類的第三方遊戲引擎工具的地步。但是對於不需要遊戲引擎這麼複雜的開發團隊來說，這個工具就已經是足夠了。