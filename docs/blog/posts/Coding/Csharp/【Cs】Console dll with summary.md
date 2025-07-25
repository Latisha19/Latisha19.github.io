---
categories: []
tags:
date: 2025-06-26 09:27:40
updated: 2025-06-26 09:31:22
---
有兩隻專案

Console: 打包成 dll  
A: 參考 dll

可 ref:  
[c# - How to include documentation in DLL to show method summary in Unity3D? - Stack Overflow](https://stackoverflow.com/questions/32994598/how-to-include-documentation-in-dll-to-show-method-summary-in-unity3d)

但若是直接使用 VS2022 (其他尚未嘗試過)  
來做參考的話  
是不用把 xml 檔複製過去的  
只要重新參考 dll 就可以了


總之如果已經參考過 dll  
記得砍掉原 dll 參考，重新參考一次