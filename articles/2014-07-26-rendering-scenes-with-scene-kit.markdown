---
layout: post
title: "Scene Kit 中 Render 畫面的方法"
date: 2014-07-26 21:05:02 +0800
comments: true
categories: ["iOS", "Scene Kit", "OpenGL"]
keywords: iOS, Scene Kit, OpenGL
description: "在 Scene Kit 要把畫面 render 出來有三種方法，非常有彈性。他會幫我們在底層處理好並有效率的 render 畫面出來，並整合其他 frameworks 以及充分運用 GPU 的繪圖能力。
"
references: [{title: "WWDC 2012 Session 504 - Introduction to Scene Kit - Apple WWDC 2012", link: "https://developer.apple.com/devcenter/download.action?path=/videos/wwdc_2012__hd/session_504__introducing_scene_kit.mov"}, {title: "Layer Trees Reflect Different Aspects of the Animation State - Mac Developer Library", link: "https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CoreAnimation_guide/CoreAnimationBasics/CoreAnimationBasics.html#//apple_ref/doc/uid/TP40004514-CH2-SW19"}]
hero: "https://farm3.staticflickr.com/2933/14746500264_16a7d5b554_o.png"
---

在 Scene Kit 要把畫面 render 出來有三種方法，非常有彈性。他會幫我們在底層處理好並有效率的 render 畫面出來，並整合其他 frameworks 以及充分運用 GPU 的繪圖能力。

<!-- more -->

## 依據不同繪圖方法分責

- SCNView
- SCNLayer
- SCNRenderer

## SCNView

SCNView 是最簡單的方式，他直接可以存取 interface builder 。大大的簡化和 Cocoa/Cocoa Touch GUI 的整合過程。

## SCNLayer - Core Animation

SCNLayer 可以用於和 Core Animation 整合。這個 class 建立出來的 object 可以幫你把 3D 場景整合進 Core Animation 的 model layer tree ，由此可以更完整的利用 Core Animation 提供的 layout 和影像合成引擎。

## SCNRenderer - OpenGL

SCNRenderer 可以讓你 render 出多種低階的 OpenGL 場景。當不需要顯示圖像給使用者時，也可以做 offscreen rendering 。

