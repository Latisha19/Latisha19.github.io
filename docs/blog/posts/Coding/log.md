---
tags:
  - Codinggg/Tools/log
categories:
  - Codinggg
  - log
date: 2025-01-08 03:15:43
updated: 2025-03-27 15:29:46
---
## NLog

<!-- more -->

- .NET Framework:
   1. nuget 裝 `NLog`、`NLog.Schema`
   2. 複製 [raw.githubusercontent.com/NLog/NLog/v4.4/src/NuGet/NLog.Config/content/NLog.config](https://raw.githubusercontent.com/NLog/NLog/v4.4/src/NuGet/NLog.Config/content/NLog.config) 內文字，在跟 `NLog.xsd` 檔案同層位置新增檔案 `NLog.config`，貼上網址內東東後修改成自己想要的
   3. `NLog.config` 的屬性 設定：`複製到輸出目錄 (Copy to Output Directory)` 為 `有更新才複製 (Copy if newer)`
   4. `public static NLog.Logger logger = NLog.LogManager.GetCurrentClassLogger();`