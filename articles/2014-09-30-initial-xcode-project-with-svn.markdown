---
layout: post
title: "SVN 歷險記 - 在 SVN 版控下 Initial 一個 Xcode Project"
date: 2014-09-30 22:19:12 +0800
comments: true
categories: ['iOS', 'SVN', 'SVN 歷險記']
keywords: iOS, SVN, SVN 歷險記, ignore, project settings, 專案設定, iOS 開發, 初始設定
description: "前幾天寫了 SVN 歷險記的第一篇，雖然順利解決怎麼為專案加上 svn ignore ，但是還是發現一些問題，這篇做了解決這些問題的筆記，並真正建立一個可以使用的 Xcode Project。"
references: [{title: "Ignoring Unversioned Items - svnbook", link: "http://svnbook.red-bean.com/en/1.5/svn.advanced.props.special.ignore.html"}, {title: "How to svn ignore a file already added to the repository? - Stack Overflow", link: "http://stackoverflow.com/questions/4978381/how-to-svn-ignore-a-file-already-added-to-the-repository#comment18774759_4978398"}]
hero: "https://farm4.staticflickr.com/3857/15402654715_78a0bb18bb_o.png"
---

前幾天寫了 SVN 歷險記的第一篇「[SVN 歷險記 - Xcode + SVN 之 ignore]({{site.url}}/2014/09/25/using-svn-ignore-with-xcode/) 」，雖然順利解決怎麼為專案加上 svn ignore ，這幾天下來發現：

> SVN 並沒有辦法 ignore unversioned 檔案及目錄及其子項目

