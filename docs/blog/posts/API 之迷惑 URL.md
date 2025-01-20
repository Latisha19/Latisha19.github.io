---
categories:
  - API
date: 2025-01-14 10:59:30
updated: 2025-01-20 04:20:06
---
~~文前：這個問題應當不是 Postman 本身的錯誤，是我也不記得哪來的錯誤...~~

這個問題困擾我昨天晚上到剛剛，沒想到這麼扯...  
總之昨天用 Postman 測 API 時失敗，爆
```markdown
Could not send request

Error: Invalid character in header content ["Host"]
```
仔細觀察一下我的 Host...預設的應該沒關係吧？？
中間跑去參考一下跟我的案例類似的 [Invalid character in header content [“Host”] Postman - 🙋 Help - Postman Community](https://community.postman.com/t/invalid-character-in-header-content-host-postman/25225)，看起來也沒什麼用處...

最後改成用能測成功 api 的 SwaggerUI 內 url 複製貼上去 Postman 的 url 那裏再測看看，咦，過了？
非常莫名的我決定把兩段 url 丟去 Mergely 比較看看...咦，怎麼多出了很多空格？

附上範例 url，可以貼在記事本上用左右鍵試試看，會發現 xxx 跟 /、i 跟 / 中間各多了隱藏空格QQ
`http://localhost:xxx​/api​/ooo`

依舊破案的莫名其妙記...

## 4/25 補記：

剛剛被建議丟去 ChatGPT 問，結果得到了...

```
You
為甚麼這段字串 http://localhost:xxx​/api​/ooo 會出現隱藏空格？

ChatGPT
這段字串中看起來像是有一個隱藏的空格，但實際上可能不是空格字符。這可能是因為你複製了這段文字從某個文本編輯器、網頁或其他來源，而該來源在呈現文字時添加了不可見的字符。

要確定這個問題的根源，你可以使用一個十六進位編輯器或其他工具來檢查該字串中的每個字符，以確定是否存在額外的不可見字符。這樣你就可以更好地了解該字串中的特殊字符來源，並做出相應的處理。
```

ummm...

閒著無聊開了 f12，意外發現

？！那4什麼？！
wiki 表示：[零寬空格 - 維基百科，自由的百科全書](https://zh.wikipedia.org/zh-tw/%E9%9B%B6%E5%AF%AC%E7%A9%BA%E6%A0%BC)
嗯嗯...好...

另外終於抓到罪魁禍首了，原來我當初 url 是從 SwaggerUI 那直接複製它開頭 url 的結果...怪了，以前複製那麼多次怎麼只有這次出問題...

破案確認✌️
順帶因為牽涉的不只 Postman 所以改了標題。

> Written with [StackEdit](https://stackedit.io/).