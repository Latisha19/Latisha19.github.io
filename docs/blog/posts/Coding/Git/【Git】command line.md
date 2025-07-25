---
categories: []
tags:
date: 2025-05-08 14:22:45
updated: 2025-07-16 13:34:28
---

## 主文

### tag

列出某 TAG 釋出後至今的所有 commit 詳細紀錄：

```bash
git log TAG..HEAD
```

追加 只列出 commit 訊息標頭：

```bash
git log TAG..HEAD --pretty=format:"%s"
```

若想順序反過來列 (亦即舊到新)：  
命令尾加上 `--reverse`

### 救援誤刪的 stash

ref: [recovery - How do I recover a dropped stash in Git? - Stack Overflow](https://stackoverflow.com/questions/89332/how-do-i-recover-a-dropped-stash-in-git)

```bash
git fsck --unreachable | grep commit | cut -d" " -f3 | xargs git log --merges --no-walk --grep=xxx
```

xxx 是你誤刪 stash 的名稱

結果列出會類似：

```bash
Checking object directories: 100% (256/256), done.
Checking objects: 100% (3348/3348), done.
Verifying commits in commit graph: 100% (590/590), done.
commit AAA
Merge: a b
Author: ...
Date:   ...

    On feat/...: xxx
```

列出可能有多筆或一筆  
確認你需要的那筆的 commit hash，例如「AAA」  
然後：

```bash
git stash apply AAA
```

這樣就回到你的 unstage 區啦。


真可怕啊...改了一堆東西，要是救不回來...


## UPDATE LOG

114.

05/08 開文 (TAG  
07/10 追加 救援誤刪的 stash