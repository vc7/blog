---
layout: post
title: "Enum 搭配 Switch 使用的心得 - 囧"
date: 2015-04-15 00:02:14 +0800
comments: true
categories: ["iOS", "Swift"] 
keywords: Swift, enumerations, enum, NS_ENUM
description: "在 Objective-C 裡面我會定義一組 NS_ENUM 用來把 table view 中的 section index 標示用途，換到 Swift 之後也想要用這個方式做，但是嘗試之後發現結果好噁心 ..."
---

在 Objective-C 裡面我會定義一組 NS_ENUM 用來把 table view 中的 section index 標示用途，換到 Swift 之後也想要用這個方式做，但是嘗試之後發現結果好噁心 ...

<!-- more -->

# In Objective-C

在 Objective-C 之中，覺得渾然天成

``` objc 定義NS_ENUM
typedef NS_ENUM(NSInteger, DemoEnum) {
    DemoEnumZero = 0,
    DemoEnumOne = 1,
    DemoEnumTwo = 2
};
```

情境設定在 table view 相關的 data source 或 delegate methods 使用，會接到一個  section index ：


``` objc 搭配switch使用
NSInteger section = indexPath.section

switch (0) {
    case DemoEnumZero:
        NSLog(@"0");
        break;
    case DemoEnumOne:
        NSLog(@"1");
        break;
    case DemoEnumTwo:
        NSLog(@"2");
        break;
    default:
        break;
}
```

看起來還滿 OK 的，

但是到 Swift 之後世界就變了 ´･ω･`

# In Swift

來到 Swift ，先定義 raw value 型態為 Int 的 enum 

``` swift
enum DemoEnum: Int {
    case Zero = 0
    case One = 1 
    case Two = 2
}
```

當 enum 型態為 Int 時，指定第一個值之後，後面的值會自己遞增上去，因此這裡也是可以不用寫後面兩個 cases 的 raw values

這時候如果用一樣的寫法：

```
let section:Int = indexPath.section
switch section {
case DemoEnum.Zero:
    println("0")
default
}
```

馬上就會看到 Xcode 在抱怨：

![](https://farm9.staticflickr.com/8801/16972503689_995b7dff07_o.png)

> Enum case pattern cannot match values of the non-enum type 'Int'

說我 case 的型態和要比較的型態不一樣，而無法讓我編譯

到這邊為止，如果有看過[文件](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Enumerations.html)，就會知道問題出在哪邊， **是要拿 enum 的 raw value** 來比較，也就是應該要寫成這樣

``` swift
case DemoEnum.Zero.rawValue:
```

這時候眉頭皺了一下，心裡想著這是什麼鬼東東，如果要用的話不是就變成 ...

``` swift
switch section {
case DemoEnum.Zero.rawValue:
    println("0")
case DemoEnum.One.rawValue:
    println("1")
case DemoEnum.Two.rawValue:
    println("2")
default
}
```

寫完之後，手停下來盯著每個 case 後面都要掛的 .rawValue 看，覺得非常噁心 

看來是不能這樣寫，寫完都覺得會討厭自己

後來就是著把取得 raw value 這件事情分離出去一個 method

```
func fetchRaw(theEnum: DemoEnum) -> Int {
    return theEnum.rawValue
}
```

原本的 switch-case 就可以變成這樣，也至少比較易讀了

``` swift
switch section {
case fetchRaw(.Zero):
    println("0")
case fetchRaw(.One):
    println("1")
case fetchRaw(.Two):
    println("2")
default
}
```

如果有什麼建議或疑問，歡迎在下方留言討論 '3`y

## Qiita 同步發佈

- [Enum 搭配 Switch 使用的心得 - 囧](http://qiita.com/vc7/items/7e5618fe8a62027d8ae1)

<hr>
<small><span>
本文範例以 Xcode Version 6.3 (6D570), iOS SDK 8.3 建置。如果發現相關內容有更新，麻煩請在下方留言告知，謝謝。<br>
<span></small>