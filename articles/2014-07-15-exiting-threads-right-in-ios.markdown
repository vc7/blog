---
layout: post
title: "Concurrency - 確保退出目前的 Thread 並不繼續執行"
date: 2014-07-15 13:50:15 +0800
comments: true
categories: ["iOS", "Concurrency", "Thread"]
keywords: iOS, Concurrency, Thread, 執行緒, 多工, 並行
description: "要退出某個 thread 很簡單，但是還是要注意雖然退出了，有執行時間差的關係，還是會去執行到原先的 statements 。"
references: []
hero: "https://farm4.staticflickr.com/3859/14472707898_be71fec0a8_o.png"
---

要退出某個 thread 很簡單，但是還是要注意雖然退出了，有執行時間差的關係，還是會去執行到原先的 statements 。

<!-- more -->

## 解法

在 `NSThread` 中，如果使用 `exit` 潛在會造成 leaking 資源，因此應該要使用 `cancel` 來退出 thread 。

``` objc
NSThread *thread = /* 取得 thread 的 reference */;
[thread cancel];
```

## 實際使用

在專案中實際使用的時候，如非同步處理不同的運算，就算一個 thread 被 cancel 了，他在退出前還是會把這個執行緒該做的事情做完。

這裡有個範例的程式碼，今天所有的程式碼都會在 appDelegate.m 裡面撰寫：

``` objc
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    self.myThread = [[NSThread alloc] initWithTarget:self
                                            selector:@selector(threadEntryPoint)
                                              object:nil];
    
    // 在 Delay 3 秒鐘之後，離開 myThread
    [self performSelector:@selector(stopThread) withObject:nil afterDelay:3.0f];
    
    [self.myThread start];
    
    self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
    // Override point for customization after application launch.
    self.window.backgroundColor = [UIColor whiteColor];
    [self.window makeKeyAndVisible];
    return YES;
}

#pragma mark - Thread Operations

- (void)threadEntryPoint
{
    @autoreleasepool {
        NSLog(@"Thread Entry Point");
        
        while ([[NSThread currentThread] isCancelled] == NO) {
            
            // 每 sleep 4 秒才會被 fired
            [NSThread sleepForTimeInterval:4.0f];
            
            NSLog(@"Thread Loop");
        }
        
        NSLog(@"Thread Finished");
    }
}

- (void)stopThread {
    NSLog(@"Thread Canceling");
    [self.myThread cancel];
    
    NSLog(@"Thread Releasing");
    self.myThread = nil;
}
```

這裡做的事情是：

1. `self.myThread` 被初始化的時候，就指定要在這個 thread 執行 `- threadEntryPoint` 這個 method 。
2. 這時候因為以上第 29 行的 sleep 4 秒，每四秒才會印一次 "Thread Loop"
3. 當 `self.myThread` 初始化後，自己在另外一個 thread 讀秒的時候，在原有的 thread 上指定 delay 3 秒之後退出 `self.myThread`
4. 開始執行 `self.myThread` ，這時候還沒有 3 秒，所以 while 的條件滿足，因此會執行 while 的內容。這裡的 `currentThread` 就是指 `self.myThread` ，因為在 thread 初始化的時候就被指定了。

### 初步執行結果：

``` sh
...
Thread Entry Point
Thread Canceling
Thread Releasing
Thread Loop
Thread Finished
...
```

這時候就會發現，雖然已經 cancelled 甚至 released ，在最後還是會印出 thread 中執行的兩個 NSLog。就印證就算已經退出了一個 thread ，thread 原有的程式還是會執行完的。

#### 目前為止的程式碼

- File: [VCAppDelegate.m at 10d666d](https://github.com/kumayast/Exiting-Thread/blob/10d666d56ca803f6c9680ae95bdade1d64302675/exitingThreads/VCAppDelegate.m)


### 解決辦法

這個情形其實滿常見的，雖然在 while 的時候會去檢查 thread 是不是已經被 cancelled 了，但是當在經過一段時間等待之後、實際執行之前，並沒有檢查是不是已經被 cancelled 了。

因此只要在等待之後，開始執行該執行的 statement 的之前，加上判斷就可以了：

``` objc
- (void)threadEntryPoint
{
    @autoreleasepool {
        NSLog(@"Thread Entry Point");
        
        while ([[NSThread currentThread] isCancelled] == NO) {
            
            [NSThread sleepForTimeInterval:4.0f];
            
            // 執行前先判斷是否已經被 cancelled 了
            if ([[NSThread currentThread] isCancelled] == NO) {
                NSLog(@"Thread Loop");
            }

        }
        
        NSLog(@"Thread Finished");
    }
}
```

再次執行就會看到， while loop 裡面的東西經過判斷後，就不會被跑到了：

``` sh
...
Thread Entry Point
Thread Canceling
Thread Releasing
Thread Finished
...
```

#### 最後的程式碼

- File: [VCAppDelegate.m at a87aa27](https://github.com/kumayast/Exiting-Thread/blob/a87aa27701ae6312e431d107e097bab348df7cb9/exitingThreads/VCAppDelegate.m)
- Diff: [commit a87aa27](https://github.com/kumayast/Exiting-Thread/commit/a87aa27701ae6312e431d107e097bab348df7cb9)

## 範例 Repo

我有把這篇文章的專案在 Github 建一個 repo ，可以 clone 下來玩玩看 :D

Repo: [https://github.com/kumayast/Exiting-Thread](https://github.com/kumayast/Exiting-Thread)

*本篇文章撰寫時使用 Xcode 5.1.1 以及 iOS SDK 7.1 on Mac OSX 10.10 beta 2 ，如果發現相關內容有更新，請在下方留言，謝謝。*