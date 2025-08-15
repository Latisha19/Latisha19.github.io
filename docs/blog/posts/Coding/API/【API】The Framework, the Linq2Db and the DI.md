---
comments: true
categories: []
tags:
date: 2025-08-13 16:45:56
updated: 2025-08-15 10:30:39
---
## 文前言

標題是仿造 N 年前看過的小說：  
《**納尼亞傳奇：獅子·女巫·魔衣櫥**》（英語：_The Chronicles of Narnia: The Lion, the Witch and the Wardrobe_）  
不知道文法可不可，總之概念就是這篇是個超級客製化文(?

如標題所示，此篇是寫了 Framework API + Linq2Db + DI  
以及標題沒寫的 db 來源的 dll。

為了大量客製化，努力問了 GPT。  
(不是客戶需求，而是個人希望可以走最短路徑，也就是現有資源全部利用上。)

<!-- more -->

## 主文

### 首先該有的是

Linq2Db:

- [linq2db/linq2db: Linq to database provider.](https://github.com/linq2db/linq2db)
- [LINQ to DB | Linq To DB](https://linq2db.github.io/index.html)

可以配合 MySQL

dll: 裡面塞滿了 MySQL 相關 db。  
會這麼用是因為前陣子有隻做大用處且到處使用的程式被我抽成 dll 了，要不然每次更新大程式個地方就要更新一次滿痛苦的。

而有個問題是主程式跟大程式 dll 都有用到 db，因此出了點狀況

最後問了 GPT 結果決定將 DB 拆成 dll 引用。

(順帶一提，如果是兩隻都有用到相同的 model class 且參數重合，那把該 class 放在大程式 dll 就好。但 db 不能這樣玩，只能再拆一個 dll。)


今天的 api 要裝的套件：

1. Linq2Db
2. Swashbuckle (私心想用 swagger)
   - ref: [[WEB API] 使用 Swagger 自動產生 WebAPI 技術文件 ~ m@rcus 學習筆記](https://marcus116.blogspot.com/2019/01/how-to-add-api-document-using-swagger-in-webapi.html)
	   - `c.IncludeXmlComments(GetXmlCommentsPath());`
		   - => xml 文件轉化為介面文字
	   - `c.DocExpansion(DocExpansion.List);`
		   - => 預設展開所有節點 api
3. Autofac、.WebApi2 (以做 DI
	- GPT 介紹的


### 接下來

首先請在 config 檔內放上連線字串。(假設是放在名稱 DefaultConnection 內)

接下來：  
(假設專案名稱是 XXApi、dll 內 db 名稱是 OODB)

找到 XXApi.cs (應該會在 App_Start/ 下)  
在 `// Web API 路由` 這行上方加入：

```cs
// 1. Autofac 建立 ContainerBuilder  
var builder = new ContainerBuilder();  

// 註冊 Web API Controllers  
builder.RegisterApiControllers(Assembly.GetExecutingAssembly());  
  
builder.RegisterType<OODB>()  
		.AsSelf()       
		.InstancePerRequest();  
  
// 建立容器
var container = builder.Build();  
config.DependencyResolver = new AutofacWebApiDependencyResolver(container);
```

dll 內我的預設 db 建構子如下：  
(config 檔內該 db 連線字串名稱也是 DefaultConnection)

```cs
public OODB()
    : base("DefaultConnection")
{
}

public OODB(string configuration)
    : base(configuration)
{
}

public OODB(DataOptions options)
    : base(options)
{
}

public OODB(DataOptions<OODB> options)
    : base(options.Options)
{
}
```

因為建構子有多個，因此 DI 注入時出了點麻煩。  
但只要保證主程式與 dll 內放的連線字串名稱相同，就可以類似這樣注入了！  
(尚無需求研究若名稱不同怎麼辦...)


然後 Controller:

```cs
public class AAAController : ApiController
{
    private readonly OODB _ooDb;
    private static readonly ILogger Logger = LogManager.GetCurrentClassLogger();

    public AAAController(OODB ooDb)
    {
        _ooDb = ooDb;
    }
    ...
}
```

其中 log 是用 [NLOG](../log.md###NLog)，GPT 說不用做前面的 builder 那裏的注入配置。

### 結束啦！

研究結束。  
耗費半天...應該算少了？


## UPDATE LOG

114.

08/13 開新篇