之前在寫文章的時候不知道為什麼沒有碰到這個問題，於是馬上就朝了相關的方向尋找線索，這篇一樣會以 command line 為主，必要時 [Versions](http://versionsapp.com/) 作為視覺輔助。

<!-- more -->

## 問題 與 解決方案

在建立一個新專案的時候，所有相關的檔案都會處於 "unversioned" 的狀態，在 svn 的符號就是 "`?`" ，問號。在未加入版本控制（符號變為 "`A`" ）之前，並沒有辦法把檔案加上 `svn:ignore` 這個 property 。

### 解決方案

解決方案就是

- 先把所有檔案都加入 repository 
- 設定 ignore property 之後
- 再將 ignore files/folders 從 repository 中移除即立刻無懸念

## 範例動手做

以下範例可以自行上 [code.google.com](http://code.google.com) 註冊一個免費的 svn project 來做練習。

開始前請記得把 repository checkout 到本機。

### 步驟 1 - 創建 .svnignore

首先在 trunk 目錄之下，創建一個 .svnignore 來設定 Xcode 專案哪些檔案需要 ignore 。詳細一樣可以看[這篇文章]({{site.url}}/2014/09/25/using-svn-ignore-with-xcode/)。

.svnignore 檔案的內容則如下：

``` text
.DS_Store
build
*.pbxuser
*.mode1v3
*.mode2v3
*.perspectivev3
*.xcworkspace
xcuserdata
profile
*.moved-aside
DerivedData
.idea
```

### 步驟 2 - 創建 Xcode 專案並加入 SVN

在新增 Xcode 專案的時候記得將建立本地 git repo 的選項取消勾選，這時候 Xcode 會在 svn repo 的目錄底下再創建另外一個資料夾，如果想要把 repo 根目錄當成 project 的根目錄可以自行移動檔案。

這時候再 svn repo 的跟目錄下指令

``` sh
$ svn status
```

就可以列出目前 svn 的狀態：

``` text
?       demo-project
?       demo-project.xcodeproj
?       demo-projectTests
```

雖然一些需要 ignore 的檔案都是包含在裡面的，但是就先豪邁地把所有檔案加入 version control 中：

``` sh
$ svn add demo-project*
```

這樣就可以把任何以 project name 開頭的所有檔案及目錄加入 svn repository 成為 `A` 的狀態：

``` sh
A         demo-project
A         demo-project/AppDelegate.h
A         demo-project/Images.xcassets
A         demo-project/Images.xcassets/AppIcon.appiconset
A         demo-project/Images.xcassets/AppIcon.appiconset/Contents.json
A         demo-project/ViewController.h
A         demo-project/Info.plist
A         demo-project/AppDelegate.m
A         demo-project/ViewController.m
A         demo-project/Base.lproj
A         demo-project/Base.lproj/LaunchScreen.xib
A         demo-project/Base.lproj/Main.storyboard
A         demo-project/main.m
A         demo-project.xcodeproj
A         demo-project.xcodeproj/xcuserdata
A         demo-project.xcodeproj/xcuserdata/vincent.xcuserdatad
A         demo-project.xcodeproj/xcuserdata/vincent.xcuserdatad/xcschemes
A         demo-project.xcodeproj/xcuserdata/vincent.xcuserdatad/xcschemes/demo-project.xcscheme
A         demo-project.xcodeproj/xcuserdata/vincent.xcuserdatad/xcschemes/xcschememanagement.plist
A         demo-project.xcodeproj/project.pbxproj
A         demo-project.xcodeproj/project.xcworkspace
A         demo-project.xcodeproj/project.xcworkspace/xcshareddata
A         demo-project.xcodeproj/project.xcworkspace/xcshareddata/demo-project.xccheckout
A         demo-project.xcodeproj/project.xcworkspace/contents.xcworkspacedata
A         demo-project.xcodeproj/project.xcworkspace/xcuserdata
A         demo-project.xcodeproj/project.xcworkspace/xcuserdata/vincent.xcuserdatad
A  (bin)  demo-project.xcodeproj/project.xcworkspace/xcuserdata/vincent.xcuserdatad/UserInterfaceState.xcuserstate
A         demo-projectTests
A         demo-projectTests/externals_uiTests.m
A         demo-projectTests/Info.plist
```

這時候只是將檔案加入 repository 中，尚未 commit 。這時候可以發現上方回應的訊息 Line 15~19 的資料夾 `xcuserdata` 和 Line 21~27 的 `*.xcworkspace` 都是要 ignore 的資料夾。

### 步驟 3 - 設定 Ignore Property

這時候就先把 svn:ignore 設定好（詳細還是看[這篇文章]({{site.url}}/2014/09/25/using-svn-ignore-with-xcode/)吧 :D）：

``` sh
$ svn propset svn:ignore -R -F .svnignore .
```

這時候 console 的目錄還是要在 repo 的根目錄之下，接著可以在 terminal 看到以下的處理結果訊息：

``` sh
property 'svn:ignore' set on '.'
property 'svn:ignore' set on 'demo-project.xcodeproj'
property 'svn:ignore' set on 'demo-project.xcodeproj/xcuserdata'
property 'svn:ignore' set on 'demo-project.xcodeproj/xcuserdata/vincent.xcuserdatad'
property 'svn:ignore' set on 'demo-project.xcodeproj/xcuserdata/vincent.xcuserdatad/xcschemes'
property 'svn:ignore' set on 'demo-project.xcodeproj/project.xcworkspace'
property 'svn:ignore' set on 'demo-project.xcodeproj/project.xcworkspace/xcuserdata'
property 'svn:ignore' set on 'demo-project.xcodeproj/project.xcworkspace/xcuserdata/vincent.xcuserdatad'
property 'svn:ignore' set on 'demo-project.xcodeproj/project.xcworkspace/xcshareddata'
property 'svn:ignore' set on 'demo-project'
property 'svn:ignore' set on 'demo-project/Base.lproj'
property 'svn:ignore' set on 'demo-project/Images.xcassets'
property 'svn:ignore' set on 'demo-project/Images.xcassets/AppIcon.appiconset'
property 'svn:ignore' set on 'demo-projectTests'
```

因為有加上 `-R` ，接著很豪邁的洗了一圈，所有檔案都設定好 `svn:ignore` 了。

### 步驟 4 - 移除不要的檔案及目錄

這時候就可以用 svn 的 delete 指令來將指定的目錄解除 svn 對他們的控制。

由於只是要移出 repository ，檔案並不需要刪除，因此可以加上 --keep-local 就可以把本地的檔案留下來。

``` sh
$ svn delete --keep-local demo-project.xcodeproj/project.xcworkspace/
$ svn delete --keep-local demo-project.xcodeproj/xcuserdata/
```

這兩個指令分別會得到以下的回應訊息：

``` sh
D         demo-project.xcodeproj/project.xcworkspace
D         demo-project.xcodeproj/project.xcworkspace/contents.xcworkspacedata
D         demo-project.xcodeproj/project.xcworkspace/xcshareddata
D         demo-project.xcodeproj/project.xcworkspace/xcshareddata/demo-project.xccheckout
D         demo-project.xcodeproj/project.xcworkspace/xcuserdata
D         demo-project.xcodeproj/project.xcworkspace/xcuserdata/vincent.xcuserdatad
D         demo-project.xcodeproj/project.xcworkspace/xcuserdata/vincent.xcuserdatad/UserInterfaceState.xcuserstate
```

以及

``` sh
D         demo-project.xcodeproj/xcuserdata
D         demo-project.xcodeproj/xcuserdata/vincent.xcuserdatad
D         demo-project.xcodeproj/xcuserdata/vincent.xcuserdatad/xcschemes
D         demo-project.xcodeproj/xcuserdata/vincent.xcuserdatad/xcschemes/demo-project.xcscheme
D         demo-project.xcodeproj/xcuserdata/vincent.xcuserdatad/xcschemes/xcschememanagement.plist
```

在 commit 之前在來執行 `svn status` 來看結果如何：

``` sh
 M      .
A       demo-project
A       demo-project/AppDelegate.h
A       demo-project/AppDelegate.m
A       demo-project/Base.lproj
A       demo-project/Base.lproj/LaunchScreen.xib
A       demo-project/Base.lproj/Main.storyboard
A       demo-project/Images.xcassets
A       demo-project/Images.xcassets/AppIcon.appiconset
A       demo-project/Images.xcassets/AppIcon.appiconset/Contents.json
A       demo-project/Info.plist
A       demo-project/ViewController.h
A       demo-project/ViewController.m
A       demo-project/main.m
A       demo-project.xcodeproj
A       demo-project.xcodeproj/project.pbxproj
A       demo-projectTests
A       demo-projectTests/Info.plist
A       demo-projectTests/externals_uiTests.m
```

就可以發現需要被 ignored 的檔案都沒有在上面了，這時候也就可以開心而且放心地做 initial commit 了！ '3`)b

### 完成

在 commit 完之後，可以看 ignore file 是不是真的被忽略了，下 `svn status --no-ignore` 指令，加上 `--no-ignore` flag 就可以讓被忽略的檔案或目錄也可以被顯示出來。

```sh
I       demo-project.xcodeproj/project.xcworkspace
I       demo-project.xcodeproj/xcuserdata
```

這時候可以看到訊息最前面的字是 `I` ，就是後方列的目錄或檔案就是已經被忽略的檔案，這時候若是都符合自己的需求，就成功了！

## 心得

SVN 真的需要心力來交涉 ... :|

下一篇預計來寫 externals ~