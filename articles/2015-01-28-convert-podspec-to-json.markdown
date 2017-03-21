---
layout: post
title: "把 .podspec 轉換成 .podspec.json"
date: 2015-01-28 23:35:23 +0800
comments: true
categories: ['iOS', 'cocoapods'] 
keywords: 模組化, 相依性管理, CocoaPods
description: '最近在寫使用 CocoaPods 用的 library 時，要把 podspec 轉成 .podspec.json 格式，就上網找了找該怎麼轉。'
references: []
hero: 'https://farm9.staticflickr.com/8599/16201636640_a49b5b6055_o.png'
---

最近在寫使用 CocoaPods 用的 library 時，要把 podspec 轉成 .podspec.json 格式，就上網找了找該怎麼轉。

<!-- more -->

# pod ipc spec

找了找發現 pod 有這個功能可以用 &gt; `ipc` , inter-process communication ，加上 `spec` 之後，就可以轉換成 JSON 格式。其下還有其他的指令可以用，不過還是先專注在如何轉換到 JSON 格式即可。

以下的指令可以把 podspec 轉換成 JSON 格式後印出來：

``` sh	
$ pod ipc spec LibraryName.podspec
```

接著可以用 double greater than operator 來把印出來的 JSON 內容存成想要的檔案即可：

``` sh	
$ pod ipc spec LibraryName.podspec >> LibraryName.podspec.json
```

# 參考資料

- [How to convert podspec to podspec.json? (from console) - @azu_re](https://twitter.com/azu_re/status/474051332920209408)

## Qiita 同步發佈

- [.podspec を .podspec.json にコンバートしよう](http://qiita.com/vc7/items/7334c6fb728ba3af41a8)