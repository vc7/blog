---
layout: post
title: "架構好 Express Modules"
date: 2014-06-30 17:37:20 +0800
comments: true
categories: ["node.js", "express"]
keywords: node.js, express, module, architecture
description: "架構好 express module 可以讓日後更好維護，清楚切割 exports 出去的物件與實作。"
references: [{title: "", link: ""}]
hero: "https://farm3.staticflickr.com/2921/14357134817_d9d887d918_o.png"
---

在前一篇文章 [Express Route Seperation]({{ root_url }}/2014/06/17/express-route-seperation) 中有提到怎麼寫 express module，後來有發現比較好的寫法：把需要 export 出去的變數以及 function objects, 全部提到檔案一開始的地方。這樣子在開發的時候比較能夠一覽無遺這個 module 的組成，也比較好維護。

<!-- more -->

## 架構

因此，歸納之後，檔案架構上是如此：

```
[     Require     ]
[     Headers     ]
[       Body      ]
```

首先，一開始的 `Require` 區塊引入相關的其他 module 之後，就可以再 `Header` 指定這個 module 需要 export 的東西有哪些，所以算是檔頭的宣告區。

## 成果

所以假如原本的檔案是這個樣子，如果 export 的 function 一多的話，會很難維護：

```js
var AppService = require('/services/appService');

exports.index = function(req, res){
  res.render('index', { title: 'Route Separation Example - Index' });
};

exports.show = function(req, res){
  var postId = req.params.id;
  res.render('show', { title: 'Route Separation Example - Show' });
};
```

結構化過後，顯然比較好閱讀和維護：

```js
var AppService = require('/services/appService');

exports.index = index;
exports.show = show;

function index(req, res) {
  res.render('index', { title: 'Route Separation Example - Index' });
};

function show(req, res) {
  var postId = req.params.id;
  res.render('show', { title: 'Route Separation Example - Show' });
};
```

