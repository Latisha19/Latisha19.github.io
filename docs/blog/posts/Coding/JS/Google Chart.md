---
categories:
  - Codinggg
  - Tools
tags:
  - Codinggg/Lan/js
  - Codinggg/Tools
date: 2025-03-31 15:07:52
updated: 2025-05-07 15:09:07
---
## 文前言...

原本沒有打算要寫一篇介紹它的，畢竟官方文件其實很清楚  
當初靠著官方文件 + 網路文章 就解決了直方圖版本。

官方文件：[Using Google Charts  |  Google for Developers](https://developers.google.com/chart/interactive/docs)  
網路文章：[Day18 iT邦鐵人賽 —來一個 Google Pie Chart 吧 | by Lai | Medium](https://medium.com/@lai0706/day18-it%E9%82%A6%E9%90%B5%E4%BA%BA%E8%B3%BD-%E4%BE%86%E4%B8%80%E5%80%8B-google-pie-chart-%E5%90%A7-bdca9690655c)

之所以為甚麼會參考這篇網路文章呢，是的，原本一開始是打算畫圓餅圖  
萬萬沒想到某區塊過大以至於不適合圓餅圖，太擠了...  
最近才因為區塊開始趨向平均分配，所以加個圓餅圖。  
也因此需要撿回當時的記憶，因為沒有記下來所以完全重找 QQ

因此記下。

<!-- more -->

## 主文

簡單介紹直方圖 & 圓餅圖 (目前也只碰過這兩種)  
範例 DEMO 就...以後再說 (?

先來點簡單外框。

1. 引入 https://www.gstatic.com/charts/loader.js 檔案
2. html 給個 div，只需要設定 id：`{html} <div id="chart"></div>`
3. js 撰寫 (範例來自 [Example /Visualization: Pie Chart  |  Charts  |  Google for Developers](https://developers.google.com/chart/interactive/docs/gallery/piechart#example) 簡化)
	```js
	// Load google charts
	google.charts.load('current', {'packages':['corechart']});
	
	// 當 Google Charts 讀取完成後執行 drawChart 函式
	google.charts.setOnLoadCallback(drawChart);
	
	function drawChart() {
	
		var data = google.visualization.arrayToDataTable([
					  ['Task', 'Hours per Day'],
					  ['Work',     11],
					  ['Eat',      2],
					  ['Commute',  2],
					  ['Watch TV', 2],
					  ['Sleep',    7]
					]);
		
		var options = {
		  title: 'My Daily Activities'
		};
		
		var chart = new google.visualization.XXX(
						document.getElementById('chart')
					);
		
		chart.draw(data, options);
	}
	```
4. 結束！

主要變動的是 JS 的部分。

- XXX => 看使用哪種圖表：圓餅圖 `PieChart`、直方圖 `ColumnChart`...
- `data`: array 的部分
	- 第一個是名稱 (用在圖例名稱、滑鼠移上的時候顯示)
	- 第二個是數量 (次數之類的)
	- 動態資料可採用一般 js 寫法：
	```js
	let dataTable = [
		['name', 'count'],
	];
	dataTable.push(...);
	
	// 初始化 Google Charts 的 DataTable
	let data = google.visualization.arrayToDataTable(dataTable);
	```
- `options`: 各種選項。比如設定圖的寬與高、圖例顯示與否及擺放位置、圓餅圖改 3D 版...

另外注意：`drawChart()` 函式只能出現一次，所以不管你要畫幾個圖都要放在同一個 `drawChart()` 裡面，不要像我一樣傻 QQ

### 共用 `options` (應該？)

- `title`: string、圖表顯示名稱
* `width`、`height`: int、圖表寬與高
* `legend`: 物件、圖例顯示位置
	* eg: `{js} legend: {position: 'top', textStyle: {color: 'blue', fontSize: 16}},`

ref: [Google Chart Background Color - Stack Overflow](https://stackoverflow.com/questions/8808100/google-chart-background-color)

* `backgroundColor`: 物件、設定圖表背景色 (預設白色)
	* eg: `{js} backgroundColor: { fill: 'snow' },`

### Column Chart 直方圖

應該是這個中文對吧 (?

ref: [Visualization: Column Chart  |  Charts  |  Google for Developers](https://developers.google.com/chart/interactive/docs/gallery/columnchart)

#### `options`

* `colors`: 設定各條指定顏色
	* eg: `{js} colors: ['#EA0000', '#3366CC', '#EA0000', '#3366CC', '#EA0000', '#3366CC']`

衍伸 條上顯示數值用：

```js
let dataTable = [
    ['name', 'count'],
];

...

// 條上顯示數值用
let view = new google.visualization.DataView(data);
view.setColumns([0, 1,
	{
	 calc: "stringify",
	 sourceColumn: 1,
	 type: "string",
	 role: "annotation"
	}, 2]);

let chart = new google.visualization.ColumnChart(document.getElementById('chart'));
chart.draw(view, options);
```


### Pie Chart 圓餅圖

ref: [Visualization: Pie Chart  |  Charts  |  Google for Developers](https://developers.google.com/chart/interactive/docs/gallery/piechart)

#### `options`

* `is3D`: bool、是否改用 3D 立體版