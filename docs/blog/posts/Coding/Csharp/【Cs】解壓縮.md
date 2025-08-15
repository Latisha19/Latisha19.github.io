---
comments: true
tags:
  - Codinggg/Lan/Cs
categories:
  - Codinggg
date: 2025-01-24 10:34:39
updated: 2025-08-15 10:31:14
---
原先是嘗試使用其他的開源解壓縮  
測試壓縮檔內有一千多個檔案，解了五分鐘以上，決定換掉  
經資深提示，去尋找 7-Zip 的 dll  
找了一些方法，最終嘗試是 SevenZipExtractor。  
官網：[adoconnection/SevenZipExtractor: C# wrapper for 7z.dll](https://github.com/adoconnection/SevenZipExtractor)

<!-- more -->

SevenZipExtractor 據官方描述是 7-Zip dll 的 c# 版包裝器  
同樣的測試壓縮檔，解壓縮約 40 秒，效果相當不錯。  

解壓縮寫法請參照：  
[Decompression is too slow · Issue #66 · adoconnection/SevenZipExtractor](https://github.com/adoconnection/SevenZipExtractor/issues/66)  
