---
date: 2025-01-14 11:00:08
updated: 2025-01-14 11:00:15
---
*** 緩慢更新中

沒想到居然會需要看起來算 n 年前的 jqGrid...
(看起來不支援已經有幾年了
總之先列一下我落落長的參考資料在註腳[^1] 。(太多了已經不確定有沒有漏掉的了QQ  新手努力參考)

嚴格來說我用的是 [Guriddo jqGrid JS](https://www.guriddo.net/documentation/guriddo/javascript/#system-requirements)

## 前置
至少要引用的：(路徑替換為自己的、`grid.locale-en.js` 可以換自己需要的)
```html
<link rel="stylesheet" type="text/css" href="jquery-ui.css"  />
<link rel="stylesheet" type="text/css" href="ui.jqgrid.css"  />

<script src="/1.7.1/jquery.min.js"  type="text/javascript"></script>
<script src="grid.locale-en.js"  type="text/javascript"></script>
<script src="jquery.jqGrid.min.js"  type="text/javascript"></script>
```

## 使用
我的簡易成果：(主要來自 ChatGPT，零散調整成我需要的長相)
![enter image description here](https://blogger.googleusercontent.com/img/a/AVvXsEjGvfk_Jp4hhz9DQLTMaoR_gP1mgLzG1dkFKpvj4c-OqU1p90otrPP5_t2xnms2sacOnfAkztOZ-N72YIAvXQqp1T2hmFgvQNI4-rjC1HcrOkuI23dG9_yXejyazKnEsMchP5YXnL68HcdrjHYwDVWgzm79B_xz9xIP2y9jza8ymQaz6nRh-11LxalfP-s=w640-h391)

```html=
<div>
    <table id="grid"></table>
    <div id="pager"></div>
</div>
<script>
    $(function() {
        var mydata = [
			{ id: 1, name: "John", age: 30, city: "" },
			{ id: 2, name: "Jane", age: 25, city: "" },
			{ id: 3, name: "Doe", age: 40, city: "" }
		];

        // 設置jqGrid
        $("#grid").jqGrid({
            datatype: "local",
            data: mydata,
            colModel: [
	            { name: "id", label: "ID", key: true, width: 50 },
				{ name: "name", label: "Name", width: 100, editable: true },
				{ name: "age", label: "Age", width: 50, editable: true },
				{ name: "city", label: "City", width: 100, editable: true }
			],
            pager: "#pager",
            rowNum: 10,
            rowList: [10, 20, 30],
            sortname: "id",
            sortorder: "asc",
            viewrecords: true,
            gridview: true,
            autoencode: true,
            caption: "Basic jqGrid Example"

        }).jqGrid("navGrid", "#pager", 
        { edit: true, add: true, del: true },
        {
            size: "100%",
            closeAfterEdit: true,
            recreateForm: true,
            url: '...',
            afterSubmit: function(response, postdata) {
                ...
            }
        });
    });

    $("#grid").trigger("refresh_grid", [{ page: 1 }]);
</script>
```
其中可以設點屬性
- `height`、`width` 設定整個 grid 的寬高
- 第二個 `.jqGrid()` 的第三個屬性 {} 是決定 toolbar 要不要有新增刪除編輯的小圖示
- 如果開編輯的話，`url` 要設值否則會一直報 "Not Set a Url" 的問題。要設你想把編輯結果塞給哪個檔案 (例如某 .php 檔之類的)。若是純本地不傳值給後端，值要設 'clientArray'。
- `multiselect` : 各 row 前是否要有複選框
- 若要對 colModel 中的誰做客製化 grid 外表時，使用屬性 `formatter`。
	- 例如：想讓值如果是空白，預設顯示 x :
		```js=
		formatter: function (cellValue, options, rowObject) {
		    return cellValue === "" ? "x" : cellValue;
		}
		```

### 表單編輯
#### 編輯：使用 checkbox 多值
來自 [這個(grid 內顯示用)](https://stackoverflow.com/a/21046993) 跟 [這個 (JS 內撰寫)](https://stackoverflow.com/a/27321972)

結果：
![enter image description here](https://blogger.googleusercontent.com/img/a/AVvXsEh-T5aNPgQTRilKdyvUIXCRV6ORtnZonx5U02Bl73dl19ndMTrQv5_XXAP9HQf-pteFljrZFjBjaSD8b6RLFTunVsK_3_Gyeq8TAZZcbIByl9lCktWcz6x_uNKQ5MQPio8aOH29odah83US6SmgfK1284MjYiJiaNQ4Yk3T_cArKAyMX0RvCoo6BsGE4ow=w649-h324)

若要進階一點，設定表單編輯內欄位名稱自製，可以使用 `beforeShowForm` 的參數來調整。
![enter image description here](https://blogger.googleusercontent.com/img/a/AVvXsEj2o-H5AU4Bq0la100tjwYXPgP4cGWOWiJ6qAprFp-XEYxbzyJ_D3koXRbj1ajEPJ9hblu_0rrmpXkfT_u5qBShpJi5DwGM8Kbmgks5FRvoQa_k1-7VXeOLbGg9cpkaft2saDLBc1slxun4e_9jjeab2pnk407zXbFKyl3Ow1KCleUzUFpZFZjP8m1-Bak=w500-h398)

[^1]: [JqGrid—功能強大的jQuery Grid Control](https://www.cc.ntu.edu.tw/chinese/epaper/0021/20120620_2109.html)
[JqGrid How to change width of edit form? - Stack Overflow](https://stackoverflow.com/questions/3901306/jqgrid-how-to-change-width-of-edit-form)
[Does jQuery jqGrid edit form support fields that are multiselect? - Stack Overflow](https://stackoverflow.com/questions/4984886/does-jquery-jqgrid-edit-form-support-fields-that-are-multiselect)
[wiki:common_rules - jqGrid Wiki](http://www.trirand.com/jqgridwiki/doku.php?id=wiki:common_rules)
ChatGPT
[softwarenotes/javascript/jqgrid - 設計筆記.md at master · mrtony/softwarenotes](https://github.com/mrtony/softwarenotes/blob/master/javascript/jqgrid%20-%20%E8%A8%AD%E8%A8%88%E7%AD%86%E8%A8%98.md)
[jqGrid 實用技巧 (十一) inline edit 及 confirm dialog before save | Mister Ngan](https://www.misterngan.com/1130/jqgrid-%E5%AF%A6%E7%94%A8%E6%8A%80%E5%B7%A7-%E5%8D%81%E4%B8%80-inline-edit-%E5%8F%8A-confirm-dialog-before-save/)
[jqGrid Demos](http://www.trirand.com/blog/jqgrid/jqgrid.html)
[JqGrid 教學(基礎配置) @ 討厭鬼教學 :: 痞客邦 ::](https://nerdyworld.pixnet.net/blog/post/24966733)
[提姆備忘錄 Javascript  jQuery Grid Control - jqGrid 簡介](https://d8890007.blogspot.com/2012/11/javascript-jquery-grid-control-jqgrid.html)
[第4個jqGrid範例: 資料列處理 – 簡睿隨筆](https://jdev.tw/blog/1640/jqgrid-data-manipulation)
[jQuery學習筆記&#8211;jqGrid的使用方法(編輯,刪除,更新,新增)](https://topic.alibabacloud.com/tc/a/jquery-learning-notes-how-to-use-jqgrid-edit-delete-update-and-add_8_8_31964283.html)

> Written with [StackEdit](https://stackedit.io/).