---
layout: post
title: "在 Terminal 裡面玩 Swift"
date: 2014-07-13 11:23:20 +0800
comments: true
categories: ["iOS", "Swift"]
keywords: iOS, Swift, beta, terminal
description: "在 Terminal 裡面開啟 Swift REPL。"
references: [{title: "How can I use swift in Terminal?", link: "http://stackoverflow.com/a/24011715"}]
hero: "https://farm6.staticflickr.com/5580/14636950161_3700a30f93_o.png"
---

根據 Xcode 6 beta 發布的訊息， Swift 也有和其他語言一樣可以在 terminal 開啟 [REPL](http://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop) 工具可以用。

<!-- more -->

{% blockquote %}

Command Line

Xcode’s debugger includes an interactive version of the Swift language, known as the REPL (Read-Eval-Print-Loop). Use Swift syntax to evaluate and interact with your running app or write new code in a script-like environment. The REPL is available from within LLDB in Xcode’s console, or from Terminal.

{% endblockquote %}

## 怎麼開

``` sh
sudo xcode-select -s /Applications/Xcode6-Beta.app/Contents/Developer/
```

那在 `Xcode6-Beta.app` 的地方，可以根據 Beta 版本不同改成對應的加上 2 或現在的 3 。

執行結束後，接著輸入

``` sh
xcrun swift
```

enter 之後會 就會 prompt 訊息出來，就可以開始用了：

``` sh
Welcome to Swift!  Type :help for assistance.
  1> 
```

Have fun!