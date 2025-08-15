---
comments: true
categories:
  - 雜記
  - Tools
tags:
  - Tools
  - Docker
date: 2025-05-29 14:40:01
updated: 2025-08-15 10:31:59
---
## 文前言

雖說是雜記，但這其實是篇正經八百的搞 docker...  
但得老實說，我跟 docker 熟悉的距離大概有一個馬里亞納海溝 (???

所以事實上架起來全靠網路文章 & 資深拯救。  
於此無聊記。

ref: [玩具烏托邦: 架設 bugzilla， 懶人版](https://newtoypia.blogspot.com/2020/05/bugzilla.html)

## 主文

### 至少得先理解一點？？

- [GitHub - bugzilla/bugzilla: Official repository for the Bugzilla bug tracking system. Report bugs to https://bugzilla.mozilla.org/enter_bug.cgi?product=Bugzilla&format=__default__ . Main website:](https://github.com/bugzilla/bugzilla)


### 架

參考該網站使用 docker 的選項。

一點注意給跟我一樣完全不理解的人：

- 在 Bugzilla Docker 容器中建立的 MySQL 使用者與資料庫，其實是容器內部的 MySQL，不是本機上的 MySQL。所以在自己的 MySQL Workbench 上找不到新增的使用者& db 是正常的。

然後發現一點奇怪的問題：  
在輸到 `perl checksetup.pl` 這段執行後會得到請輸入 email 跟密碼的部分  
我的 email 是 xxx@gmail.com，哪知道會報：

```
The e-mail address you entered (lynx http://localhost/bugzilla/xxx@gmail.com) didn't pass our syntax checking for a legal email address. A legal address must contain exactly one '@', and at least one '.' after the @. It also must not contain any illegal characters.
```

...為何...

無奈重打一次，過了。

接下來是密碼。  
反覆說密碼長度至少 6 字元，但確實輸入超過 6 字元了...  
在這裡失敗了三次才過去。(用一模一樣的密碼)

成功架起來的成果：

![](assets/images/雜記自架%20BugZilla_bugzilla.png)



### 再繼續

起因是登入時帳號打 EMAIL 進不去，懷疑設定有問題，所以諮詢了資深。  
將整個 container 砍掉重來。

(接下來是有點不太一樣的輸入方式)

`GRANT ALL PRIVILEGES ON bugs.* TO 'bugs'@'localhost' WITH GRANT OPTION;`  
輸入完後請接著繼續：  
`flush privileges;`

接下來才是跳離 (Ctrl + D)

輸入 `find / -iname 'checksetup.pl' > ~/where.txt`

結果發現一串 Permission denied

因此輸入 `sudo su` 升格權限  
再重輸 `find / -iname 'checksetup.pl' > ~/where.txt`  
還是一串 Permission denied

因此：

```linux
# pwd
# ls
# cd /var/www/html/bugzilla/
# ls
```

接下來才往後輸 `perl checksetup.pl`

然後就來到輸入 email、使用者名稱、真實姓名、密碼  
(在這裡完全都是一次過的 qwq)

成功結束後進入一模一樣的畫面  
帳號 email 依舊進不去  
靈機一動改打使用者名稱，欸，過了...

### 寄信

架起來後，可能有甚麼原因導致需要讓它能夠自動寄信。  
例如：建立新帳號、密碼更改等等

請調設定：  
/var/www/html/bugzilla/data/params.json

此檔案內調整：

- mailfrom: 這裡我改為實際存在的信箱 (沒試過放假的)
- mail_delivery_method: 個人是用 smtp
- smtp_password: 實際存在的信箱密碼
- smtp_username: 實際存在的信箱 (同 mailfrom，沒試過不同)
- smtpserver: 伺服器嘛

寄信成功圖：  
![](/assets/images/雜記自架%20BugZilla_mail.png)



### api

雖然意外挖到了官方文件：  
[6. WebService API Reference — Bugzilla 5.2+ documentation](https://bugzilla.readthedocs.io/en/latest/api/index.html)

但實際操作時卻有點差異。是因為自架嗎？

直送 Insomnia:

url: http://localhost:8124/bugzilla/rest.cgi/...  (這裡用 rect 會失敗！)  
Auth: [Preferences] > [API Keys] 申請新 api，此處會用到寄信。所以使用前記得先開好

## update log

114  
05.29 開文章