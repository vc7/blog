# 用 NSAttributedString 在文字行中加入圖片
<center><p><img src="https://farm9.staticflickr.com/8633/16457106725_f948a2da25_o.png"><br><small>Facebook iOS app version 23.1<br>...</small></p></center>

最近在公司的專案中需要做到這種 UI ：在行內中插入小 icon ，就如上圖中 Facebook app 中呈現各種小圖的方式（紅色箭頭所指處）。一樣在 stackoverflow 上面找到再也簡單不過的解法，一樣要請出萬能的 NSAttributedString 。

<!-- more -->

# 概念

概念其實不會很複雜

- 把一個字串加入一個特定字串當標的物
- 把特定字符換成小圖

接著就可以來做做看了。

# 實作

結果會以一個 UILabel 顯示出來，因此當然要宣告一個 UILabel 來顯示，最後再將組合好的字串， assign 給這個 UILabel 的 `attributedText` property 。

<center><p><small><img src="https://farm8.staticflickr.com/7327/16277547789_69088e38e3_o.png"><br>結果圖<br>...</small></p></center>

這次要設法將字串中的笑臉文字 `:)` 把它換成 Android 裡面笑臉的表情圖，而這個 `:)` 就是要拿來作為替換的標的物字串。

# 建立基本的 UILabel

一開始在範例中先建立一個 UILable ，輸入想要顯示的字串，並加入主要的 view 中：

``` swift
class ViewController: UIViewController {

    var label: UILabel?
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        label = UILabel(frame: CGRectMake(0, 0, CGRectGetWidth(view.bounds), CGRectGetHeight(view.bounds)))
        label?.center = view.center
        label?.textAlignment = .Center
        
        var mutableAttributedString = NSMutableAttributedString(string: "this is a smile :)")
        
        label?.attributedText = mutableAttributedString
        
        view.addSubview(label!)
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }
}
```

<div style="text-align:right;"><small><span style="color:gray">這一階段的成果：Swift: <a href="https://github.com/kumayaco/inlineImageDemoSwift/blob/5e3726a/inlineImageDemoSwift/ViewController.swift#L11-L33">5e3726a</a>, Objective-C: <a href="https://github.com/kumayaco/inlineImageDemoObjectiveC/blob/47b4e7a/inlineImageDemoObjectiveC/ViewController.m#L19-L31">47b4e7a</a></span></small></div>

<h1> 宣告圖片的容器 <small>&gt; NSTextAttachment</small></h1>

接著宣告並初始化一個 NSTextAttachment 物件來裝要顯示的圖片，這個物件在下個步驟就會被置換到字串裡並顯示出來。這個步驟完成後目標文字還沒有被替換掉，因此這個時候執行還是不會顯示圖片喔。

``` swift
var textAttachment = NSTextAttachment()
var image = UIImage(named: "1f601.png")
        
textAttachment.image = image
```

（這邊因為用到的原圖比較大，在範例檔中我有多加上一個縮小圖片的處理）

<div style="text-align:right;"><small><span style="color:gray">這一階段的成果：Swift: <a href="https://github.com/kumayaco/inlineImageDemoSwift/blob/26d1856/inlineImageDemoSwift/ViewController.swift#L24-L27">26d1856</a>, Objective-C: <a href="https://github.com/kumayaco/inlineImageDemoObjectiveC/blob/f8c2e97/inlineImageDemoObjectiveC/ViewController.m#L28-L31">f8c2e97</a></span></small></div>

<h1> 換上圖片 <small>&gt; replaceCharactersInRange</small></h1>

最後將目標文字找到後換上圖片就完成了，

``` swift
var iconAttributedString = NSAttributedString(attachment: textAttachment)
mutableAttributedString.replaceCharactersInRange(NSMakeRange(16, 2), withAttributedString: iconAttributedString)
```

<div style="text-align:right;"><small><span style="color:gray">這一階段的成果：Swift: <a href="https://github.com/kumayaco/inlineImageDemoSwift/blob/bc8e161/inlineImageDemoSwift/ViewController.swift#L29-L31">bc8e161</a>, Objective-C: <a href="https://github.com/kumayaco/inlineImageDemoObjectiveC/blob/d6d46c1/inlineImageDemoObjectiveC/ViewController.m#L33-L35">d6d46c1</a></span></small></div>

<h1>範例專案 <small>Demo Project</small></h1>

- **Swift:** [[GitHub](https://github.com/kumayaco/inlineImageDemoSwift)] [[Download](https://github.com/kumayaco/inlineImageDemoSwift/archive/master.zip)]
- **Objective-C:** [[GitHub](https://github.com/kumayaco/inlineImageDemoObjectiveC)] [[Download](https://github.com/kumayaco/inlineImageDemoObjectiveC/archive/master.zip)]

# 後記

真心覺得 NSAttributedString 真的太強大，可以做好多事情。因此把圖片塞進字裡行間中這件事情，比原先想像中的容易好多。

再來可能是對 Swift 還不是很熟，在寫範例專案的時候碰到了很多困難，寫起來也還不像寫 Objective-C 還直觀，還要多多練習 :DDD

# 參考連結

- [NSAttributedSring - Apple Developer](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSAttributedString_Class/index.html)
- [NSMutableAttributedSring - Apple Developer](https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSMutableAttributedString_Class/index.html#//apple_ref/occ/cl/NSMutableAttributedString)
- [NSTextAttachment - Apple Developer](https://developer.apple.com/library/ios/documentation/UIKit/Reference/NSTextAttachment_Class_TextKit/)

## 同步發佈

- [用 NSAttributedString 在文字行中加入圖片 - 熊屋](http://blog.kumaya.co/2015/02/07/insert-image-inline-with-nsattributedstring/)
- [用 NSAttributedString 在文字行中加入圖片 - Qiita](http://qiita.com/vc7/items/4fae890b363348eb335d)

<hr>

<small><span>
本文範例以 Xcode Version 6.1.1 (6A2008a), iOS SDK 8.1 建置。<br>
專案裡 demo 用的 Android 表情圖 - Copyright © 2008 The Android Open Source Project. Licensed under the <a href="http://www.apache.org/licenses/LICENSE-2.0">Apache License</a> and includes this <a href="https://s3-eu-west-1.amazonaws.com/tw-font/android/NOTICE">notice</a>.
<span></small>