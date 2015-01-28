# 把 podspec 轉成 json

最近在寫使用 CocoaPods 作為管理工具而用的 library 時，要把 podspec 轉成 .podspec.json 格式，就上網找了找該怎麼轉

<!-- more -->


``` sh
$ pod ipc spec LibraryName.podspec >> LibraryName.podspec.json
```

# 參考資料

http://www.qaster.com/q/474051332920209408/How+to+convert+podspec+to+podspecjson+from+console