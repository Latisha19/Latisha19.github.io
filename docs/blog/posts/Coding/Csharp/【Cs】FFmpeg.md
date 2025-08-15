---
comments: true
categories: []
tags:
date: 2025-05-14 11:38:28
updated: 2025-08-15 10:30:55
---
## 文前言

使用的目的其實是要做 影片切割輸出圖片  
於是問了 GPT  
得到結合 FFmpeg 處理。  
因此產出。

*** 以下程式碼皆在 LinqPad5 .NET Framework 下 C# Program 測試過

<!-- more -->

## 主文

FFmpeg 前置處理請見：  
[FFMPEG 安裝教學(windows)](https://vocus.cc/article/64701a2cfd897800014daed0)

程式碼邏輯流程：

1. 給定影片路徑 `videoPath` 與輸出資料夾路徑 `outputFolder`
2. 呼叫 FFmpeg 指令，給予引數
3. 輸出處理結果

主要需要談的是 2. 的部分，其他都是基本 C# 功

```cs
string ffmpegPath = "ffmpeg"; // 如果未設定環境變數，請改為完整路徑
//格式化編號指令：
//	%：代表「格式化」的起始符號
//	0：數字不足時用 0 補齊
//	4：總共要有 4 位數
//	d：代表「整數（digit）」格式
string outputPattern = Path.Combine(outputFolder, "frame_%04d.png");	
string arguments 
	= $"-i \"{videoPath}\" -vf fps=24 -start_number 0  \"{outputPattern}\"";
	
ProcessStartInfo startInfo = new ProcessStartInfo
{
	FileName = ffmpegPath,
	Arguments = arguments,
	UseShellExecute = false,
	RedirectStandardOutput = true,
	RedirectStandardError = true,
	CreateNoWindow = true
};
```

引數：

- `fps=24`: 個人設定每秒切割 24 張
- `-start_number 0`: 輸出圖片檔 index 想從 0 開始
- "frame_%04d.png": 格式化編號指令
	- %：代表「格式化」的起始符號
	- 0：數字不足時用 0 補齊
	- 4：總共要有 4 位數
	- d：代表「整數（digit）」格式
	- => 以此結果：可輸出範圍 0000~9999


## UPDATE LOG

114.

- 0514 首次新增 