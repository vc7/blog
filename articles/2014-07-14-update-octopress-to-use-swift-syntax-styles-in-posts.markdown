---
layout: post
title: "更新 Octopress 來使用 Swift 的 Syntax Style"
date: 2014-07-14 22:13:15 +0800
comments: true
categories: ["Octopress", "Site", "Upgrade"]
keywords: Octopress, upgrade, swift, jekyll, pygment
description: "在 WWDC 2014 發佈 Swift 之後，軟體開發又多了一個語言。當然在 Octopress 裡面掛的 gem 也要更新，因為有需求，今天就來一併做一做。"
references: []
hero: "https://farm6.staticflickr.com/5545/14650269901_e6393d906b_o.png"
---

在 WWDC 2014 發佈 Swift 之後，軟體開發又多了一個語言。當然在 Octopress 裡面掛的 gem 也要更新，因為有需求，今天就來一併做一做。

<!-- more -->

## 要做的事情

- 更新 Octopress 到最新的，這篇文章抓這個 commit 時的 master： [4fdae](https://github.com/imathis/octopress/tree/4fdae37e4294618084f652c99c0c06ba7663ac07)
- 更新 Jekyll 到 `~> 2.1.1`
- 更新 Pygment 到 `~> 0.6.0`

### 今天相關的東西

- [Octopress](http://octopress.org/)
- gem: [Jekyll](https://rubygems.org/gems/jekyll)
- gem: [Pygment](https://rubygems.org/gems/pygments.rb)

## 更新 Octopress

首先就要來更新一下 Octopress ，因為我用的版本有點舊，前幾天手動去更新 gem 的時候發現 Octopress 所用的 Jekyll 更新之後有些指令 deprecated 了，因此決定直接更新 Octopress 本身比較快，直接可以把其他所有舊的 gem 一併 upgrade 。

### 加入 remote

首先，如果還沒有把 Octopress 加入 remote 的話，可以和平常在 fork 時加上 upstream 一樣，把 Octopress 的 repo 加入目前的 git repo 中。

開啟 terminal 並進入要更新的資料夾中，執行以下指令：

``` sh
$ git remote add octopress git://github.com/imathis/octopress.git
```

### pull 

``` sh
$ git pull octopress master
```

接著把官方 repo 的東西拉下來，更新原有專案。這時候有可能會發生 conflicts ，請根據自己原有的修改解決 coflicts。

### Update Templates (Optional)

如果想要更新官方的主題，請執行以下的指令

``` sh
$ rake update_source
$ rake update_style
```

我有自己做的主題，所以這邊沒有做，因此看個人需求。

### 更新完了

到這裡雖然更新完了，在編譯文章的時候會跳出來這個錯誤：

``` sh
jekyll 2.0.3 | Error:  Pygments can't parse unknown language: swift.
```

這時候就可以開始解決 Swift 的語言編譯的問題了。

## 更新 Jekyll 和 Pygment

### Pygment

Pygment 原有是一個 Python 寫成的 syntax highlighter ，在 WWDC 過後，馬上有人發 issue 要做 Swift support 的更新： [birkenfeld/pygments-main issue #1000](https://bitbucket.org/birkenfeld/pygments-main/issue/1000/swift-programming-language-support) 。 

於是 Ruby 端的 Pygment wrapper 也有做對應的更新： [tmm1/pygments.rb issue 126](https://github.com/tmm1/pygments.rb/issues/126) ，更新到 0.6.0 版本之後就可以認得 Swift 了，不過目前仍然沒有 update log 可以參照。

首先，先在 Gemfile 裡面的 `:development` 區塊加入：

``` rb
gem 'pygments.rb', '~> 0.6.0'
```

接著跑 bundle install 後試跑 rake preview 就會發現， Jekyll 至少要到 2.1.1 才行，而這個版本的 Octopress 只有用到 2.0 以上。

``` sh
You have requested:
  jekyll ~> 2.1.1
```

### Jekyll 

這個版本的 pygment 依賴於 2.1.1 ，於是進到 Gemfile 把 jekyll 的版本提升到 2.1.1 ，接著跑以下指令，更新 Jekyll ：

``` sh
$ bundle update jekyll
```

## 收工

在跑所有 gem 的改變之後，別忘記要做 `bundle install` 重新配置使用的 gem 。

這時候就可以下 `rake preview` 指令來試試看 Swift 的 syntax 可不可以被顯示出來了：

``` swift
let year: Int = 2014
var earning: Double?
var list: [Int] = [1,2,3]

func lessThanTen(number:Int) -> Bool {
    return number < 10
}
```

成功了！

這樣就可以安心在 Octopress 裡面寫 Swift code 了 :DDD

## 心得

要注意的事情是，由於 Swift 的語法在 beta 之間有許多變動，所以日後正式版本出來之後，也有可能需要再次更新 Pygment 。

再來就是，在更新之前還滿緊張的，很怕一更新 Octopress 就壞掉了，開始更新之前還反覆做了 3 次更新的練習才開始更新自己的部落格，不過實際更新時幸好沒有問題發生。

Have fun with Swift!
