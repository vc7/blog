---
layout: post
title: "使用 CocoaPods 以及 Rakefile 協助開發"
date: 2014-07-29 15:30:02 +0800
comments: true
categories: ["iOS", "CocoaPods", "Rakefile"]
keywords: iOS, CocoaPods, workflow, rakefile, automatic
description: "這個星期新開一個專案，決定用我自己之前規劃好卻一直沒有實踐的完整過的開發方式，用到的工具有 Rakefile 和 Cocoa/Cocoa Touch 開發會用到的 CocoaPods 這兩個。"
references:
hero: "https://farm4.staticflickr.com/3913/14772587684_fd035bc69d_o.png"
---

這個星期新開一個專案，決定用我自己之前規劃好卻一直沒有實踐的完整過的開發方式，用到的工具有 Rakefile 和 Cocoa/Cocoa Touch 開發會用到的 CocoaPods 這兩個。

<!-- more -->

## 簡介

### Rakefile

[http://docs.seattlerb.org/rake/](http://docs.seattlerb.org/rake/)

從我之前寫的一篇文章： [讓 Rakefile 幫你做完一些雜事]({{ site.url }}/2014/06/07/let-rakefile-helps-you/) 可以一窺 Rakefile 可以怎麼用。

### CocoPods

[http://cocoapods.org/](http://cocoapods.org/)

而 CocoaPods 則是給 iOS/OS X 專案使用的套件管理工具，基本上提供了比單單使用 git 的 submodule 更完善的套件管理功能。除了可以上他們的 [Spec](https://github.com/CocoaPods/Specs/tree/master/Specs) repo 查看有什麼第三方 libraries 可以用之外，也可以自己建立自己的 private Spec repo 來使用。除了這些建立集中管理的 Spec repo ，當然也可以把尚未開發完成的 repo 留在本機或 git server 上使用也可以。

## 工作流程

今天要做兩件事情

1. 我要很方便地用我自己分離出的 libraries ，但是我的 libraries 本身可能是尚未完成需要頻繁的修改及試驗功能。
2. 我有第三方服務的 API Keys ，這是不能暴露並記錄在 git 中的 

### 靈活使用 CocoaPods

在做 iOS 專案的時候會想要把各自有明確責任的區塊切出去成一個 static library ，在過去的做法就是：另外有一個 git repository 。但是這樣我要使用未完成 library 的時候可能要先 commit changes，再從開發中的專案去 pull 修改下來試用；或是直接對專案拉進來的 subproject 修改。兩者都不是非常理想的作法。

#### 主專案

所幸 CocoaPods 很方便，可以讓開發者直接使用本地或遠端 git server 上的 pod ，把 library 加好 podspec 檔之後，在主專案的 Podfile 中指定路徑或 git repository 的網址就可以找到該本地 pod ：

``` ruby
# 本地
pod 'Name', :path => '../' # 相對路徑或絕對路徑都可以

# git server
pod 'Name', :git => 'https://github.com/username/Name'
```

只要 podspec 撰寫沒有問題的話，在 terminal 執行 `pod install` 就都會通過並安裝完畢。

### 應用 Rakefile

第二個部分主要針對做自動化和隱藏重要訊息的處理：自動安裝 pods 以及隱藏不該記錄起來的 api key ，在公開的 repo 中記錄下來並可被查看的話就非常危險了。

#### Template Files

因此以 API Key 等資訊，可以放在 Defines 檔中，並把這一組 Defines 檔（ .h, .m ）加入 .gitignore 中。這些檔案需要加入 Xcode project 中

接著另外建立一組 DefinesExample 檔案作為 template 檔案，不用拉進 Xcode project 中。在 Rakefile 中，則將這一組 template 檔案複製並命名成 Defines，執行完相關指令就可以使用了。

#### 範例

這個是我參考 *[nothingmagical/cheddar-ios](https://github.com/nothingmagical/cheddar-ios/blob/master/Rakefile)* 所做的最基本的 Rakefile ，主要幫我做完上述我想做的兩件事。直接執行 `rake` 或是 `rake setup` 就可以幫我做完這兩件事情了。

``` ruby
desc 'CocoaPod related'
task :pod_install do
  puts 'Installing pods...'
  `pod install`
end

desc 'Setup with example files'
task :setup => :pod_install do
  # Copy examples defines
  puts 'Copying example MYDefines into place...'
  `cp MYProject/MYDefinesExample.h MYProject/MYDefines.h`
  `cp MYProject/MYDefinesExample.m MYProject/MYDefines.m`

  # Done!
  puts 'Done! You\'re ready to get started!'
end

# Run setup by default
task :default => :setup
```

當然，如果有其他的需求，可以再自己加上及串接一些想要做自動化的 rake tasks：如執行單元測試、更新 git submodule 等等。

## 結論

加入這兩個做法相對於原先的做法，基本上很容易的就可以加快開發的速度，也可以讓一些做法更加乾淨而且方便。沒有這樣做過的朋友不妨可以試試。