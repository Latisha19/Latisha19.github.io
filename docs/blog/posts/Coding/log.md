---
tags:
  - Codinggg/Tools/log
categories:
  - Codinggg
  - log
date: 2025-01-08 03:15:43
updated: 2025-04-09 16:52:12
---
## NLog

<!-- more -->

ref: [NLog 教學：快速建立自訂日誌紀錄(基礎篇). 嗨嗨，各位好，今天要介紹的是 NLog 這個 logging… | by Tom | appxtech | Medium](https://medium.com/appxtech/nlog-%E6%95%99%E5%AD%B8-%E5%BF%AB%E9%80%9F%E5%BB%BA%E7%AB%8B%E8%87%AA%E8%A8%82%E6%97%A5%E8%AA%8C%E7%B4%80%E9%8C%84-%E5%9F%BA%E7%A4%8E%E7%AF%87-8b4f27739f30)  
(core 版)

- .NET Framework:
   1. nuget 裝 `NLog`、`NLog.Schema`
   2. 複製 [raw.githubusercontent.com/NLog/NLog/v4.4/src/NuGet/NLog.Config/content/NLog.config](https://raw.githubusercontent.com/NLog/NLog/v4.4/src/NuGet/NLog.Config/content/NLog.config) 內文字，在跟 `NLog.xsd` 檔案同層位置新增檔案 `NLog.config`，貼上網址內東東後修改成自己想要的
   3. `NLog.config` 的屬性 設定：`複製到輸出目錄 (Copy to Output Directory)` 為 `有更新才複製 (Copy if newer)`
   4. `public static NLog.Logger logger = NLog.LogManager.GetCurrentClassLogger();`