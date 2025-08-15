---
comments: true
categories:
  - Codinggg
tags:
  - Codinggg/Lan/Html
date: 2025-02-17 04:39:21
updated: 2025-08-15 10:31:30
---
雖然是個學了 html 就是必須的基礎觀念，但有時候還是容易忘記有哪些可以使用...

官網：[`<input>`: The HTML Input element - HTML: HyperText Markup Language | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#input_types)

<!-- more -->

| Type                                                                                             | 自我認知描述                           | 範例                                                                                                                                    |
| ------------------------------------------------------------------------------------------------ | -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| **常見**                                                                                           |                                  |                                                                                                                                       |
| [button](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/button)                 | 跟 `<button>` 很像，個人不太用            | ![](../../../../assets/images/Pasted%20image%2020250217165843.png)                                                                    |
| [email](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/email)                   | 電子郵件                             | ![](../../../../assets/images/Pasted%20image%2020250217170616.png)                                                                    |
| [file](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/file)                     | 檔案上傳用                            | ![](../../../../assets/images/Pasted%20image%2020250217170545.png)                                                                    |
| [hidden](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/hidden)                 | 隱藏欄位，但內部值在 form 會被一起提交           | play                                                                                                                                  |
| [image](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/image)                   | 照片用                              | play                                                                                                                                  |
| [number](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/number)                 | 純數字輸入用                           | ![](../../../../assets/images/Pasted%20image%2020250217170529.png)                                                                    |
| [password](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/password)             | 密碼欄，輸入值會自動被用黑點點隱藏                | ![](../../../../assets/images/Pasted%20image%2020250217170232.png)                                                                    |
| [search](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/search)                 | 搜尋                               | play                                                                                                                                  |
| [submit](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/submit)                 | form 提交用按鈕                       | ![](../../../../assets/images/Pasted%20image%2020250217170140.png)                                                                    |
| [tel](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/tel)                       | 電話，在手機版用處比較明顯                    | play                                                                                                                                  |
| [text](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/text)                     | 純文字                              | play                                                                                                                                  |
| **勾選框**                                                                                          |                                  |                                                                                                                                       |
| [checkbox](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/checkbox)             | 多選用勾選框                           | ![](../../../../assets/images/Pasted%20image%2020250217165859.png)![](../../../../assets/images/Pasted%20image%2020250217165941.png)  |
| [radio](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/radio)                   | 單選用勾選框                           | ![](../../../../assets/images/Pasted%20image%2020250217170310.png) ![](../../../../assets/images/Pasted%20image%2020250217170318.png) |
| **日期&時間**                                                                                        |                                  |                                                                                                                                       |
| [month](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/month)                   | 至今沒用過，月的輸入框，選擇器在 firefox 不支援...  | ![](../../../../assets/images/Pasted%20image%2020250217170451.png)                                                                    |
| [week](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/week)                     | 至今沒用過，周的輸入框，選擇器在 firefox 不支援...  | ![](../../../../assets/images/Pasted%20image%2020250217170213.png)                                                                    |
| [date](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/date)                     | 選日期用，會因瀏覽器提供不同內建選擇器              | ![](../../../../assets/images/Pasted%20image%2020250217170417.png)                                                                    |
| [time](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/time)                     | 時間輸入框，選擇器在 firefox 不支援...        | ![](../../../../assets/images/Pasted%20image%2020250217170155.png)                                                                    |
| [datetime-local](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/datetime-local) | 地區性日期時間輸入框，時間選擇器在 firefox 不支援... | ![](../../../../assets/images/Pasted%20image%2020250217170401.png)                                                                    |
| 已棄用：`datetime`                                                                                   | 至今沒用過                            | -                                                                                                                                     |
| **個人不常用**                                                                                        |                                  |                                                                                                                                       |
| [color](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/color)                   | 至今沒用過，好像是可以選顏色的顏色選擇器？            | ![](../../../../assets/images/Pasted%20image%2020250217170025.png)                                                                    |
| [range](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/range)                   | 至今沒用過，好像是可以選範圍？                  | ![](../../../../assets/images/Pasted%20image%2020250217170044.png)                                                                    |
| [reset](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/reset)                   | 至今沒用過，看官方說明是可以重設 form 內所有值但不推薦用  | ![](../../../../assets/images/Pasted%20image%2020250217170056.png)                                                                    |
| [url](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/url)                       | 至今沒用過，好像是可以輸網址？                  | ![](../../../../assets/images/Pasted%20image%2020250217170119.png)                                                                    |

