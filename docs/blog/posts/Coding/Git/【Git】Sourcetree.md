---
comments: true
tags:
  - Codinggg/Tools/git
categories:
  - Codinggg
  - Tools
date: 2025-01-14 10:57:52
updated: 2025-08-15 10:31:27
---
最近 Sourcetree 更新了 (忘記甚麼時候更得，總之我現在是 win 3.4.17)  
最明顯的功能大概就是 Stash 了吧...  
原本只有 **[Keep staged changed]** 選項，更新後多出 **[Untracked files]** 與 **[All]** 兩個選項。

<!-- more -->

目前實驗如下：(假設已有 Stage 檔、Unstage 檔、Untracked 檔

- **[Keep staged changed]**: 消失 unstage、untrack 保留；全部存入 stash
- **[Untracked files]**: 全部消失、全部存入 stash
- **[Keep staged changed]** + **[Untracked files]**： 消失 unstaged、全部存入 stash
- **[All]**: (好像會動到所有檔案，等哪天專案檔案不多再來嘗試

暫時先這樣。

> Written with [StackEdit](https://stackedit.io/)