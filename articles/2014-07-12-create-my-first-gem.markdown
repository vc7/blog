---
layout: post
title: "自己的 Gem 自己做"
date: 2014-07-12 18:52:10 +0800
comments: true
categories: ["Ruby", "gem", "basic"]
keywords: Ruby, basic, gem, write a gem
description: "日常中寫 Ruby 的人天天在用 gem ，但是總要知道 gem 怎麼寫吧。於是就找到一篇還不錯教怎麼簡單起步做 gem 的文章，做個筆記分享出來。"
references: [{title: "Specification Reference - RubyGems Guides", link: "http://guides.rubygems.org/specification-reference/"}, {title: "Creating Your First Gem - Sitepoint", link: "http://www.sitepoint.com/creating-your-first-gem/"}]
hero: "https://farm3.staticflickr.com/2914/14629940604_1b04a3128e_o.png"
---

日常中寫 Ruby 的人天天在用 gem ，但是總要知道 gem 怎麼寫吧。

雖然我是新手，但是還是好奇一下有沒有簡單教怎麼寫 gem 的文章，結果就發現了這篇： [Creating Your First Gem](http://www.sitepoint.com/creating-your-first-gem/) ，就來動手做做看了。

<!-- more -->

## 起步走

撰寫一個 gem 最基本分成以下幾個步驟

1. 建立基本的檔案架構
2. 建立 `gemspec` 撰寫規格
3. 加上你的程式碼
4. 產生 gem file
5. 安裝 gem 到電腦中
6. 加入一個 Ruby 專案中並使用

### 建立基本的檔案架構

首先開啟你的終端機，建立這個 gem 專屬的資料夾並建立檔案架構：

``` sh
$ mkdir my_gem
$ cd my_gem
$ mkdir lib
```

`my_gem` 則是這個 gem 的根目錄，而其中的 `lib` 則用來放你的 Ruby 檔案。

### 建立 `gemspec` 撰寫規格

接著建立一個簡單的 `gemspec` 來加入一些這個 gem 的規格。

在根目錄之中，建立一個 `my_gem.gemspec` ，打開之後加入以下程式碼：

``` ruby
Gem::Specification.new do |spec|
  spec.name = %q{my_gem}
  spec.author = "Vincent at Kumaya"
  spec.version = "0.0.0"
  spec.date = %q{2014-07-12}
  spec.summary = %q{my_gem is the best}
  spec.files = [
    "lib/my_gem.rb"
  ]
  spec.require_paths = ["lib"]
end
```

在這裡加上 gem 的名稱、作者、版本、日期、簡介及會用到的檔案以及使用這個 gem 時該引用檔案路徑。

**注意** 如果沒有加作者的話會有以下錯誤 build 不過去：

``` sh
WARNING:  See http://guides.rubygems.org/specification-reference/ for help
ERROR:  While executing gem ... (Gem::InvalidSpecificationException)
    authors may not be empty
``` 

詳細更多的參數可以參照 [官方文件](http://guides.rubygems.org/specification-reference/)

### 加上你的程式碼

根據上一小節的規格顯示，我們要把我們的 ruby 檔放在 lib 就可以讀到了。因此在 lib 資料夾中加入 `my_gem.rb` 這個檔案。

那這個簡易 `my_gem.rb` 這個檔案則長這樣

``` ruby
module MyGem
  class WhoIs
    def self.awesome?
      puts "MY FIRST GEM IS AWESOME!!"
    end
  end
end
```

### 產生 gem file

接著在 shell 中下指令根據 gemspec build 一個 gem 出來：

``` sh
$ gem build my_gem.gemspec
```

執行完之後則會跳出以下訊息：

```
Successfully built RubyGem
Name: my_gem
Version: 0.0.0
File: my_gem-0.0.0.gem
```

產出的 gem file 的名稱則是 Name 和版本的合併起來的字串。

### 安裝 gem 到電腦中

這時候還是待在 `my_gem` 的資料夾裡面，執行以下指令就可以安裝這個 gem ：

``` sh
$ gem install my_gem
```

接著會 prompt 出以下訊息，就代表安裝成功了：

``` sh
Successfully installed my_gem-0.0.0
Parsing documentation for my_gem-0.0.0
Installing ri documentation for my_gem-0.0.0
Done installing documentation for my_gem after 0 seconds
1 gem installed
```

#### 解除安裝

如果測試完要解除安裝，直接執行 `gem uninstall [gem_name]` 就可以了：

``` sh
$ gem uninstall my_gem
```

Prompt 這個訊息就代表成功解除安裝了

``` sh
Successfully uninstalled my_gem-0.0.0
```

### 加入一個 Ruby 專案中並使用

不過還不要急著解除安裝，先放到一個 Ruby 裡面測試看看是不是可以跑。

先出去這個資料夾，在其他地方建一個 Ruby file 來測這個 gem ，檔案就先叫 "my_first_gem.rb" 好了。

檔案內容如下：

``` ruby
require 'my_gem'
 
MyGem::WhoIs.awesome?
```

首先在第一行，引入剛剛安裝的 my_gem ，接著直接 call 我們寫好的 method 。

存檔之後在 shell 裡面執行這個檔案：

``` sh
$ ruby my_first_gem.rb
```

如果沒有任何意外，就會有以下訊息：

``` sh
MY FIRST GEM IS AWESOME!!
```

到這邊為止就完成一個簡單的 gem 了。

## 結論

透過這篇文章，可以做出一個簡單的 gem ，雖然很簡單，不過可以為更多的進階的功能打好基礎。雖然在外面有一些自動化的工具可以幫你產一個 gem ，但是了解 what is behind the code 還是是最重要的，若是不了解其中運作的原理對日後的開發會越來越不利。

當然，一般可以套用在 frameworks 的 gem 沒有這麼簡單，還是需要一些進一步的設定才能運作，日後玩玩之後再來寫篇筆記吧！

### 範例 Repo

我有把這篇文章的 code 在 Github 建一個 repo ，可以 clone 下來玩玩看 :D

**Repo:** [https://github.com/kumayast/my_gem](https://github.com/kumayast/my_gem)

*本篇文章撰寫時使用 Ruby `2.1.1p76` 以及 gem `2.2.2` on Mac OSX 10.10 beta 2 ，如果發現相關內容有更新，請在下方留言，謝謝。*
