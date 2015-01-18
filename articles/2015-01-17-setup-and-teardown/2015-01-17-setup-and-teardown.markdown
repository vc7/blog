- `setup` - 設置環境
- `teardown` - 清理環境

使用 `setup` 優點在於可以確保每次測試使用到的共同物件都是新的之外，也可以確保每次使用的新物件初始化的方式也是一樣的。

- 在測試中使用 `setup` 和 `teardown` 的特性來重用程式碼，例如新建和初始化所有測試都要用到的物件
- 不要在 `setup` 和 `teardown` 中初始化或清除並非所有測試都在使用的物件，否則會讓測試難以理解。閱讀程式碼的人不知道那些測試使用了 `setup` 裡面的邏輯，那些測試沒有使用。

# XCTest with Xcode

<i><small>in Xcode 5 and later</small></i>

在 XCTest Framework 中，除了以 instance method 的方式存在之外，開發者還可以加入 class method 形式的 `+ (void)setUp` 以及 `+ (void)tearDown` 。這兩個 methods 就會在該 class 中所有的 test methods 的最前以及最後執行。

# 參考資料
- [Test Class Structure | Apple](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/testing_with_xcode/testing_3_writing_test_classes/testing_3_writing_test_classes.html#//apple_ref/doc/uid/TP40014132-CH4-SW2]
