---
tags:
  - Codinggg/Lan/Angular
categories:
  - 雜記
date: 2025-01-14 10:58:19
updated: 2025-02-21 17:08:29
---
文前言：本篇因為在昏沉狀況下處理所以也沒整理多好，故給名【雜記】

網路上大部分的教學都是在 Angular 啟動時預設使用 http 的，所以基本不能參考...  
總之大概查了一下，有兩種方法 (或稱 1 種？) 可以讓 Json Server 預設用 https 啟動。

先說結論：最後放棄了，網址改用 http 處理。感覺是就差一點點...

## 1. OpenSSL

總之如果要用這個的話，可以先安裝 `choco`，再裝 `openssl`。  
處理好之後會得到兩個檔案。(好像就是所謂的 SSL 憑證？  
它

另外附上幾個我半嘗試掉的...？(可能有成功的路徑？)  
[Support for HTTPS · Issue #244 · typicode/json-server](https://github.com/typicode/json-server/issues/244)  
[Secure JSON-Server Setup on HTTPS](https://json-server.dev/json-server-https/)  
[How to run json-server under HTTPS? · Issue #389 · typicode/json-server](https://github.com/typicode/json-server/issues/389)

## 2. Hotel

先放上 [官網](https://github.com/typicode/hotel)  
特性之一是可以改用 https 接。  
期間嘗試了 1+2：  
`hotel start --ssl-cert AAA --ssl-key BBB`  
再做 `json-server --watch db.json --port xxx`

最後仍舊失敗。但總覺得差一點點而已...  
未來如果有成功再來附上後續處理吧qq



> Written with [StackEdit](https://stackedit.io/).