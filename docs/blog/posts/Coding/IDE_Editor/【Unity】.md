---
comments: true
categories:
  - Codinggg
tags:
  - Codinggg/Lan/Cs
  - Codinggg/Unity
date: 2025-04-14 17:25:29
updated: 2025-08-15 10:31:32
---
## 文前言

是也有想過說有一天可能需要碰到 Unity  
但還真沒想到碰到的時候先學的不是語法，而是新專案爆掉怎麼修...= =

<!-- more -->

## 主文

- 任何套件掛掉問題：
	- 先刪掉 Library 資料夾
	- 失敗：再跑去 Program Files/Unity 內刪掉 cache 資料夾試試看
- Assets 要修改前先關掉 Editor 再大改動 否則可能卡住


### debug

VS2022  
ref: [How to debug code with Microsoft Visual Studio 2022 | Unity](https://unity.com/how-to/debugging-with-microsoft-visual-studio-2022)

### Game

播放：[Play]  
暫停：[Pause]  
繼續播放：[Pause]  
停止：[Play]

### build

1. 使用 UI 設定：  
	嘗試參考 [[Unity 教學] 輸出至 Windows 平台 (.exe) - 偵錯桐人](https://tedliou.com/unity/build-for-windows/)
	
	[Player Settings...] > [Resolution and Presentation]>
	
	[Resolution] > 
	- [Fullscreen Mode] : [Windowed]
	- [Run In Background] : false
	
	[Standalone Player Options] >
	- [Resizable Window] : true
	- [Visible In Background] : false
	- [Allow Fullscreen Switch] : true
	
	結果：
	
	![](../../../../assets/images/Unity_build%20exe%20screen.png)

2. 使用程式設定  
   ref: [Unity - Scripting API: Screen.SetResolution](https://docs.unity3d.com/ScriptReference/Screen.SetResolution.html)