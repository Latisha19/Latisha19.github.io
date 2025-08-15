---
comments: true
categories:
  - Codinggg
  - Tools
tags:
  - Codinggg/Lan/Cs
  - Codinggg/Tools
date: 2025-03-25 11:57:37
updated: 2025-08-15 10:34:17
---
## 文前言

本篇有包含向 ChatGPT 詢問後的結果，並結合個人實際使用感想

<!-- more -->
## 主文

需求：給定模板 .doc，要求產出需求格式的 .doc  
模板是空表格，有固定行列數及 cell 寬與高。cell 內要求塞入圖片。

.doc 要處理，直接詢問 ChatGPT，得到了：
1. `Microsoft.Office.Interop.Word`，但不建議
2. `DocumentFormat.OpenXml`，建議但要改用 .docx。

基於懶得繞遠路的理念嘗試了第一種...

### `Microsoft.Office.Interop.Word`

類似直接使用 word 應用程式執行的感覺。  
所以要記得處理解鎖 `winword.exe` 避免發生檔案被它鎖住的問題

實際嘗試才發現，救命啊他的設定好複雜！！！  
個人使用 WebForm + IIS 架站  
程式碼本身還好，但架站開始是大惡夢...

網路資源參考：
1. [網頁伺服器端Word合併列印實作(.NET C#)](https://www.cc.ntu.edu.tw/chinese/epaper/home/20231220_006708.html)
2. [[COM]為什麼Office在DCOM設定中找不到 | 亂馬客 - 點部落](https://dotblogs.com.tw/rainmaker/2012/12/13/85621)
3. [在DCOM設定中找不到 Microsoft Word - HackMD](https://hackmd.io/@aO674exYRgWnn1BgZhND9A/SJUSZQ8Ss)
4. [Microsoft.Office.Interop.Excel 的 IIS 設定 - HsuTingHuan - Medium](https://medium.com/@s780609/microsoft-office-interop-excel-%E7%9A%84-iis-%E8%A8%AD%E5%AE%9A-0a7a48e0cc1a)

簡單來說，配置太過複雜，而且已經超出我的知識領域範圍ㄌ...  
嘗試以上四點配置但完全失敗。  
[識別身分] 只能選擇 特定使用者，個人選擇 [互動式使用者] 會變回未授權。

種種情況造成最終決定放棄。  
(歷時一個禮拜)

### `DocumentFormat.OpenXml`

使用 dll 處理。

基本 is
```cs
using DocumentFormat.OpenXml;
using DocumentFormat.OpenXml.Packaging;
using DocumentFormat.OpenXml.Wordprocessing;
```

把模板改成 .docx + 此套件馬上取得大成功。  
歷時兩天解決，而且不須更動任何當時架站時的配置！！！

- 表格塞圖片 ref: [程式範例 - C# 動態產生 Word 表格-黑暗執行緒](https://blog.darkthread.net/blog/openxml-word-table-example/)

- 尺寸轉換 ref: [Points, inches and Emus: Measuring units in Office Open XML – Lars Corneliussen](https://startbigthinksmall.wordpress.com/2010/01/04/points-inches-and-emus-measuring-units-in-office-open-xml/)

- ChatGPT: 取得實際 table 內 cell 寬 (dxa 轉 EMU)
	- 但實際上總覺得沒那麼好用...？(個人是有手動設定 TABLE CELL 的實際寬高 cm)
	
```cs
private static long GetTableCellWidthInEMU(DocumentFormat.OpenXml.Wordprocessing.TableCell cell)
{
	// 確保 TableCell 中包含 TableCellProperties
	TableCellProperties cellProperties = cell.GetFirstChild<TableCellProperties>();
	if (cellProperties != null)
	{
		// 嘗試取得 TableCellWidth 屬性
		TableCellWidth cellWidth = cellProperties.GetFirstChild<TableCellWidth>();
		if (cellWidth != null && cellWidth.Type == TableWidthUnitValues.Dxa)
		{
			// 將 StringValue 轉換為 int
			int widthInDxa = int.Parse(cellWidth.Width);

			// 1 dxa = 20 EMU，轉換為 EMU
			return widthInDxa * 20;
		}
	}
	return 0; // 若無法找到合適的寬度屬性，返回 0
}
```


- 另外因為有旋轉圖片再塞入的需求，因此另外上網查了，也問過 ChatGPT。
	- 最終決定採用：[c# - Rotate image (byte array) in datatable - Stack Overflow](https://stackoverflow.com/questions/25569827/rotate-image-byte-array-in-datatable) 內的接受答案。


## 後言

畢竟順帶用了 ChatGPT，為了第一種方式還把它問爆掉只好轉向 Claude 繼續問 QQ  
對個人而言，還是比較習慣 ChatGPT。(以這兩種 AI 比較下)

但總不可能所有的程式碼都交給 AI 寫吧...基本思辨能力還是要有的。  
所以差不多理解後，就會轉為完全不問 AI 改純自己改寫後續了。  
後續維護也是。