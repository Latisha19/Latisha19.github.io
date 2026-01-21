---
comments: true
categories:
  - Codinggg
tags:
  - Codinggg/Lan/Cs
  - Codinggg/WebForm
date: 2025-11-26 13:56:03
updated: 2026-01-21 16:59:46
---
## 文前言

來記一些自己的新發現... (零零碎碎)  
錯過好多，感謝基於好奇翻網路 & AI 的自己

<!-- more -->

## 主文

### `.aspx.cs` 內 `Page_Load` 方法呼叫非同步

- [在 ASP.NET 4.5 中使用非同步方法 | Microsoft Learn](https://learn.microsoft.com/zh-tw/aspnet/web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45#CreatingAsynchGizmos)


### `.ashx` 內使用 `.aspx.cs` 內才能用的 `Session["xxx"]` 寫法

- `context.Session["xxx"]`、類別繼承要加上 `IRequiresSessionState`  

(這樣甚至可以吃到之前設定好的 login 資料)


### `.ashx` 內 `ProcessRequest()` 改非同步寫法

原
```cs
public class Handler : IHttpHandler
{
    public void ProcessRequest(HttpContext context)
    {
        xxx();
    }

    public bool IsReusable
    {
        get { return false; }
    }
}
```

後
```cs
public class Handler : HttpTaskAsyncHandler
{
    public override async Task ProcessRequestAsync(HttpContext context)
    {
        await xxx();
    }
}
```


## UPDATE LOG

114.
11/26 開文