---
comments: true
categories: []
tags:
date: 2025-06-30 14:13:15
updated: 2025-08-15 10:34:00
---
## 文前言

基於想寫出一個好一點的架構  
(但不確定正確喔...只問了 GPT)  
以及獨立於真資料的單元測試

<!-- more -->

## 主文

### 架構調整

表格來自 GPT 與個人狀況調整

| 階段       | 架構                                                                | 重點                                                          |
| -------- | ----------------------------------------------------------------- | ----------------------------------------------------------- |
| 未改動前     | .aspx > .aspx.cs > DAL                                            |                                                             |
| 部分改動     | .aspx > js > ashx > DAL                                           |                                                             |
| 要改       | ashx > Service > DAL                                              | 建立 Service layer 分層，避免 UI 呼叫 DAL。**ONLY 整合測試**              |
| **下一步**  | ashx > IService > Service > DAL                                   | 為 Service 加上 interface，準備 DI。                               |
| **再下一步** | ashx / Controller > IService > Service > IRepository > Repository | 完整導入 Repository pattern 與 DI Container。**可 isolation test** |
| **最終**   | Web API Controller > IService (透過 DI Container 注入)                | 將 ashx API 遷移至 Web API，結構現代化，易於前後端分離與身份驗證擴充 (JWT)。          |

#### ashx > Service > DAL

如何讓 ashx 可改 async:  
[asp.net - making asynchronous calls from generic handler (.ashx) - Stack Overflow](https://stackoverflow.com/questions/8299194/making-asynchronous-calls-from-generic-handler-ashx)
```cs
public class MyHandler : HttpTaskAsyncHandler 
{
    public override async Task ProcessRequestAsync(HttpContext context) 
    {
       await WhateverAsync(context);
    }
}
```

現階段有部分是改到這裡
已經有感架構改動的好處了



#### ashx > IService > Service > DAL

範例皆來自 ChatGPT

cs: (aspx.cs or ashx)

```cs
public void XXX()
{
	// 將 IService 注入 (日後可改用 DI Container)
	IUserService service = new UserService();

	var user = service.GetUser(1);

	...
}

// 或者多處方法使用到時，可以放在外面：
//private readonly IUserService _service = new UserService();
```

IService:

```cs
public interface IUserService
{
    User GetUser(int id);
}
```

Service:

> ChatGPT:  
> 如果 Service 方法只是直接呼叫 DAL 的 async 方法，可選擇不使用 await，直接 return Task（在沒有其他 await 邏輯狀況下），避免產生多餘 state machine。

```cs
public class UserService : IUserService
{
    public User GetUser(int id)
    {
        var dal = new UserDAL();
        return dal.GetUser(id);
    }
}
```

DAL:

```cs
public class UserDAL
{
    public User GetUser(int id)
    {
        // 資料庫查詢
    }
}
```


### 抽個模板吧

使用 User Control  
[[iT鐵人賽Day35]ASP.NET-使用者控制項-分頁的用法 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10225633)




## UPDATE LOG

114.

06/30 開文  
07/18 加上 User Control 段落