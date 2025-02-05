---
tags:
categories:
  - Python
  - MSSQL 
date: 2025-01-14 11:00:41
updated: 2025-02-05 03:59:05
---
從學寫程式開始最崩潰的前三名就是用程式連線資料庫，以及編碼...

今天拿 Python 連 MSSQL 第一次不意外地失敗了...
甚至不設帳密的連線也可以報個

```
"Login failed for user '\xe3\x83\xbeUWU\xe3\x82\x9e\\xxx'.DB-Lib error message 20018, severity 14:
General SQL Server error: Check messages from the SQL Server
DB-Lib error message 20002, severity 9:
Adaptive Server connection failed (127.0.0.1)
DB-Lib error message 20002, severity 9:
Adaptive Server connection failed (127.0.0.1)
")
```

(其中 xxx 是我的本機使用者名稱)

後來才發現到：
`Trusted_Connection` 是設給是否要用 Windows 身分驗證用的...
(參考自 [利用 Python 直接匯出資料庫 (MSSQL) 的資料 (Pandas ReadSQL) | YC's Weekly Journal](https://ycjhuo.gitlab.io/blogs/Python-Pandas-Download-Data-From-MSSQL.html#%E9%80%A3%E6%8E%A5-mssql-%E8%B3%87%E6%96%99%E5%BA%AB) )

其他的還有

```
('42000', '[42000] [Microsoft][ODBC SQL Server Driver][SQL Server]Cannot open database "AAA" requested by the login. The login failed. (4060) (SQLDriverConnect); [42000] [Microsoft][ODBC SQL Server Driver][SQL Server]Cannot open database "AAA" requested by the login. The login failed. (4060)')
```

(連線的資料庫名稱是 AAA)

....之類的

後來忽然想到，何不用 master 當資料庫連線看看？
這次沒有錯誤訊息了，改成不知道什麼東西的訊息：
`<pyodbc.Connection object at 0x00000261D55E36A0>`
嗯...這啥？

總之隱約覺得有成功，於是再加上 `SELECT 1` 配上印連線成功 or 失敗的結果
...！ 成功！

測試結果，迷惑的是這種系統資料庫都能連，當初自建的資料庫通通失敗...(劃掉的是自建的，其他都是系統的全部成功)
![enter image description here](https://blogger.googleusercontent.com/img/a/AVvXsEjEHqXuRsKGV3Efosv2prVc_NmF4oDA4AOsDDk7tNu97PcA_nHhOsm0Z1fpHWmfdh6NbVzzjVqeJHyVM9WTQT9pt4fi5Grtb-TuT6K5Gs0BoM1u0Gr49dbLIPvnvKniuEqcW-ks9uTbE1orn3q3hEpjSe-OIhaJ0IRl_u7TqT_7frSyPMDynYmns0khOyA)

請等待下一集...希望今天能搞懂...

> Written with [StackEdit](https://stackedit.io/).