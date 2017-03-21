---
layout: post
title: "如何在 Xcode 中設定 Library 的版本號"
date: 2015-01-16 22:51:45 +0800
comments: true
categories: ['iOS'] 
keywords: library, versioning, 版本, 函式庫, Current Project Version, Current Library Version, CURRENT_PROJECT_VERSION, DYLIB_CURRENT_VERSION
description: '在 Build Settings 項下，可以找到參數來設定專案以及 build 的版本號。'
references: []
hero: 'https://farm8.staticflickr.com/7578/16121892840_827273bcba_o.png'
---

在 library 的 `Project` > `Build Settings` 中搜尋 `CURRENT_PROJECT_VERSION` 或是 `Current Project Version` 就可以找到專案的版本號，在 Versioning 的 section 項下；相對的，在 Targets 中的一個選中一個 target ，也有一樣的參數可以設定。

<!-- more -->

<center><p><img src="https://farm8.staticflickr.com/7569/16308051182_07f9bd2c86_o.png" width="100%" alt="build-settings"><br><small>Project > Build Settings 的搜尋結果</small></p></center>

搜尋之後也會注意到有另外一個參數一起出現： `Current Library Version` ， 搜尋 `DYLIB_CURRENT_VERSION` 的話也會出現這項。

這個參數主要是來設定由這個 project 產出的 framework 的版本號是多少，在專案剛建立時，他的預設值就是 `$(CURRENT_PROJECT_VERSION)` ，也就是為什麼在搜尋的時候會一起出現。

# 數值

根據官方的敘述，數值設定一定要是 integer 或是 floating point number ；預設值則為 1 。

# 參考資料

- [Xcode Build Setting Reference - Apple Developer](https://developer.apple.com/library/ios/documentation/DeveloperTools/Reference/XcodeBuildSettingRef/1-Build_Setting_Reference/build_setting_ref.html#//apple_ref/doc/uid/TP40003931-CH3-SW63)