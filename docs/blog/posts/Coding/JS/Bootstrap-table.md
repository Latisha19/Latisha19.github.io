---
tags:
  - Codinggg/Tools/Bootstrap
categories:
  - Codinggg
  - Tools
date: 2025-01-08 03:15:43
updated: 2025-03-27 11:40:38
---

官網：[Bootstrap Table · An extended table to the integration with some of the most widely used CSS frameworks. (Supports Bootstrap, Semantic UI, Bulma, Material Design, Foundation)](https://bootstrap-table.com/)

基於要把 WebForm 改版成 row 可點選展開的樣式，加上時間問題，因此選擇現成解法。  
當然因為這個解法目前個人寫起來容易有 XSS 問題，所以未來還要再改版一次...

目前最新版是 1.24.0  
之前的意外放了 CSS v1.23.5 + JS v1.15.5，後來察覺不對就調成一致用最新版了。

目前觀察到的版本升級差異：  
1. 舊版沒有 `expandRowByUniqueId` 跟 `collapseRowByUniqueId`，只有 `expandRow` 跟 `collapseRow`。  
2. 展開收合 row 的 icon 從 fontawesome 改成使用 bootstrap-icon，這問題耗了我一些時間，直到猜出問題所在...

基本製作參考了 [Bootstrap Table 響應式表格. 前言 | by 鯫生's Coding World | Medium](https://timchen0607.medium.com/bootstrap-table-%E9%9F%BF%E6%87%89%E5%BC%8F%E8%A1%A8%E6%A0%BC-9f6bb11fc5bc)


## 中間來談一下 bootstrap-icon 吧。

引用方式：原本先看了 bootstrap-table 的官網得知引用了 https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.css ，後來查了網路找到新說法，也驗證成功。  
ref: [Bootstrap框架的Icons使用教程- 66字體網](http://www.ziti66.com/net/html/68.html)  
這裡使用本機檔案引入的部分。  

1. 找到 bootstrap-icon 的 GitHub 官網  
2. 找到所需 release 版本  
3. 下載 source code  
4. 在要引用的專案內放入 `...\icons-(版本號)\font` 資料夾內的 css 與 fonts 資料夾  
5. 引用只要寫 css 就好



## detail-formatter, operateFormatter

### detail-formatter

ref: [Table Options · Bootstrap Table](https://bootstrap-table.com/docs/api/table-options/#detailformatter)

要求回傳字串，因此容易有 XSS 問題  
因為本人需求太過複雜，所以只好用字串塞  
接外部傳入參數請小心。

### operateFormatter

ref: [Column Options · Bootstrap Table](https://bootstrap-table.com/docs/api/column-options/#formatter)

為了放在操作欄位用的  
預期吃 `jQuery`、`String` 或 `HTMLElement`  
但有點奇怪的是使用 `HTMLElement` 沒成功？  
所以只好先以 string 處理  

保險作法：  
因為本人需求不大，放按鈕就好  
所以先用 `HTMLElement` 組好，再用 `.innerHtml` 回傳。

## 另外用法

checkbox:  
預設的是連 title 都是一個 checkbox  
希望有自訂 title 名稱可用 detail-formatter 處理。


## Column Options

ref: [Column Options · Bootstrap Table](https://bootstrap-table.com/docs/api/column-options/)

因我的資料是來自 cs，不曉得是否因此造成無法直接寫 `<th></th>` 來取得資料，一定要用 Column Options。  

- `title`: 標頭名稱、字串
- `field`: 對應 json 值名稱、字串
- `align`
- `valign`
- `sortable`: 是否啟用單欄排序、bool  

- `titleTooltip`: 標頭工具提示、字串
- `clickToSelect`
- `formatter`: 方法，配合前面的 operateFormatter 使用
- `escape`: 逃脫字元處理
- `visible`: 該欄是否顯示處理 (配合邏輯決定：可用單行 if-else)

- `editable`: 該欄位編輯、bool


### Editable

ref: [Editable with Bootstrap 5? · Issue #6034 · wenzhixin/bootstrap-table](https://github.com/wenzhixin/bootstrap-table/issues/6034)

使用 Bootstrap 5 + 需求 cell 編輯。  
試用成功，但不知為何更改值後按勾勾確定會爆掉...

```
Message= 
Source= 
StackTrace: 
at Object.values (<anonymous>) 
at un._isWithActiveTrigger (xxx/Scripts/bootstrap.bundle.min.js:6:68746) 
at xxx/Scripts/bootstrap.bundle.min.js:6:64589 
at m (xxx/Scripts/bootstrap.bundle.min.js:6:2010) 
at HTMLDivElement.a (xxx/Scripts/bootstrap.bundle.min.js:6:2383) 
at s (xxx/Scripts/bootstrap.bundle.min.js:6:636) 
at xxx/Scripts/bootstrap.bundle.min.js:6:2434
```

於是決定切去其他方式：  
[Tabulator + Bootstrap 5 css](Tabulator%20+%20Bootstrap%205%20css.md)