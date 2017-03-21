---
layout: post
title: "在 iOS 中寫 protocol 的時候要加上 NSObject protocol"
date: 2014-07-02 23:58:20 +0800
comments: true
categories: ["iOS", "protocol", "delegate"]
keywords: iOS, delegate, protocol
description: "今天在檢查 delegate 的 respondsToSelector 怎麼不能用，原來是要讓 protocols 遵循 NSObject protocol。"
references: [{title: "Make the protocol conform to NSObject - stackoverflow", link: "http://stackoverflow.com/a/7941080"}]
hero: "https://farm4.staticflickr.com/3855/14372603637_af9e6e7a42_o.png"
---

{% img https://farm4.staticflickr.com/3859/14558035262_9e0f10ab1e_o.png %}

今天在寫 `delegate` (`@protocol`) 要檢查 delegate method 有沒有被實作，結果 Xcode 抱怨：

``` text
No know instance method for selector 'respondsToSelector:'
```

`respondsToSelector:` 這個 method 是未知的。

<!-- more -->

## 讓自訂 Protocol 遵循 NSObject Protocol

因為之前在寫 protocol 的時候都太順了。後來發現是在 @protocol 宣告的地方：

``` objc
@protocol VCTabBarDelegate

	// some methods 

@end
```

沒有讓 protocol conform to `NSObject` protocol 。因此在宣告的時候加上 `<NSObject>` 就可以了：

``` objc
@protocol VCTabBarDelegate <NSObject>

	// some methods 

@end
```

而 `respondsToSelector:` 是屬於 `NSObject` protocol ，所以要是要讓自己的 protocol 能夠使用這些檢查用的 methods ，就要加上去。

