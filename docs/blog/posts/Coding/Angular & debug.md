---
tags:
  - Codinggg/Lan/Angular
  - Codinggg/debug
categories:
  - Codinggg
  - debug
date: 2025-01-14 10:58:19
updated: 2025-03-31 17:03:25
---
身為一個最近越來越常在搞 debug 的人而言 (why 呢，我也不曉得...)，使用 angular 是一定要 debug 的！

<!-- more -->

然而常常因為專案的 npm 版本太舊，使用 WebStorm 時無法 debug...

> ref: [debugging - Unable to debug Angular with WebStorm - Stack  
> Overflow](https://stackoverflow.com/questions/58630797/unable-to-debug-angular-with-webstorm)  
> 裡面提到當 Node.js 版本 <= 16 時無法 debug

看到這個訊息後悲傷的改找 chrome 開 f12 慢慢 debug...

直到最近某天因為 WebStorm 太大隻了改開 vscode，意外發現 debug 的新方法！  
以下就來點快樂 debug 吧：
1. 使用 vscode 開啟專案
2. 開啟專案內的 package.json 檔案
3. ![](../../../assets/images/Angular%20&%20debug_debug%20btn.png)  
4. 有看到這顆小小的 Debug 按鈕嗎？給他按下去！
5. vscode 會自動開啟終端機開跑 debug！
6. 快樂 debug 人生

...雖說如此，但是 debug 好累啊...  
但總算有好辦法處理了…




> Written with [StackEdit](https://stackedit.io/).
