---
layout: post
title: "開發東西真的要寫 Test 啊！"
date: 2014-07-28 23:30:01 +0800
comments: true
categories: ["Thinking", "Software Engineer"]
keywords: scrum, agile, qa, 自動化測試, monkey test, CI
description: "這幾天因為比較有時間，就開始認真看怎麼寫 test cases 。接著開始想到我自己以前的 code 都沒有寫過單元測試，真的是心臟很大顆。"
references:
hero: "https://farm4.staticflickr.com/3858/14774596022_09e8a7032c_o.png"
---

這幾天因為比較有時間，就開始認真看怎麼寫 test cases 。接著開始想到我自己以前的 code 都沒有寫過單元測試，真的是心臟很大顆。今天在試著用自己之前做的 library ，也是沒有通過，用的時候也是掛掉。

<!-- more -->

在過去待的都是新創公司，老闆對程式碼的品質其實沒有非常在意。從根本不會在意你寫得怎樣，你只要寫的出來可以動就好；到懶得做持續整合、「單元測試先不用寫啊」等等。於是乎有種新創公司都只重視有東西卻不重視程式碼品質的錯覺。

但是這好像跟 build team 的人和 team members 有直接關係，以及重不重視、熟不熟悉開發必要流程等等也有關係。

最近也從什麼都不懂，到可以該怎麼寫普通的測試、該怎麼寫非同步功能的測試。最近還聽到涵蓋率，還有測試應該還要寫包含例外處理的 test case 等等 ，還真的很多東西需要繼續學，也是寫程式幾年以來自己最弱、需要加強的部分。