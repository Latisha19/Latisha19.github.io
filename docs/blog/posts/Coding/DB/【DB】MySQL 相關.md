---
comments: true
categories:
  - Codinggg
  - db
tags:
  - Codinggg/DB
date: 2025-04-07 17:52:18
updated: 2026-01-21 17:06:46
---
## 文前言

記一些需要記得的東西，以及搬遷部分 HackMD 來集中放置

<!-- more -->

## 主文

- [Customizing Keyboard Shortcuts in MySql Workbench - Stack Overflow](https://stackoverflow.com/questions/12043478/customizing-keyboard-shortcuts-in-mysql-workbench)

### SQL 欄位

#### FK

`ON DELETE RESTRICT` 和 `ON UPDATE RESTRICT`: 被參考欄位 更新/刪除 時需要把參考欄位加上一起更新/刪除


`ON DELETE CASCADE` 和 `ON UPDATE CASCADE`: 被參考欄位 更新/刪除 時，參考欄位會自動一起更新/刪除

#### DROP vs TRUNCATE

DROP 刪除表。

TRUNCATE 清空但保留其結構


## UPDATE LOG

114.
04/07 開文  
11/27 調整版面結構、加入 [SQL 欄位](#SQL%20欄位)