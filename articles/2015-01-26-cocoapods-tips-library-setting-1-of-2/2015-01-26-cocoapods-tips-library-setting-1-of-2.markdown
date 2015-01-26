# CocoaPods 小技巧 - Library 設定 (1/2)

[CocoaPods](http://cocoapods.org/) 是 iOS 的一個函式庫相依管理的工具，在日常開發中或是撰寫函式庫都有用到，今天來對 library 在 Podfile 中可用的設定方法做個筆記。

<!-- more -->

今天 library 以最常用到的 AFNetworking 為例。

# 最基本的使用方式

``` ruby
pod 'AFNetworking'
```

最基本的使用方式，`pod` 加上 library name 即可搞定

## 版本指定

版本指定是最基本的指定方式之一，由於 CocoaPods 的是用 ruby 寫成，因此他用了 RubyGem 的方式來指定版本

``` ruby
pod 'AFNetworking', '2.0' # 指定使用 2.0 版本
```

### >, >=,  <, <=

除了最基本的指定版本，可以運用邏輯運算元來指定版本：

- `'> 0.1'` - 大於 0.1 的版本
- `'>= 0.1'` - 大於等於 0.1 的版本
- `'< 0.1'` - 小於 0.1 的版本
- `'<= 0.1'` - 小於等於 0.1 以下的版本

### ~>

CocoaPods 還有一個邏輯運算元 ~> 可以有比較不一樣的比較方式，是根據 semantic versioning 的方式去比較版本號並控制範圍。

- `'~> 0.1.2'` - 包含 0.1.2 到 0.2 之間的版本，不含 0.2 及以上
- `'~> 0.1'` - 包含 0.1 到 1.0 之間的版本，不含 1.0 及以上

### 版本設定參考

- [Semantic Versioning](http://semver.org/)
- [RubyGems Versioning Policies](http://guides.rubygems.org/patterns/#semantic-versioning)

# 參考資料

- [CocoaPods Guides - The Podfile](http://guides.cocoapods.org/using/the-podfile.html)

## Qiita 同步發布

- [CocoaPods 小技巧 - Library 設定 (1/2)](http://qiita.com/vc7/items/c21a04e6644c6037d39c)