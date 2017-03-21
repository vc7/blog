---
layout: post
title: "讓 Touch Events 不被上方的透明 UIView 擋住"
date: 2014-07-04 22:09:12 +0800
comments: true
categories: ["iOS", "UIView"]
keywords: iOS, UIView, touch event, transparent
description: "之前在做專案的時候有發生，透明的 UIView 會擋住底層的 touch 事件。原因是當 touch event 觸發的時候，是以最上方可互動的 UI control 為優先，但是還是有辦法可以穿過的。"
references: [{title: "UIView - pointInside:withEvent: - Apple", link: "https://developer.apple.com/library/ios/documentation/uikit/reference/uiview_class/uiview/uiview.html#//apple_ref/occ/instm/UIView/pointInside:withEvent:"},{title: "How can I click a button behind a transparent UIView? - stackoverflow", link: "http://stackoverflow.com/a/4010809"}]
hero: "https://farm6.staticflickr.com/5314/14572355395_78bafe39a8_o.png"
---

之前在做專案的時候有發生，透明的 UIView 會擋住底層的 touch 事件。原因是當 touch event 觸發的時候，是以最上方可互動的 UI control 為優先，但是還是有辦法可以穿過的。

<!-- more -->

## 解法

解法就是在需要穿透的 UIView subclass 中 override `UIView` 的 `pointInside:withEvent:` method 來改變觸控到時的行為。

### 設定觸發 Touch Event 的條件

根據 Apple 的 Documentation ([ref.](https://developer.apple.com/library/ios/documentation/uikit/reference/uiview_class/uiview/uiview.html#//apple_ref/occ/instm/UIView/pointInside:withEvent:)) 中提到，這個 method 會回傳 BOOL value 。回傳 YES 則可以觸發該 view 的 touch event。

因此根據我的需求（*透明的不要觸發*）就設下以下條件，就會觸發 touch event ，如果沒有符合條件，則會自動再往後面一層的 UIView 觸發 touch event：

- view 不是 hidden
- view 的 alpha 大於 0
- view 可以互動 (userInteractionEnabled == YES)
- view 可以接收這個 touch event


``` objc
// In a UIView subclass

- (BOOL)pointInside:(CGPoint)point withEvent:(UIEvent *)event
{
    // 檢查自己
    if ([self checkInteractableWithView:self point:point event:event])
    {
        return YES;
    }

    // 檢查 subviews
    for (UIView *view in self.subviews) 
    {
        if ([self checkInteractableWithView:view point:point event:event])
        {
            return YES;
        }
    }

    return NO;
} 

- (BOOL)checkInteractableWithView:(UIView *)view point:(CGPoint)point event:(UIEvent *)event
{
    if ( ! view.hidden && 
        view.alpha > 0 && 
        view.userInteractionEnabled && 
        [view pointInside:[self convertPoint:point toView:view] withEvent:event])
    {
        return YES;
    }
    else
    {
        return NO;
    }
}

```

這樣就可以讓 touch events 穿過任何透明或無法互動的 `UIControl` 了。