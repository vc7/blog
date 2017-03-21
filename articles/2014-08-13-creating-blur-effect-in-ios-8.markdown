---
layout: post
title: "在 iOS 8 中使用模糊效果"
date: 2014-08-13 10:59:12 +0800
comments: true
categories: ["iOS", "iOS 8", "Blur", "UI"]
keywords: iOS, iOS 8, Blur, UI, User Experience, 毛玻璃效果
description: "在 iOS 8 出來之後，則提供了 `UIVisualEffectView` 可以疊加在繼承 `UIView` 的 class 的 objects ，除了 `UIView` 之外就還有 `UIImageView` 等比較常用會用來加上模糊效果，因此可以更加容易達到這個效果。"
references: [{title: "Box Blue", link: "http://en.wikipedia.org/wiki/Box_blur"}, {title: "UIVisualEffect Class Reference - Apple Documentation", link: "https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIVisualEffect_class/index.html"}, {title: "UIBlurEffect Class Reference - Apple Documentation", link: "https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIBlurEffect_Ref/index.html#//apple_ref/c/tdef/UIBlurEffectStyle"}, {title: "How to use UIVisualEffectView? - Stack Overflow", link: "http://stackoverflow.com/a/24083728"}]
hero: "https://farm4.staticflickr.com/3896/14903633405_bc9dda5018_o.png"
---

在 iOS 7 出來一個背景模糊的效果， Apple 官方的 sample code 則有提供怎麼使用 vImage, Quartz 來實作這個效果。接著在 iOS 8 出來之後，則提供了 `UIVisualEffectView` 可以疊加在繼承 `UIView` 的 class 的 objects ，除了 `UIView` 之外就還有 `UIImageView` 等比較常用會用來加上模糊效果，因此可以更加容易達到這個效果。

<!-- more -->

## Bluring Images on iOS 7

Apple 提供了一個 sample code 可以拿來用：[Blurring and Tinting an Image](https://developer.apple.com/library/prerelease/iOS/samplecode/UIImageEffects/Introduction/Intro.html)。 Implementation 在 `UIImageEffects/UIImageEffects.m` 這個檔案裡，從他的 header file 則可以看出可以怎麼使用這些效果。

### 實作細節

從 sample code 中可以看出來， 實作上利用三次 box blur 來接近高斯模糊的效果，速度上也比較快。相關的原理可以看這個連結：[Filter primitive ‘feGaussianBlur’](http://www.w3.org/TR/SVG11/filters.html#feGaussianBlurElement)

### 怎麼用

我有把這兩個實作主要的檔案抽出來成為一個 pod ，可以在 Podfile 加入下方程式碼後執行 `pod install` ：

``` ruby
pod 'UIImageBlurEffectCategory', :git => 'https://github.com/vc7/UIImageBlurEffectCategory.git'
```

接著引入 header file ：

``` objc
#import <UIImageBlurEffectCategory/UIImageBlurEffectCategory.h>
```

我把 code 改成使用 `UIImage` category ，以提供擴充 class methods 的方式來使用。舉例來說，如果要產生一個簡單模糊過的圖像，就用以下的方式即可：

``` objc
UIImage *bluredImage = [UIImage imageByApplyingLightEffectToImage:[UIImage imageNamed:@"myImage.jpg"]];
```

原先提供的 sample code 提供了比較大的變化空間，有比較多的參數（ blur radius, tint color, saturation delta factor 以及 mask image ）可以調整模糊的效果，到 iOS 8 還是可以使用。

## iOS 8, UIVisualEffectView

在過去，若是要對一個 view 模糊，需要繪製出那個 view 的快照，接著對他 apply 模糊，再加到一個 UIImageView 中顯示出來。

在 iOS 8 之後， UIKit 則提供了簡單的方式來達成模糊的效果，簡單幾個步驟就可以直接模糊 UIView instances 。

### 簡易流程

{% img center https://farm6.staticflickr.com/5590/14880257436_929eaea88d_o.png %}

#### Objective-C

``` objc
UIVisualEffect *blurEffect = [UIBlurEffect effectWithStyle:UIBlurEffectStyleLight];

UIVisualEffectView *visualEffectView = [[UIVisualEffectView alloc] initWithEffect:blurEffect];

visualEffectView.frame = imageView.bounds

[imageView addSubview:visualEffectView];
```

#### Swift

``` swift
var visualEffectView = UIVisualEffectView(effect: UIBlurEffect(style: .Light)) as UIVisualEffectView

visualEffectView.frame = imageView.bounds

imageView.addSubview(visualEffectView)
```

#### UIBlurEffect 的種類

UIBlurEffect 的種類則有以下幾種可以使用：

- **UIBlurEffectStyleExtraLight** - 模糊後加入和模糊對象相比更明亮的色相（ Hue ）調整效果
- **UIBlurEffectStyleLight** - 模糊後加入和模糊對象相等的色相調整效果
- **UIBlurEffectStyleDark** - 模糊後加入和模糊對象相比更暗色的色相調整效果

以下是分別的效果展示，前景加上原圖對照：

{% img center https://farm4.staticflickr.com/3839/14716950237_e2f1894b34_o.png %}

## 結論

原先使用 vImage 的 code 對想要對效果做客製化比較容易達成。而 iOS 8 新增的這個 visual effect 則是提供了最容易的達成方式，怎麼用就端看需求了。

而為了看懂 sample code 也稍微看了一些資料，和一些以前沒接觸過的運算公式，需要花了一些時間消化這些東西。不過花點時間了解他背後是怎麼做到的，也滿有收穫的。
