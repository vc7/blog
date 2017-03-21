---
layout: post
title: "Octopress - 在 RSS Feed 中顯示所有文章"
date: 2014-07-22 23:32:11 +0800
comments: true
categories: ["Site", "Octopress"]
keywords: Octopress, feed, rss
description: "在設定 Google Webmaster 的時候，我有把部落格內主要分類的 RSS 加到 Sitemap 中。但是有一天發現新增文章到一個數量之後， Webmaster 工具在這些 category feed 找到的連結數量就停在 6 這個數字。"
references: [{title: "For loops - Shopify/liquid", link: "https://github.com/Shopify/liquid/wiki/Liquid-for-Designers#for-loops"}]
hero: "https://farm4.staticflickr.com/3885/14531998927_980d490fb1_o.png"
---

在設定 Google Webmaster 的時候，我有把部落格內主要分類的 RSS 加到 Sitemap 中。但是有一天發現新增文章到一個數量之後， Webmaster 工具在這些 category feed 找到的連結數量就停在 6 這個數字。

<!-- more -->

## 產生 RSS 的 Templates

後來找一找就發現，在產 RSS 的地方有限制要顯示多少數量。在 Octopress 中負責產生 RSS 的 Template 的檔案則有兩個：

- source/atom.xml
- source/_includes/custom/category_feed.xml

第一個檔案是產生全站的 RSS feed ；第二個檔案則是由 `plugins/category_generator.rb` 這個檔案呼叫，建立各個 category 的 feed 。

## 移除限制

限制的設定是由 liquid 語法中的 [limit](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers#for-loops) 所設定。如果不想要限制就可以直接拿掉，如果想要顯示文章多一點，則可以把 limit 後的數字調高一點。

### atom.xml

{% raw  %}
``` diff
-  {% for post in site.posts limit: 20 %}
+  {% for post in site.posts %}
```
{% endraw  %}

### category_feed.xml

{% raw  %}
``` diff
-  {% for post in site.categories[page.category] limit: 5 %}
+  {% for post in site.categories[page.category]%}
```
{% endraw  %}

## 收工

這樣子重新 generate 網站就可以更新所有的 RSS 檔案成全部顯示了。

如果這些 templates 是放在 .theme 的自訂主題裡，更新完記得要執行

``` sh
$ rake install[theme_name]
```

才會使用修改過的 RSS templates
