---
tags:
  - Codinggg/Lan/js
categories:
  - ???
  - Codinggg
date: 2025-01-14 11:00:41
updated: 2025-03-27 13:48:45
---
今天遇到一個莫名的問題 (js)  
想讓 JQuery 選擇的某值做點調整  
開了 console 確認是一個空格，結果實際後端寫判斷 `== ' '` 或 `== " "` 都不行，忍不住在 console 直接打看看狀況，沒想到真的抓不到 = true 的狀態

<!-- more -->

最後直接複製貼上那個空格值才成功...  
我真ㄉ不知道為什麼 (抱頭

順便一說，開 Mergely 做比較，還真的有差...？？？？ (左邊是我打的，右邊是複製的)

貼在下方讓各位看官感受一下 my 崩潰

``` js
//手打
a = a + (b == ' ' ? "" : b) + ","
//複製貼上
a = a + (b == ' ' ? "" : b) + ","
```

以後還是多用複製吧...(結案)

> Written with [StackEdit](https://stackedit.io/).