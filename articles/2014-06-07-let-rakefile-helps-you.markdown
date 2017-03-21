---
layout: post
title: "讓 Rakefile 幫你做完一些雜事"
date: 2014-06-07 23:06:32 +0800
comments: true
categories: ["Rakefile", "自動化"] 
keywords: Rakefile, Automatic, CocoPods 
description: "簡單來說，Rakefile 就是 Ruby 的 Makefile，用標準的 Ruby 所寫成，並使用 rake 執行。使用者並可以自己指定要執行的 tasks ，所以可以用來 Deploy 網站、操作檔案等複雜的行為。"
references: 
hero: "https://farm3.staticflickr.com/2912/14180259148_1fd93bf68f_o.png"
---

> 其實 Ruby 是 Cocoa Development 的好朋友，真的

在看一些 iOS project repos 的時候，發現有的開發者會用 `Rakefile` 去幫自己的專案做一些自動化的事情，產生一些 dummy files 或設定檔：[nothingmagical/cheddar-ios 的 Rakefile](https://github.com/nothingmagical/cheddar-ios/blob/master/Rakefile)。

<!-- more -->

## Rakefile 是什麼？

簡單來說，Rakefile 就是 Ruby 的 Makefile，用標準的 Ruby 所寫成，並使用 rake 執行。使用者並可以自己指定要執行的 tasks ，所以可以用來 Deploy 網站、操作檔案等複雜的行為。有關 rake 詳細的安裝和說明可以先參考 [官方網站](http://rake.rubyforge.org/)。

一開始看到會有很像用 PHP 去執行 terminal commands 的感覺，只不過 rake 提供了更直覺的介面幫開發者做這個設定的事情。

## 懶惰是科技的泉源

因此，Rakefile 可以直接執行 bash 指令。於是今天就自己來實作一下，我的需求則是執行 Rakefile 能幫我同時做 iOS Project 的 CocoaPods 及 git submodule 的安裝和更新。

每一次在 pull remote repo ，或是要更新 private repo 等都有可能需要重新執行這兩個工具的相關指令，就把他們包成簡單的 Rakefile，只要執行 rake 一次就可以幫我同時做完這些事情。在切換 branch 的時候都有可能需要重新部署 pod，或是檢查 submodule 是否有更新而自動進行更新。

``` ruby
desc 'pod install'
task :setup_pod do
	puts 'Installing and updating pods libraries...'
		`pod install`
end
		 
desc 'setup git submodule'
task :setup_submodules => :setup_pod do
	puts 'Updating submodules...'
	`git submodule update --init --recursive`
end
				 
desc 'Finish message'
task :setup_done => :setup_submodules do
	puts 'Done! You can start now!'
end
					 
task :default => :setup_done
```

根據一些範例，得出如上的 Rakefile 

可以用 ` `` ` 把 bash 指令包起來，就可以執行更多 command line 複雜的動作。

`=>` 後面則是接前一個 task 名稱，這樣子就可以把各 tasks 連接起來自動執行

詳細可以看 [Rakefile 的 documentation](http://rake.rubyforge.org/doc/rakefile_rdoc.html) ，會有更多的指令，
有空再來寫詳細一點的教學 
