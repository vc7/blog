---
layout: post
title: "Sass 的 Shell 即時互動工具"
date: 2014-07-06 11:14:10 +0800
comments: true
categories: ["Web", "Sass"]
keywords: Web, CSS, Sass, Less, Programmable CSS, F2E
description: "Sass 和其他語言一樣，也有自己的即時互動工具，可以即時測試自己想要實驗的 Sass script 的結果。"
references: [{title: "Interactive Shell - Sass Documentation", link: "http://sass-lang.com/documentation/file.SASS_REFERENCE.html#interactive_shell"}, {title: "Functions - Sass Documentation", link: "http://sass-lang.com/documentation/Sass/Script/Functions.html"}, {title: "Interactive Sass: having fun on the terminal - The Sass Way", link: "http://thesassway.com/intermediate/interactive-sass-having-fun-on-the-terminal"}, {title: "Read-eval-print loop - Wikipedia", link: "http://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop"}]
hero: "https://farm3.staticflickr.com/2897/14583791372_33c417bd2e_o.png"
---

Sass 和其他語言一樣，也有自己的即時互動工具，可以即時測試自己想要實驗的 Sass script 的結果。

<!-- more -->

除了為了讓初學者可以測試和學習 Sass ，也可以讓開發者不用再新開檔案編譯，就可以測試自己寫的東西可不可行之後，再放進專案中。

## REPL

其他語言像是 Python 和 Ruby 等語言有自己的即時互動直譯器 (Interactive Interpreter)，這種方式做了以下幾件事情：

- Read a line from user
- Evaluate the line
- Result will be the output

這個過程則稱為 *Read-Eval-Print loop* ，簡稱為 [REPL](http://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop)。

## 進入和退出 Sass Interactive Shell

### 進入 Shell

輸入以下指令，就可以進入 Sass interactive shell 模式：

``` sh
$ sass -i
```

從 `sass -help` 中可以看到， interactive shell 的指令如下，這兩個方法都可以開啟：

``` sh
-i, --interactive                Run an interactive SassScript shell.
```

不過發現其實這個方法也打得開：

``` sh
$ sass -interactive
```

### 退出 Shell

如果要退出 shell 模式，只要按 `Ctrl + D` 組合鍵即可。


## 簡單使用

當進入 shell 裡面，在 terminal 中會變成兩個箭號：

``` sh
>>
```

這時候就可以輸入 SassScript 來測試，可以輸入字串、 運算和 function calls 等：

``` sass
>> "Hello 熊屋 !"
"Hello 熊屋 !"
>> 1 * 43 - 7
36
>> 1px + 1px + 1px
3px
```

也可以用內建的 [functions](http://sass-lang.com/documentation/Sass/Script/Functions.html)：

``` sass
>> invert(black)
#ffffff
>> lighten(tomato, 20%) 
#ffb9ad
>> if(1>2, black, white)
#ffffff
```

## 注意事項

### 型別衝突

不同的單位無法相加，會報錯，如：

``` sass
>> 1em + 1px
SyntaxError: Incompatible units: 'px' and 'em'.
```

### 宣告變數

在 shell 中不吃 `;` 結尾，所以在宣告變數的時候，寫完 expression 之後不用加分號結尾：

``` sass
>> $width: 5em;
SyntaxError: Invalid CSS after "5em": expected expression (e.g. 1px, bold), was ";"
>> $width: 5em
5em
```

### 進階的功能被限制

因為在 **Sass interactive shell 吃的是 expressions** ，除了無法吃像 mixin 、function 宣告，他也無法像 Ruby 的 irb 一樣有多行的 expressions 來組合自定義 functions：

``` sass
>> @function func() { @return 1 }
SyntaxError: Invalid CSS after "": expected expression (e.g. 1px, bold), was "@function func(..."
```

## 結論

Sass shell script 雖然有些固有的限制，但是還是可以讓我們開發者可以更方便的測試許多 SassScript 的 functions ：色彩計算、數學計算等複雜組合變化，並直接看到結果。

*本篇文章撰寫時使用 `Sass 3.3.9 (Maptastic Maple)` ，如果發現指令有改或更新，請在下方留言，謝謝。*