這一系列的文章英文標題是 "UIKit Programmatically" ，中文的話標題是 "用 Code 寫 UI" 。將會是一系列如何用純程式碼構成 iOS UI 的文章。

<!-- more -->

# 小歷史

在 2012 年底參加比賽的 iOS app 以及接下來的工作有幾的專案，都有碰過是用 storyboard 或是 xib 構成 UI 。這時候有察覺到 Xcode 會自動幫開發者初始化並設定一些 UI 物件；甚至為按鈕加上觸控事件都有辦法一併解決。

但是回頭看過去的作法，寫 JavaScript 的時候，按鈕事件當然都是用 code 加上去的、 UI 是用 HTML 加上去、畫面風格則是用 css 加上去的，那為什麼搬到 Xcode 上來就要用 GUI 工具把 UI 慢慢拉出來？

# 自己的學習過程

於是就在邊做專案、邊用 storyboard 的時候，就邊去查目前做的這個動作，怎麼只用程式碼達成。

# 有什麼好處

你可以知道一個 UIView 物件的 life cycle 是什麼，進而可以去在自己想要的階段去控制他們。甚至可以並用 lazy init view 的做法，再把某個 UI 元件初始化出來，不需要的時候可以讓 ARC 直接收走。可控制度就高很多了。

# 你要注意什麼

初期要花非常多時間序反覆練習怎麼用 code 寫 UI ，一開始不熟悉的話也不要輕易直接導入專案使用。但是在習慣這個寫法之後，會發現以前在用 storyboard 的時候，怎麼多做了一些看起來不必要的設定（至少我自己覺得是這樣啦）。

# 可以先預習一下

建議沒事就去看看過去幾年 WWDC, Tech Talks 所有有關 UIKit 的 sessions 。沒錯，是所有，看過之後勝過看所有 UIKit 不知道看多久。

一開始會從怎麼把 Xcode 6 的專案，轉換成不用 storyboard 的專案，接著就會逐篇帶過一些常用的 UIView 子類別。也盡量都會提供 Swift 和 Objective-C 的 sample code ，敬請期待唄！