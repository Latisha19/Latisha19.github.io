---
tags:
  - Codinggg/Tools/git
categories:
  - 雜記
  - Tools
date: 2025-01-14 10:58:19
updated: 2025-02-21 17:08:25
---
一開始很常使用 Sourcetree，後來需求 SmartGit。  
其實這也沒甚麼，用久了就會發現它們彼此都有各自的好壞，一起開著更方便我自己。  
然而後來發生了一個問題：Bitbucket 有一個預設用 Sourcetree 開啟的連結 (網址大概是 `sourcetree://xxx` 這種)  
結果問題產生了：不管如何都會變成預設 SmartGit 開啟。

<!-- more -->

原本問題還不大，檢查一下發現預設應用程式開啟 `sourcetree://xxx` 是 SmartGit，改回去就好了。  
其實不然，你會發現預設開啟應用程式只有 SmartGit 跟去 MicrosoftStore 挖應用程式...

困擾了幾個月，今天終於找到解方。  
消息來源是 [sourcetree:// url scheme hijacked by SmartGit! / General / SmartGit](https://smartgit.userecho.com/communities/1/topics/1146-sourcetree-url-scheme-hijacked-by-smartgit)，官方表示於 20.1 版移除。  
那簡單，我改裝 20.1 嘛~

實測結果：`sourcetree://xxx` 網址成功開啟 Sourcetree✌️

> Written with [StackEdit](https://stackedit.io/).