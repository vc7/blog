---
layout: post
title: "怎麼讓 Safari 開 UIWebView 裡的連結"
date: 2014-06-18 18:33:20 +0800
comments: true
categories: ["iOS", "UIWebView"] 
keywords: iOS, UIWebView, iOS Development
description: "用 UIWebViewDelegate 可以從 app 內的 UIWebView 直接打開 Safari"
references: [{title: "", link: ""}]
hero: "https://farm6.staticflickr.com/5507/14272650938_a8c76779a6.jpg"
---

最近接到一個需求：希望在點擊 app 中包的 web view 中的 link，直接在 Safari 中開啟。立馬翻了翻 Google ，是有辦法這樣做的。

<!-- more -->

這篇文章結束時也會附一個 demo 的 repository。

# UIWebViewDelegate

有寫 iOS 的人應該都會有直覺，如果想要做什麼事情，又很難做到的，一定會想到他所屬的 delegate。在這裡當然就是指 `UIWebViewDelegate` 了。

那什麼是 delegate 在這裡先不贅述，之後會再寫一篇文章說明；但是可以先想，delegate 簡單講，我常常說那就是一個 *跑腿的* ，他的實體則是 `@protocol`，協議，也可以想，這個協議可以指定他能幫你做什麼事：<del>買便當、買飲料之類的</del>。


## 開始！

在這裏，`UIWebViewDelegate` 則可以幫你把你點到的連結從 web view 中帶出來。今天要用到的 method 是這個：

[– webView:shouldStartLoadWithRequest:navigationType:](https://developer.apple.com/library/ios/documentation/uikit/reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webView:shouldStartLoadWithRequest:navigationType:)

這個 delegate method 在 web view 發生事件的時候會被呼叫到。他可以把自己、 `NSURLRequest` 和事件種類傳出來交給你。

### 先做一個 UIWebView 出來

第一步當然要有一個 `UIWebView`。在這裡先簡單化，在 `- viewDidLoad` 宣告，直接加進主要 controller 的 `view` 中：

```objc
UIWebView *webView = [[UIWebView alloc] initWithFrame:(CGRect){ 10, 30, 300, 300 }];
[webView loadRequest:[NSURLRequest requestWithURL:[NSURL URLWithString:@"http://apple.com"]]];

[self.view addSubview:webView];
```

這裏完成之後會長這樣， load 進來的頁面會根據 apple 官網內容而變動。

<center><img src="https://farm6.staticflickr.com/5523/14435029086_79289d869d.jpg"/></center>

### 加上 delegate 協議

在這邊，要把 view controller 和 web view 連結起來：就是透過在 view controller 加上 delegate 標籤，並將 view controller 指定為 web view 的 delegate。

#### 對 view controller 的 interface 加上標籤

在 .h 檔的檔頭或是 .m 的開頭都會看到有 `@interface` 標記的開頭一行。在 .h 的是 "Objective-C Class Declaration" ，類別的宣告，也就是會公開出去的介面；在 .m 裡面的通常則是 "Objective-C Class Extension"， 類別的擴充介面；還有另外一種是 Objective-C Category，先記這個也可以擴充 class 就好，只是用途不同。

在這裡，我的習慣是，如果加在 .h 的東西，就是要讓外部知道我有用這個 delegate ，如果不想讓別人知道，我就會加在 .m 中的 interface extension 。在這裡我先加在 .m 檔裡面就好了。

做法就是在 `@interface` 的最後面，用 `<[Delegate]>` 箭頭把 delegate 包起來就好：

```objc
@interface VCViewController () <UIWebViewDelegate>
```

如果想要在 .h 加，則是長這樣

```objc
@interface VCViewController : UIViewController <UIWebViewDelegate>
```

#### 讓 web view 成為 view controller 的跑腿

在原本宣告 `UIWebView` 的下一行加入指定 delegate：

```objc
webView.delegate = self;
```

所以上一段的宣告就會變成這樣：

```objc
UIWebView *webView = [[UIWebView alloc] initWithFrame:(CGRect){ 10, 30, 300, 300 }];
webView.delegate = self;
[webView loadRequest:[NSURLRequest requestWithURL:[NSURL URLWithString:@"http://apple.com"]]];

[self.view addSubview:webView];
```

到這邊為止，已經幫兩邊牽線完成了，接下來就可以開始接東西了。

### 實作 delegate method

這邊就要把一開始提到的 method 實作出來，在 `@implementation` ... `@end` 之間的最後加上這個 method ：

```objc
- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType
{
    return YES;
}
```

在這邊回傳的 `BOOL` 值則表示說，我要不要讓這個 URL request 開始載入，如果不想讓他載入，就 return `NO` 即可。

### 在 delegate method 中加入邏輯，推 Safari 出來

在 delegate method 中的邏輯分成兩個步驟：

1. 判斷事件種類
2. 是點擊連結的話，請 app 自己打開 request 裡的 URL


```
if ( navigationType == UIWebViewNavigationTypeLinkClicked ) {
	[[UIApplication sharedApplication] openURL:[request URL]];

	return NO;
}
```

#### 判斷事件種類

第一步，可以取得 `navigationType` ，由此可以判斷是不是 `UIWebViewNavigationTypeLinkClicked`

```
if ( navigationType == UIWebViewNavigationTypeLinkClicked ) {
}
```

*其他 `UIWebViewNavigationType` ： [[文件](https://developer.apple.com/library/ios/documentation/uikit/reference/UIWebView_Class/Reference/Reference.html#//apple_ref/doc/uid/TP40006950-CH3-SW22)]*

#### 請 app 打開 URL

```
[[UIApplication sharedApplication] openURL:[request URL]];
```

`[UIApplication sharedApplication]` 是取得 App 本身的 shared instance，利用 `openURL` 就可以打開 Safari ，前往從 delegate method 取得的 request 之中的 URL。

### 把 Delegate Method 的內容組裝起來

在 if 成立的時候，我則希望 web view 本身不要前往我點擊的 URL ，所以我回傳 `NO` 。其他則回傳 `YES` ，照原有的樣式執行。

```
- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType
{
    if ( navigationType == UIWebViewNavigationTypeLinkClicked ) {
        [[UIApplication sharedApplication] openURL:[request URL]];
        
        return NO;
    }
    
    return YES;
}
```

## 結論

這樣子就可以達成「在 web view 中點擊連結，推到 Safari 打開」這件事。以上程式碼寫作方式都是以最直接的方式表達要呈現的功能，比較沒有要為了要有 better pratices 就變得很複雜，實際專案上請視情況封裝。

關於以上的專案檔，我放在 Github 上，如果文字敘述不懂，可以直接執行專案來檢視成果： [[kumayast/VCWebViewPushDemo](https://github.com/kumayast/VCWebViewPushDemo)] 。

### Repository 資訊

做這份範例專案時的執行環境

- Mac OSX 10.10 beta 1
- Xcode 5.1.1
- iOS SDK 7
- Empty Application 


