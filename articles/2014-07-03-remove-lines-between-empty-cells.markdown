---
layout: post
title: "不顯示 UITableView 中空 Cells 分隔線"
date: 2014-07-03 23:29:20 +0800
comments: true
categories: ["iOS", "UITableView"]
keywords: iOS, UITableView
description: "在建立 UITableView 的時候，在沒有 cell 的地方還是會有等距的分隔線跑出來，這篇文章將說怎麼移除這些分隔線。"
references: [{title: "Make the protocol conform to NSObject - stackoverflow", link: "http://stackoverflow.com/a/5377569"}]
hero: "https://farm6.staticflickr.com/5520/14379199089_58d56e9124_o.png"
---

在建立 `UITableView` 的時候，在沒有 cell 的地方還是會有等距的分隔線跑出來，這篇文章將說怎麼移除這些分隔線。

<!-- more -->

## iOS 6.1 及 iOS 7.*

最簡單的方式就是設定 tableView 的 `tableFooterView` property 的矩形大小設定為 0 ：

``` objc
- (void)viewDidLoad 
{
	[super viewDidLoad];

	// 這一行就會讓分隔線消失
	self.tableView.tableFooterView = [[UIView alloc] initWithFrame:CGRectZero];
}
```

## 比較舊的版本

用 `UITableViewDelegate` 的 method 直接指定一個很小的高度給 footer view 就可以了：

``` objc
- (CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section 
{
	return 0.01f;
}

```

如果覺得這個方法還不夠，也可以直接再加上一個 delegate method ，回傳一個新的 `UIView` ：

``` objc
- (UIView *)tableView:(UITableView *)tableView viewForFooterInSection:(NSInteger)section
{        
	return [UIView new];
}
```

如果是 non-ARC ，請加上 `autorelease` ：

``` objc
- (UIView *)tableView:(UITableView *)tableView viewForFooterInSection:(NSInteger)section
{        
	return [[UIView new] autorelease];
}
```