---
layout: post
title: "NSDate 和測試的那些小事"
date: 2015-01-22 23:49:45 +0800
comments: true
categories: ['iOS', 'unit test', 'xctest'] 
keywords: unit test, 單元測試, XCTest, TDD, NSDate, NSDateFormatter
description: '純粹筆記一下遇到的問題～'
references: []
hero: 'https://farm9.staticflickr.com/8581/16339766931_8bb091b4ce_o.png'
---

純粹筆記一下遇到的問題～

<!-- more -->

最近在處理日期資料，寫了測試要驗證日期是否正確，於是就碰到兩的問題：

- 要怎麼驗證 `NSDate` object?
- 知道怎麼驗證了，怎麼時間就差這麼一些，測試沒過？

# 怎麼驗證 `NSDate`

之前有寫過一篇有關於 XCTAssert 的文章 - [XCTest Assertions 及其種類]({{site.url}}/2014/07/10/xctest-assertions/) 有列出有哪些 XCTAssert 可以用，於是就要從這邊挑一個出來用。

上網找了找資料，其實只要取得 NSDate 物件的 timestamp 即可，再去對照處理的日期的 timestamp 是否符合預期即可。

timestamp 的型態是 doulble ，因此要比較他的精準度，就要使用 `XCTAssertEqualWithAccuracy` 了

## XCTAssertEqualWithAccuracy

先複習一下，

``` objective-c XCTAssertEqualWithAccuracy
XCTAssertEqualWithAccuracy(expression1, expression2, accuracy, format...)
```

- `expression1` 和 `expression2` 是兩個需要比較的對象
- `accuracy` 則是放比較的精準度
- `format...` 則是可以放測試沒過需要在 console 印出的文字

## 那就來比吧

``` objective-c XCTAssertEqualWithAccuracy Usage
XCTAssertEqualWithAccuracy(targetTimestamp, [checkedDate timeIntervalSince1970], 0.01);
```

- `targetTimestamp` 是個 double 變數，存放預期值
- `checkedDate` 則是需要被檢查的目標 NSDate 物件
- `0.01` 則是容許的誤差值

當時這樣跑下去，綠燈還亮不了 ... 繼續看下去 ...

# 時區問題

在寫 date formatter 的時候，我的需求是要由字串轉成 NSDate object ，

於是很爽快地就做完了，

測下去發現產出的 timestamp 轉換之後硬生生的多了八個小時。

後來又翻了翻文件，發現 `NSDateFormatter` 會自動幫你轉成手機的當地時間，我所在的時區是台北， GMT 的時間 +8 ，因此在這時候就需要把 date formatter 的時區設定好，或是在測試的時候需要加上時區的調整。

## NSDateFormatter - Time Zone

如果時間要以 GMT 為準，就可以設定一下 date formatter 的時區，

``` objective-c NSDateFormatter - set time zone
dateFormatter.timeZone = [NSTimeZone timeZoneWithAbbreviation:@"GMT"];
```

- `dateFormatter` 是 NSDateFormatter 的 instance

這時候，
由這個物件產出來的日期，就和 GMT 一致了

# 參考資料
- [XCTest Assertions 及其種類 - 熊屋 | 技術小記]({{site.url}}/2014/07/10/xctest-assertions/)
- [ios - XCTAssertEqual: How to compare NSDates? - Stack Overflow](http://stackoverflow.com/a/19777152)
- [iphone - NSDateFormatter with default time zone gives different date than NSDate description method - Stack Overflow](http://stackoverflow.com/a/16719818)