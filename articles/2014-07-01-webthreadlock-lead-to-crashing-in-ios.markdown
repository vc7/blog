---
layout: post
title: "在 iOS 中避免 WebThreadLock 造成 Crash"
date: 2014-07-01 09:04:22 +0800
comments: true
categories: ["iOS", "thread", "webview"]
keywords: iOS, thread, webview, webthreadlock, ui
description: "UITableView 中的 UIWebView 重載時導致 app crash"
references: [{title: "", link: ""}]
hero: "https://farm3.staticflickr.com/2910/14545647054_c3ec196001_o.png"
---

最近的專案中，需要在 `UITableView` 中放 `UIWebView` ，web view 本身會有 web thread lock，因為他在 reload 之後是 UI 的 re-render，會被要求一定要在 main thread 上面跑。

<!-- more -->

> UI 繪製一定要在 main thread 上

```objc
[webView reload];
```

但是當 web view 要 reload 時， table view 也正在 reload，web view reload 所使用的 `WebThreadLock` 就會被推到 secondary thread ，就會導致 crash 而跳出以下的錯誤訊息：

```text

 bool _WebTryThreadLock(bool), 0x115504830: Tried to obtain the web lock from a thread other than the main thread or the web thread. This may be a result of calling to UIKit from a secondary thread. Crashing now...
1   0x108412aa8 WebThreadLock
2   0x102579e77 -[UIWebView reload]

```

## 解法

強制讓 `[webView reload]` 一定要在 main thread 即可解決， call method `performSelectorOnMainThread:withObject:waitUntilDone:` 指定要在 main thread 上執行的 method：

```objc
[webView performSelectorOnMainThread:@selector(reload) withObject:nil waitUntilDone:NO];
```