---
layout: post
title: "Ruby 中變數的種類"
date: 2014-07-19 23:20:15 +0800
comments: true
categories: ["Web", "Ruby", "basic"]
keywords: web, ruby, basic
description: "Ruby 中的變數有四種，根據有無 prefix 和 prefix 種類不同，各種的作用域及功能都不一樣。"
references: [{title: "Variables and Constants - Wikibooks", link: "http://en.wikibooks.org/wiki/Ruby_Programming/Syntax/Variables_and_Constants"}, {title: "class variable - Wikipedia", link: "http://ja.wikipedia.org/wiki/%E3%82%AF%E3%83%A9%E3%82%B9%E5%A4%89%E6%95%B0"}]
hero: "https://farm4.staticflickr.com/3901/14691400965_04c1c536e6_o.png"
---

Ruby 中的變數有四種，根據有無 prefix 和 prefix 種類不同，各種的作用域及功能都不一樣。

<!-- more -->

- 無 prefix ：普通的 local variable ， scope 根據所在位置而定。
- 有 prefix
  - `@` ： instance variable ， based on its class ，無法由外部存取
  - `@@` ： class variable ，屬於 class 的 attribute
  - `$` ： global variable ，全域的變數。