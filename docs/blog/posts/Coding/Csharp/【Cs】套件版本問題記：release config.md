---
comments: true
categories: []
tags:
date: 2025-08-01 15:42:03
updated: 2025-08-051 1:36:091:00
---
## 文前言

也許這個問題對於恰好找來這篇的人不會有用，也可能有用...

前提：主程式有開兩種 config，其中 release config 是手動複製過去的

<!-- more -->

## 主文


(以下皆是 Framework)  

### 起因：

主程式：WinForm (AAA 執行時呼叫 XXX dll)  
呼叫 dll：類別庫 (XXX dll 執行時呼叫 OOO dll)

原本在本機跑的都好好的，但不知為何轉 release 重建後塞到正式機時開始爆掉：

```log
System.IO.FileLoadException: 
無法載入檔案或組件 
'System.Memory, Version=4.0.1.2, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51' 
或其相依性的其中之一。 找到的組件資訊清單定義與組件參考不符。 
(發生例外狀況於 HRESULT: 0x80131040) 

檔案名稱: 
'System.Memory, Version=4.0.1.2, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51' 

於 UglyToad.PdfPig.Parser.PdfDocumentFactory.Open(Byte[] fileBytes, ParsingOptions options) 

於 UglyToad.PdfPig.Parser.PdfDocumentFactory.Open(String filename, ParsingOptions options) 
於 C:\git\csharp\PdfPig\src\UglyToad.PdfPig\Parser\PdfDocumentFactory.cs: 行 38 

於 XXX(...) 
於 D:\XXX.cs: 行 244 

於 XXX..ctor(...) 
於 D:\XXX.cs: 行 77 

於 OOO(...) 
於 D:\OOO.cs: 行 325 

警告: 組件繫結記錄切換為 OFF。 若要記錄組件繫結失敗，請將登錄值 [HKLM\Software\Microsoft\Fusion!EnableLog] (DWORD) 設為 1。 注意: 與組件繫結失敗記錄相關的效能會有部分負面影響。 若要關閉此功能，請移除登錄值 [HKLM\Software\Microsoft\Fusion!EnableLog]。
```


```log
System.IO.FileLoadException: 
無法載入檔案或組件 
'System.Buffers, Version=4.0.3.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51' 
或其相依性的其中之一。 找到的組件資訊清單定義與組件參考不符。 
(發生例外狀況於 HRESULT: 0x80131040)

檔案名稱: 
'System.Buffers, Version=4.0.3.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'


   於 PDFtoImage.Internals.NativeMethods.FPDF_GetBlock(IntPtr param, UInt32 position, IntPtr buffer, UInt32 size)
   
   於 PDFtoImage.Internals.NativeMethods.Imports.FPDF_LoadCustomDocument(IntPtr access, String password)
   
   於 PDFtoImage.Internals.NativeMethods.FPDF_LoadCustomDocument(Stream input, String password, Int32 id)
   
   於 PDFtoImage.Internals.PdfFile..ctor(Stream stream, String password, Boolean disposeStream)
   
   於 PDFtoImage.Internals.PdfDocument..ctor(Stream stream, String password, Boolean disposeStream)
   
   於 PDFtoImage.Internals.PdfDocument.Load(Stream stream, String password, Boolean disposeStream)
   
   於 PDFtoImage.Conversion.ToImage(Stream pdfStream, Index page, Boolean leaveOpen, String password, RenderOptions options)
   
   於 PDFtoImage.Conversion.SaveImpl(Stream stream, SKEncodedImageFormat format, Stream pdfStream, Index page, Boolean leaveOpen, String password, RenderOptions options)
   
   於 PDFtoImage.Conversion.SaveImpl(String filename, SKEncodedImageFormat format, Stream pdfStream, Index page, Boolean leaveOpen, String password, RenderOptions options)
   
   於 PDFtoImage.Conversion.SaveJpeg(String imageFilename, Stream pdfStream, Index page, Boolean leaveOpen, String password, RenderOptions options)
   
   於 OOO(...) 於 D:\OOO.cs: 行 665
   
   於 AAA.<aaa>d__2.MoveNext() 於 D:\AAA.cs: 行 130

警告: 組件繫結記錄切換為 OFF。
若要記錄組件繫結失敗，請將登錄值 [HKLM\Software\Microsoft\Fusion!EnableLog] (DWORD) 設為 1。
注意: 與組件繫結失敗記錄相關的效能會有部分負面影響。
若要關閉此功能，請移除登錄值 [HKLM\Software\Microsoft\Fusion!EnableLog]。
```


今天上午爆掉，研究大概半天，下午終於找到解法。

### 研究

此問題牽涉了兩個層面：

首先，UglyToad.PdfPig 版本是 0.1.8，翻了套件更新紀錄後決定更新至 0.1.9，因為該版本有涉及 `System.Memory`。
   > [Release Red Wattle Hog · UglyToad/PdfPig](https://github.com/UglyToad/PdfPig/releases/tag/v0.1.9)

(該套件只用在最底下的 OOO dll 內，所以只用更新他就好)

更新完畢再跑一次，咦，問題還是一模一樣？


中間有跑去翻網路找到了：[c# - Could not load file or assembly 'System.Memory, Version=4.0.1.' in Visual Studio 2015 - Stack Overflow](https://stackoverflow.com/a/60087926)  
(連結會導到其中一個答案)

雖然沒有解決完全，但也不確定是否因為包含做了這件事才完全成功的？

### 解決！

最終發現：

原來是 release config 當初是手動複製過去，因此無法像一般 config 那樣自動配合 dll 更新 runtime 區塊 (也就是包含一堆關於套件版本的狀況)

因此再把 release config 內 runtime 區塊用一般 config 的 runtime 區塊取代就好了。

順帶一提發現第二個問題的起因是爆掉訊息內有：  
`套件名稱, Version=版本, Culture=neutral, PublicKeyToken=xxx`

直覺不夠，搜尋來湊  
翻到了 config 內有用這種寫法  
因此打開正式機下的 config 跟本機 debug config 比較後就破案啦！


## UPDATE LOG

114.

08/01 開篇