---
date: 2025-01-14 11:01:21
updated: 2025-01-14 11:01:25
---
昨天嘗試由 [【Windows排程】使用windows排程器讓python自己動起來吧!只要簡單四步驟 @ 恩哥Python量化教室-零基礎也能學會Python :: 痞客邦 ::](https://pixnashpython.pixnet.net/blog/post/41511724-%E3%80%90win10%E6%8E%92%E7%A8%8B%E3%80%91%E4%BD%BF%E7%94%A8windows%E6%8E%92%E7%A8%8B) 教學做點排程，哪知最後居然失敗了...

立刻執行後顯示 code 0x2
網路上說這代表找不到檔案...

研究半天才發現因為我填入要執行的檔名那裏，檔名有空格QQ
浪費我一個小時(哭

另外，如果在終端機跑 `where python` 發現有超過一個路徑，[程式或指令碼] 那裏不能只填 [Python]，要填你指定的其中一個路徑。

總之，結案！


> Written with [StackEdit](https://stackedit.io/).