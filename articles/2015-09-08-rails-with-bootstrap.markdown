---
layout: post
title: "在 Rails 專案加入 Bootstrap 套件"
date: 2015-09-08 09:44:14 +0800
comments: true
categories: ["Rails"] 
keywords: Rails, Bootstrap
description: "這篇是以 4.2.2 產生出來的 rails 專案為例，使用 gem 安裝 bootstrap 3 的主題。"
hero: "https://farm6.staticflickr.com/5642/21205765206_ef484e9f41_o.png"
---

<img src="https://farm6.staticflickr.com/5642/21205765206_ef484e9f41_o.png">

<!-- more -->

[Bootstrap](http://getbootstrap.com/) 是網站前端非常有名的框架之一，原始專案是由 twitter 內部的工程師， [Mark Otto](https://twitter.com/mdo) and [Jacob Thornton](https://twitter.com/fat)，所發起。可以讓工程師在短時間內，幫網站的套上漂漂亮亮的外觀。

## 使用工具

- rails 4.2.2
- [bootstrap-sass](https://github.com/twbs/bootstrap-sass) 3.3.5

這篇是以 4.2.2 產生出來的 rails 專案為例，使用 gem 安裝 bootstrap 3 的主題。

bootstrap-sass 的 [官方 repo](https://github.com/twbs/bootstrap-sass) 有各種不同使用方式的教學，這篇是就只清楚的寫出 rails 專案該怎麼使用，比較方便參考。

## 加入 Gem

第一步當然是要加入 bootstrap 的 gem 囉

``` ruby Gemfile
(前略)
...
gem 'bootstrap-sass', '~> 3.3.5'
gem 'sass-rails', '>= 3.2'
...
(後略)
```

打開 Gemfile, 加的地方要確保這個 gem 在 development, production 的環境內都可以取用到。

sass-rails 這個 gem 在 `rails new` 產生的全新專案就會預先載入了，通常不需要再加一次，就已經在 Gemfile 的某處了。

``` bash
bundle install
```

執行 install, 安裝這一段所加上的 gems 。

安裝新的 gem 之後，重啟 `rails s` 。

## 加入 Stylesheet 相關資源

為了要在專案裡全域都可以使用，接下來這段必須加在 `app/assets/stylesheets/application.scss` 中。

### 修改副檔名

因為 rails 生出來的副檔名是 `application.css` ，必須要改成 `.scss` 這個副檔名才可以使用 sass 的檔案內容。

這時候在專案的根目錄執行以下指令，更換副檔名

``` bash
mv app/assets/stylesheets/application.css app/assets/stylesheets/application.scss
```

### 引入 Bootstrap

打開這個檔案之後，在所有註解的最末端空白的地方，加上以下的程式碼


``` scss app/assets/stylesheets/application.scss
@import "bootstrap-sprockets"; // 這行一定要在 "bootstrap 前面
@import "bootstrap";
```

這裡的行末要記得 <strong style="color:red">加上分號</strong>

## 加入 JavaScript 相關資源

最後一步就是加入 JavaScript 的檔案了，因為這是網站的全域要使用的，就要加在 `app/assets/javascripts/application.js` 裡面

``` js app/assets/javascripts/application.js
//= require jquery
//= require bootstrap-sprockets
```

第一行 jquery 在新產生的 rails 專案就已經存在了，所以通常是加第二行的 bootstrap-sprockets 即可

## 其他資源

- [Bootstrap CSS (官方資源)](http://getbootstrap.com/css/)

## 同步發佈

- [Qiita](http://qiita.com/vc7/items/3250415f5ec1aa332a0a)