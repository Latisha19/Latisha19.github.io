---
categories:
  - log
date: 2025-01-08 03:15:43
updated: 2025-01-20 04:18:18
---
## NLog

- .NET Framework:
   1. nuget 裝 `NLog`、`NLog.Schema`
   2. 複製[raw.githubusercontent.com/NLog/NLog/v4.4/src/NuGet/NLog.Config/content/NLog.config](https://raw.githubusercontent.com/NLog/NLog/v4.4/src/NuGet/NLog.Config/content/NLog.config) 內文字，在跟 `NLog.xsd` 檔案同層位置新增檔案 `NLog.config`，貼上網址內東東後修改成自己想要的
   3. `public static NLog.Logger logger = NLog.LogManager.GetCurrentClassLogger();`