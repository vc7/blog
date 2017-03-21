---
layout: post
title: "Sass 起手式"
date: 2014-07-05 14:19:15 +0800
comments: true
categories: ["Web", "CSS", "Sass", "Less"]
keywords: Web, CSS, Sass, Less, Programmable CSS, F2E
description: "最近在整理自己的部落格樣式， Octopress/Jekyll 是用 Sass ； 開始學 Rails ，其中也是用 Sass ，因此讓我開始看 Sass 到底是怎麼一回事。"
references: []
hero: "https://farm6.staticflickr.com/5576/14576708662_bd74e86ce2_o.png"
---

最近在整理自己的部落格樣式， [Octopress](http://octopress.org/)/[Jekyll](http://jekyllrb.com/) 是用 [Sass](http://sass-lang.com/) ； 開始學 Rails ，其中也是用 Sass ，因此讓我開始看 Sass 到底是怎麼一回事。 

<!-- more -->

## 我是 Less 元開發者 

其實我不是對結構化的 CSS 是新手了。2012 年還在學校做一些校方活動網站，甚至後來到職場上，都是用 [Less](http://lesscss.org/) 來撰寫我的 CSS 樣式。會用的原因是，可以很方便、架構化乾淨的寫我的樣式，而不像是像 raw CSS 一樣，需要重複撰寫，樣式邏輯也不好維護。

接著知道 Sass 這個東西大約是在 2013 年的 WebConf 之前沒多久，在 WebConf 中也聽到 [Even Wu](http://blog.evendesign.tw/) 在介紹 Sass 這個工具。當時的想法是 Less 跑得開開心心，沒有大問題，因此沒有對 Sass 深入探討。

而當時看 compile Sass 的工具也都要錢。沒有接觸 Ruby/Rails ，也不太知道自動化的流程可以怎麼做，所以就用 Less 官方提供的工具用下去。

另外持續用 Less 的其中一個很大的原因是， Twitter Bootstrap 2.* 為止還是用 Less 來作樣式。

## 轉變

直到今年 Bootstrap 3 出來，開始正式支援 Sass 。到目前也出了好久了，到最近因為又開始做網站前端相關的東西，所以又開始接觸這一塊前端的技術。

接著發現 Sass 的 features 比 Less 還多，是還滿重量級的工具，於是決定跳進來看這玩意兒到底是怎麼一回事，也開始挖他到底可以怎麼應用它的 command line 工具。可行的話在未來還可以自己寫個超輕量架構，幫我處理生成簡單的網站頁面。