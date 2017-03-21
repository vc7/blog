---
layout: post
title: "Swift - 不用 Storyboard 來啟動 App"
date: 2015-04-14 00:02:14 +0800
comments: true
categories: ["iOS", "UI", "Programmatically", "Swift"] 
keywords: Swift, UI, Programmatically
description: "開始做自己的小專案，試著用純 Swift 來寫，第一步就是想要把跟不太喜歡又有點吃資源的 Storyboard 移掉 XD"
hero: "https://farm9.staticflickr.com/8809/16524341694_846009150a_o.png"
---

最近開始做自己的小專案，試著用純 Swift 來寫，第一步就是想要把跟不太喜歡在開發時又有點吃資源的 Storyboard 移掉 XD

<!-- more -->

我其實是 UI 手刻控、也都不用 Interface Builder。最近也認真的開始使用 xib 搭配 autolayout 使用（其實有可能是數學算太久了有點累 XD），配置的方式也比較物件導向，因此我也優先選擇這個方式做；這樣的話，也不會用到 Xcode 預設配置的 Main Storyboard。

# 初步清場

首先，在專案設定頁中，選中一個 Target ，在 **General** 頁籤下 **Deployment Info** 區塊中的 **Main Interface** 中的文字清除，就可以避免專案開啟時，去讀取 Main Storyboard。

<center><p><img src="https://farm9.staticflickr.com/8702/17136268635_9c1981c5b0_o.png"><br><small>移除 Main Interface<br>...</small></p></center>

來到 AppDelefate.swift 中，去除一些其他不需要的 application delegate methods ，只留下開啟時會呼叫的 `application:didFinishLaunchingWithOptions:`，剩下這樣：

``` swift AppDelefate.swift
import UIKit

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?

    func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {

        return true
    }
}
```

# 顯示畫面

## UIWindow

預設就有配置一個 optional variable `window` ，他就是用來顯示 app 所有畫面的視窗。這個類別類似於桌面程式中「視窗」，只不過差別在 Mac app 中的 `NSWindow` 可以有不只一個 instances 在切換；在 iOS 中沒有切換視窗的概念，則只需要一個 instance 作為根視窗。

## 初始化

首先，這個 window 並還沒有被初始化，我們將它初始化，並將 frame 設定為主要螢幕的大小

``` swift
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
    
    // 初始化 self.window
    self.window = UIWindow(frame: UIScreen.mainScreen().bounds)
    
    return true
}
```

## 顯示出來

這個 window 不會憑空就顯示在畫面上，我們則需要透過 `makeKeyAndVisible` 這個 method 讓它顯示出來。為了可以清楚知道真的有顯示，在這邊先把他的背景色改成白色。

``` swift
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
    
    self.window = UIWindow(frame: UIScreen.mainScreen().bounds)
    // 把 window 的背景改成白色
    self.window!.backgroundColor = UIColor.whiteColor()

    // 把 window 顯示出來
    self.window!.makeKeyAndVisible()
    
    return true
}
```

由於 window property 是一個 optional variable ，使用它的時候則需要幫他解開束縛、在後面加上 `!` 來 unwrapping 這個值

## 加入 Root View Controller

`UIWindow` 有一個屬性是用來顯示根 view controller - `rootViewController` ，只要將需要第一個顯示的 view controller 丟給她，在 app 開啟時，就會幫我們在顯示 launch image 之後第一個顯示出來。

為了清楚知道有顯示 view controller 出來，在這裡先指定背景為灰色來清楚識別。

``` swift
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
    
    self.window = UIWindow(frame: UIScreen.mainScreen().bounds)
    self.window!.backgroundColor = UIColor.whiteColor()
    
    // 宣告一個 view controller 並指定背景為灰色
    var viewController = UIViewController()
    viewController.view.backgroundColor = UIColor.grayColor()
    
    // 指定 root view controller
    self.window!.rootViewController = viewController
    
    self.window!.makeKeyAndVisible()
    
    return true
}
```

# 完成

到這裡即告一個段落，接著只要根據需求，根據這個方式，把主要的 view controller (如 tab bar controller) 替換上去即可。

## Qiita 同步發佈

- [Swift - 不用 Storyboard 來啟動 App](http://qiita.com/vc7/items/3f2cf7480717d7ca00b9)

<hr>
<small><span>
本文範例以 Xcode Version 6.3 (6D570), iOS SDK 8.3 建置。如果發現相關內容有更新，麻煩請在下方留言告知，謝謝。<br>
<span></small>