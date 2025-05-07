---
categories:
  - Codinggg
  - Tools
tags:
  - Codinggg/Lan/js
  - Codinggg/Tools
  - Codinggg/Tools/Bootstrap
date: 2025-03-27 11:38:19
updated: 2025-03-27 15:51:32
---
官網：[Tabulator | JavaScript Tables & Data Grids](https://tabulator.info/)  
使用版本：v6.3

官方文件：[Documentation | Tabulator](https://tabulator.info/docs/6.3)  
官方 demo: [Examples | Tabulator](https://tabulator.info/examples/6.3)

系統：Framework WebForm

<!-- more -->

## 文前言

這篇是基於前篇 [Bootstrap-table](Bootstrap-table.md) 欄位行內編輯失敗的產物。

需求：  
1. 欄位行內編輯：勾選、填值 (要符合規則驗證)
2. 下載 csv 且中文無亂碼
3. 基礎美觀

話說它好像也有展開細項的寫法？

其實原本在看以前收集的一些 datatable，有翻到  
[DataTables實用code合籍](https://yoziming.github.io/post/220825-datatables/#datatables%e5%b0%8f%e6%8a%80%e5%b7%a7)  
這篇。但後來發現編輯功能要收錢，只好放棄...

[jqGrid 紀錄](jqGrid%20紀錄.md) 擔心使用者覺得不夠美觀 (跟原先的 bootstrap-table 差太多了)

## 主文

為了配合整隻系統，所以美觀採用 Bootstrap 5 css 以保持一致。

初始化使用方式請參照官網：[Quickstart Guide | Tabulator](https://tabulator.info/docs/6.3/quickstart)  
另外主題個人使用了：[Bootstrap 5 Theme](https://tabulator.info/docs/6.3/theme#framework-boot5)

借 ChatGPT 問了範例資料來做展示：

![](../../../../assets/images/Tabulator%20+%20Bootstrap%205%20css_EXAMPLE.png)

### Column

```js
// 全域設定欄位的對齊方式
columnDefaults: {
    hozAlign: "center",        // 設定所有欄位的水平對齊方式
    headerHozAlign: "center",  // 設定所有欄位標題的水平對齊方式
    vertAlign: "middle"
},
```

全域寫的好處是如果欄位很多，就不用每個都複製貼上同樣的東西。
### Edit

ref: [Editing Data | Tabulator](https://tabulator.info/docs/6.3/edit)  
沒有特殊需求的話可以直接照網頁內 [Overview](https://tabulator.info/docs/6.3/edit#overview) 展示的寫法 
#### checkbox

給的版本沒有像 bootstrap-table 的直覺，但堪用。  
有特殊需求看：[Checkbox](https://tabulator.info/docs/6.3/edit#editor-checkbox)

#### text value

有特殊需求看：[Input](https://tabulator.info/docs/6.3/edit#editor-input)

驗證器請見：[Valid](#Valid)

### Valid

驗證器：[Data Validation | Tabulator](https://tabulator.info/docs/6.3/validate)  

也可以設定 regex。  

例如 發票號碼的簡單驗證：

```js
{
    title: '發票號碼',
    field: 'xxx',
    editor: "input",
    validator: "regex:[A-Z]{2}[0-9]{8}$"
}
```

實際展示：

![](../../../../assets/images/Tabulator%20+%20Bootstrap%205%20css_valid.png)

上方欄位：通過驗證  
下方欄位：驗證沒過

說到這裡，不知為何 Bootstrap 5 版的 css 在最右欄的右側邊框會失蹤，但預設版就不會？  
其他主題沒測試過。


### Pagination

ref: [Pagination | Tabulator](https://tabulator.info/docs/6.3/page)

分頁按鈕會自帶 tooltip 顯示。  

會有預設英文分頁，所以可以參考 [Local lang](#Local%20lang) 自訂。例如：

```js
langs: {
    "zh-tw": {
        "pagination": {
            "first": "第一頁",
            "first_title": "",
            "last": "最後一頁",
            "last_title": "",
            "prev": "上一頁",
            "prev_title": "",
            "next": "下一頁",
            "next_title": "",
        }
    }
},
```

結果：  

![](../../../../assets/images/Tabulator%20+%20Bootstrap%205%20css_pagina%20lang.png)


其他設定範例：

```js
pagination: "local",    //資料來自本機
paginationSize: 10,   //一頁最多顯示幾筆
paginationButtonCount: 10,  //分頁按鈕一次顯示幾個
```

### Local lang

ref: [Localization | Tabulator](https://tabulator.info/docs/6.3/localize)

自訂語言用的。