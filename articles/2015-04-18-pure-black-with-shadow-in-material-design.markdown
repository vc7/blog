---
layout: post
title: "Material Design 中的純黑背景與陰影"
date: 2015-04-18 00:12:14 +0800
comments: true
categories: ["UI/UX", "Material Design"] 
keywords: Material Design, UX, UI Design
description: "今天在 UI/UX 群裡面聊到 Google 的 Material Design 有純黑 (#000000) 背景出現時該怎麼呈現陰影。這也是之前在看的 guildline 的時候沒有注意過的問題，因此把它筆記下來。"
hero: "https://farm9.staticflickr.com/8717/16998230939_fe6d0360a1_o.png"
---

今天在 UI/UX 群裡面聊到 Google 的 Material Design 有純黑 (#000000) 背景出現時該怎麼呈現陰影。

這也是之前在看的 guildline 的時候沒有注意過的問題，因此把它筆記下來。

<!-- more -->

首先先放這部分的文件，Light and shadow：

Within the material environment, virtual lights illuminate the scene. Key lights create directional shadows, while ambient light creates soft shadows from all angles.

Shadows in the material environment are cast by these two light sources. In Android development, shadows occur when light sources are blocked by sheets of material at various positions along the z-axis. On the web, shadows are depicted by manipulating the y-axis only.

接著另一段是 Functional Shadow ：

Shadows provide important visual cues about objects’ depth and directional movement. They are the only visual cue indicating the amount of separation between surfaces. An object’s elevation determines the appearance of its shadow.


<!--  北京  布布 -->

接著群裡面有人提到其實在 MD 裡面純黑不應該存在的原因

- 有光源才會有陰影，只要有光源就不會出現「純黑」的底色
- 因為背景一定會被「光源」照亮
- 所以純黑背景，在 Material design 裡，不存在，是違反 Guideline 的用色

這麼說還滿合理的

討論一段之後，也有整理出另外的小結論

- 就算是想要陰影，他還是存在的，只是你看不到

最後還開玩笑說：

    對這種不講理的設計，你問他「為什麼要用純黑」，他只會回答你「我就是想要純黑」

不過這應該又是另外的故事了 XDD