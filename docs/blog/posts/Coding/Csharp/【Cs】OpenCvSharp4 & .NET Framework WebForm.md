---
comments: true
categories:
  - Codinggg
  - Tools
tags:
  - Codinggg/Lan/Cs
  - Codinggg/Tools
date: 2025-04-22 10:56:17
updated: 2025-08-15 10:33:46
---
## 文前言

寫這篇的目的是比較兩圖片間的差值並框出  
一開始只打算當作雜記，畢竟個人對 OpenCV 真的是相當不熟悉...

萬萬沒想到翻遍網路都沒查到  
OpenCvSharp4 & .NET Framework WebForm 究竟怎樣才不會爆
```
無法載入 DLL 'OpenCvSharpExtern': 找不到指定的模組。 (發生例外狀況於 HRESULT: 0x8007007E)
```
問題 (崩潰)  
(已嘗試多種網友解決法但 唉)

經由 ChatGPT 努力不懈成功解決。  
當然，也可能解決方法有其他問題  
但不論如何至少可以運作了 QQQ

OpenCvSharp4 官方文件：[Welcome to the OpenCvSharp](https://shimat.github.io/opencvsharp_docs/html/d69c29a1-7fb1-4f78-82e9-79be971c3d03.htm)

<!-- more -->

## 主文


### 前置

系統框架：4.8.1

使用版本：4.10.0.20241108  
安裝：
- OpenCvSharp4.Windows
	- 相依安裝：
		- [OpenCvSharp4](https://www.nuget.org/packages/OpenCvSharp4/)
		- [OpenCvSharp4.runtime.win](https://www.nuget.org/packages/OpenCvSharp4.runtime.win/)
		- [OpenCvSharp4.WpfExtensions](https://www.nuget.org/packages/OpenCvSharp4.WpfExtensions/)
- OpenCvSharp4.Extensions

於系統目錄下 `bin/x64` 資料夾內放上  
源於 `"C:\xxx\opencvsharp4.runtime.win\(版本號)\runtimes\win-x64\native\"`  
資料夾下的兩檔案：
- `OpenCvSharpExtern.dll`
- `opencv_videoio_ffmpeg4100_64.dll`

！！！重點！！！  
於 `Global.asax.cs` 內 `Application_Start()` 方法中加入：

```cs
// 放你 native DLL 的資料夾（建議直接放 bin\x64）
string nativePath = Server.MapPath("~/bin/x64");

// 將 native 資料夾加入到 DLL 搜尋路徑中
Environment.SetEnvironmentVariable(
	"PATH", 
	nativePath + ";" + Environment.GetEnvironmentVariable("PATH"), 
	EnvironmentVariableTarget.Process
);
```
跟網友說法只差別在這裡  
加上去就可以過了 qwq

### 測試

請在任一 `.aspx.cs` 內嘗試：

```cs
using (var mat = new Mat(100, 100, MatType.CV_8UC3, new Scalar(255, 0, 0)))
{
	Cv2.ImWrite("test.jpg", mat);
	//Console.WriteLine("Done.");
}
```

確認是否有任何失敗問題。

應當輸出：test.jpg，全藍色圖片


### 用法

(研究中...)


## UPDATE LOG

114.

04/22 開新篇

06/24 更正誤字、補上官方文件連結