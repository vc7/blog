---
layout: post
title: "SVN 歷險記 - Xcode + SVN 之 ignore"
date: 2014-09-25 23:23:12 +0800
comments: true
categories: ['iOS', 'SVN', 'SVN 歷險記']
keywords: iOS, SVN, SVN 歷險記, ignore, project settings, 專案設定, iOS 開發, 初始設定
description: "因為工作上需要，所以必須了解怎麼使用 svn 怎麼和 Xcode 相處得宜。因為 Xcode 有些檔案是不需要被 commit 進 repository 的，於是就來看看怎麼在 svn 達到和 gitignore 一樣的效果。一探究竟之後，也發現和 git 的做法截然不同。"
references: [{title: "Subversion Properties - svnbook", link: "http://svnbook.red-bean.com/en/1.7/svn.ref.properties.html"}, {title: "Getting svn to ignore files and directories
 - Superchlorine", link: "http://superchlorine.com/2013/08/getting-svn-to-ignore-files-and-directories/"}]
hero: "https://farm4.staticflickr.com/3901/15165651677_6aa12dbf2c_o.png"
---

因為工作上需要，所以必須了解怎麼使用 svn 怎麼和 Xcode 相處得宜。因為 Xcode 有些檔案是不需要被 commit 進 repository 的，於是就來看看怎麼在 svn 達到和 gitignore 一樣的效果。一探究竟之後，也發現和 git 的做法截然不同。

<!-- more -->

## Property 和 Subversion Properties

### Property

Subversion 對於 **每個檔案** 、 **每個資料夾** ，甚至是 **unversioned files** 都可以加上 property 加以設定。相關指令如下：

``` sh
$ svn propset  # 初始化一個 property
$ svn propget  # 取得一個 property
$ svn propedit # 編輯一個 property
$ svn propdel  # 刪除一個 property
$ svn proplist # 列出指定 item 的 properties
```

### Subversion Properties

而 SVN 也有自己內建的 Subversion properties 可以使用，都是以 `svn:` 作為開頭，再接著需要操作的 property name 。

今天會用到的 ignore 就是隸屬於 Subversion properties ： `svn:ignore` 。把需要忽略的檔案或資料夾設定在這個 property 裡後，就可以達到目的了。

## svn:ignore

在這裡做點記錄，看看要怎麼設定這個屬性。

### 初始化

如前一個段落提到的，使用 `propset` 這個 subcommand 就可以達成了：

``` sh
# 首先需要先進入你的 svn 的專案資料夾
# svn propset [property] [target] [location]
$ svn propset svn:ignore bin .
```

這個指令做的事情是

> 把目前目錄（`.`）中的 `bin` 資料夾設定為需要 ignore 的對象

### 影響範圍與遞迴設定 *-R*

`svn:ignore` 這個參數只能對指定對象的資料夾中做處理，並沒有辦法直接遞迴到所有的子目錄的子目錄的子目錄 ... 。所以若是在 ignore 的對象中有 `/` 有資料夾層級的斜線，將不會找到正確的目標。

因此若是需要把所有的子目錄的層級，全部設定一樣的 ignore 條件，則可以在指令加上 `-R` 這個 flag ：

``` sh
$ svn propset svn:ignore -R bin .
```

這樣一來，所有在自己目錄項下的所有子目錄及他們所有的子目錄，會全部加上 ignore `bin` 這個資料夾的 `svn:ignore` property

### 集中管理 Ignore File Settings ： *-F* 與 *.svnignore*

但是如果照上述的做法，我必須要下 `propedit` 這個副指令才有辦法看到我到底 ignore 了哪些檔案，而 svn 也不像 git 有 .gitignore 會自動讀取，非常不方便。

不過其實這個也是有對應的作法可以做，因此我們就來仿照 .gitignore 就來造個 .svnignore 檔案來列出需要 ignore 的項目吧！

而 propset 也有 flag 可以讓大家可以直接讀取使用者自定義的檔案，並根據檔案內容來覺得什麼檔案要被 ignore 。

因此，在 .svnignore 中可以長這樣，每一個新的一行就是一個需要忽略的檔案或是資料夾：

``` text
bin
photo
```

在執行初始化的時候，到目前為止則變化成如下：

``` sh
$ svn propset svn:ignore -R -F .svnignore .
```

就可以以此把 .svnignore 裡面的設定指定到所有子目錄及子檔案中。

### <span style="color:Crimson;">注意事項</span>

svn 會善盡職責好好管理任何已經被版本控制的檔案，因此當設定 ignore 之前，有些檔案已經受到版本控制，事後就算把那些檔案設定進 `svn:ignore` 也 <span style="color:Crimson;font-width:bold;">絕對、絕對、絕對不會</span> 將那些檔案解除 version control 。

## 和 Xcode 合作

由於已經被 commit 過加入 version control 的檔案在事後並沒有辦法被移除，因此在專案生成之後先不要做任何的 commit ，先對專案所有的資料夾遞迴設定 `svn:ignore` ，再來 commit 專案的資料夾。

### .svnignore Template

由於設定 ignore 條件的方式和 git 有所不同，因此貼出平常使用的 .svnignore 檔案供參考：

``` sh
.DS_Store
xcuserdata
build
*.mode1v3
*.pbxuser
*.xcworkspace
*.moved-aside
DerivedData

# If you are using CocoaPods
Pods
Podfile.lock
```

這時候再執行設定 `svn:ignore` ，也就會發現他真的會走過所有的檔案以及資料夾全部設定好。

## 結語

和我自己之前學習 git 一樣，目前為止都是盡量用 command line 直接操作 svn ，避免工具幫我在背後做掉很多事情和學習工具的一些成本。在公司用 [Versions](http://versionsapp.com/) 嘗試時也沒有找到該怎麼遞迴跑所有子目錄，不過也是可以透過它來輔助快速驗證是否設定成功。

現在多看一陣子 svn 、多瞭解一下發現似乎也沒有當初想的那麼麻煩和恐怖，還是有許多需要看就對了。





