---
layout: post
title: "Express Route Seperation"
date: 2014-06-17 23:17:20 +0800
comments: true
categories: ["node.js", "express", "route"] 
keywords: node.js, express, routing, route, code, javascript, architecture
description: "因為專案的需求，所以需要接觸到 express 。這裏大約紀錄一下實作的時候的用法，把 index.js 留得比較乾淨。"
references: [{title: "", link: "https://github.com/visionmedia/express/tree/master/examples/route-separation"}]
hero: "https://farm3.staticflickr.com/2934/14465233313_be6024144b_o.png"
---

因為專案的需求，所以需要接觸到 express 。這裏大約紀錄一下實作的時候的用法，把 index.js 留得乾淨一點，把不需要的暴露出來的細節，架構化地藏起來，這篇以 route 為主。因為第一次寫 node.js 的 app, 如果有錯誤麻煩指證，謝謝 :)

<!-- more -->

## 架構

程式碼主要分成三個部分：

- 宣告 routing 用的主要物件
- 設定 app 的 routing
- 建立 routing 對應到的 functions

### 宣告 routing 用的主要物件

這個地方在 index.js 裡面進行，在檔頭環境設定完之後的地方：

```js
var site = require('routes/site');
var post = require('routes/post');
var user = require('routes/user');
```

這裏引入各自 routing 設定檔（第三個部分）的位置，在這邊會自動加後綴 `.js` 檔案。所以這邊會抓到 `site.js`, `post.js`, `uer.js` 等三個檔案。而這三個檔案會各自成一個主物件，因此引入後直接 assign 給對應的變數給後來的步驟使用。

我的感覺這裡是有點像 MVC 中的 Controller 的感覺。

### 設定 app 的 routing

接著前面宣告的 express object，`app`，可使用 html 的動詞來指定 routing：

```
app.get('/posts', post.index);
app.get('/post/:id', post.show);
app.get('/post/:id/edit', post.edit);
```

對應到的 function 我則是用 [Rails 的 routing 命名方式](http://guides.rubyonrails.org/routing.html#resource-routing-the-rails-default)（`index`, `show`, `new`, `edit`, `create`, `update` 和 `destroy`）為主。


### 建立 routing 對應到的 functions

因此在對應的檔案，要有對應的 function 來反應 routing 的動作。

以其中一個 `post.js` 來說好了，剛剛在上面有用到 `index`, `show`, `edit` 三個 functions，所以在這邊要 `exports` 出去這三個 function objects：

```
exports.index = function(req, res){
  res.render('index', { title: 'Route Separation Example - Index' });
};

exports.show = function(req, res){
  res.render('show', { title: 'Route Separation Example - Show' });
};

exports.edit = function(req, res){
  var postId = req.params.id;
  res.render('edit', { title: 'Route Separation Example - Edit' });
};
```

exports 則是 node.js 內建的 object, 有興趣可以去看看 [文件](http://nodejs.org/api/modules.html#modules_module_exports)。他會包裝一個 .js 檔案 exports 所屬的物件成一個主要的物件來使用，所以還滿方便的。目前我用 exports 來包裝一些常用的 helpers 和第三方服務承各自主要的 services。可以減少程式碼的重複撰寫，在修改上也不會那麼繁雜。

## 心得

很開心因為有需要而碰到 node.js，因為之前一直不想碰 XD。之前有上過 [保哥](http://blog.miniasp.com/) 的 js 課程，上完之後因為沒有需要使用 javascript ，課後複習完就先放著，有些可能就就忘掉了。藉由這次的機會，可以撿回來，因為對語言特性比以前還要熟悉，就嘗試一些比較進階的寫法。

目前的專案不會太困難，但是可以自己一開始從其他程式語言取經（主要是工作上的 Objective-C 專案和 [最近開始學](http://learn-rails.today/workshops/intermediate) 的 Rails 架構），再用 javascript 的技巧和一些樣板實作包裝成一個架構滿乾淨的小專案，也算是學以致用，不浪費了！
