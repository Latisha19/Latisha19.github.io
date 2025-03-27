---
categories:
  - Codinggg
tags:
  - Codinggg/Lan/Cs
  - Codinggg/LINQ
date: 2025-02-13 03:02:02
updated: 2025-03-27 15:30:58
---
在 LINQ 寫 `.Where` 做 `.Contains`  
依照官方說法  
[String.Contains Method (System) | Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/api/system.string.contains?view=net-9.0#system-string-contains%28system-string%29)  
此方法會區分大小寫

<!-- more -->

然而轉成 SQL 會得到 `LOCATE()`  
[MySQL LOCATE() Function](https://www.w3schools.com/SQl/func_mysql_locate.asp)  
變成不區分大小寫的搜尋法

還挺有趣的ww  
對個人而言，因為沒有需要區分大小寫，反而不區分比較方便，因此用的愉快  
但若是未來有需求，可能到時候再看看如何處理了。