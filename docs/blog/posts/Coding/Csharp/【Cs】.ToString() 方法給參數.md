---
comments: true
categories: []
tags:
date: 2025-11-14 11:46:06
updated: 2025-11-25 17:07:56
---
## 來自網路的整理表格

表格 ref: [[C#]常用ToString()方法 | 程式雜記](https://cshartuc.blogspot.com/2011/12/tostring-tostring-intdoubletostring-c-2.html)

<!-- more -->

| 參數  | 應用      | 含義           | 示例                                       |
| --- | ------- | ------------ | ---------------------------------------- |
| C   | 數字類型    | 專用場合的貨幣值     | $4834.50 (USA)<br><br>£4834.50 (UK)      |
| D   | 只用於整數類型 | 一般的整數        | 4834                                     |
| E   | 數字類型    | 科學計數法        | 4.834E+003                               |
| F   | 數字類型    | 小數點後的位數固定    | 4384.50                                  |
| G   | 數字類型    | 一般的數字        | 4384.5                                   |
| N   | 數字類型    | 通常是專用場合的數字格式 | 4,384.50 (UK/USA)<br><br>4 384,50 (歐洲大陸) |
| P   | 數字類型    | 百分比計數法       | 432,000.00%                              |
| X   | 只用於整數類型 | 16進制格式       | 1120 (如果要顯示0x1120，需要寫上0x)                |

## me 研究

N: (只保留純數字？)

```cs
var aaa = Guid.NewGuid();
aaa.Dump();
aaa.ToString("N").Dump();

// output:
//10b05e38-774c-4acd-9dfb-ade68b17f69e
//10b05e38774c4acd9dfbade68b17f69e
```