---
tags:
  - Codinggg/Tools/log
categories:
  - Codinggg
  - log
date: 2025-01-08 03:15:43
updated: 2025-07-25 09:52:25
---
## 文前言

基本上這篇是介紹各種我用過的東西  
包含：NLog、log GUI 工具等等。

<!-- more -->

## 主文

### NLog

ref: [NLog 教學：快速建立自訂日誌紀錄(基礎篇). 嗨嗨，各位好，今天要介紹的是 NLog 這個 logging… | by Tom | appxtech | Medium](https://medium.com/appxtech/nlog-%E6%95%99%E5%AD%B8-%E5%BF%AB%E9%80%9F%E5%BB%BA%E7%AB%8B%E8%87%AA%E8%A8%82%E6%97%A5%E8%AA%8C%E7%B4%80%E9%8C%84-%E5%9F%BA%E7%A4%8E%E7%AF%87-8b4f27739f30)  
(core 版)

- .NET Framework:
   1. nuget 裝 `NLog`、`NLog.Schema`
   2. 複製 [raw.githubusercontent.com/NLog/NLog/v4.4/src/NuGet/NLog.Config/content/NLog.config](https://raw.githubusercontent.com/NLog/NLog/v4.4/src/NuGet/NLog.Config/content/NLog.config) 內文字，在跟 `NLog.xsd` 檔案同層位置新增檔案 `NLog.config`，貼上網址內東東後修改成自己想要的
   3. `NLog.config` 的屬性 設定：`複製到輸出目錄 (Copy to Output Directory)` 為 `有更新才複製 (Copy if newer)`
   4. `public static NLog.Logger logger = NLog.LogManager.GetCurrentClassLogger();`


加上 stack trace 訊息：  
[c# - Customizing when to capture stack trace in NLog - Stack Overflow](https://stackoverflow.com/questions/53119784/customizing-when-to-capture-stack-trace-in-nlog)

### tools

1. Log Parser Studio
	- [Day24. 凡走過必留下痕跡 - Logging, IIS Log - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10305315)
	- [Log Parser 與 Log Parser Studio 介紹，快速搜尋iislog - HackMD](https://hackmd.io/@Not/LogParserStudio_with_IISLog)
		- 似乎用的不太成功？(指自訂 nlog 解析的部分)
2. Visual Log Parser
	- [Visual Log Parser](http://www.codeplex.com/visuallogparser)
	- [介紹好用工具：Visual Log Parser ( 視覺化操作 LP 語法 ) | The Will Will Web](https://blog.miniasp.com/post/2009/02/19/Useful-tool-Visual-Log-Parser)
	- [GitHub - carehart/visuallogparser: A copy of the code at https://archive.codeplex.com/?p=visuallogparser so I can keep it around](https://github.com/carehart/visuallogparser)
		- 過時軟體？
3. Log4View v2
	- 官網：[Log4View: log4view](https://www.log4view.com/)
		- 2025 還有更新，挺好用的，一個月免費試用
		- 據說過期後還會保留基本功能，拭目以待
		- 114/07/25 Edit: 
			- 過期了，保留的基本功能也挺好用的，大概會繼續用下去吧。
			- 但有時候 log 非常簡單的話會跑去開文書編輯器 [Notepad++](Notepad++.md#log 訊息顏色突出提示) 比較快開
			- 但若是用到比如 core api 之類的有一大堆其他系統訊息，開這隻比較看得懂 (?




---

## update log


114  
01.08 開新文章  
06.02 加上 [tools] 段落  
07.24 改換文章版面 (加上例行性 文前言&主文)  
07.25 更新 [tools/Log4View v2] 感想