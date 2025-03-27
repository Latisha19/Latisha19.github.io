---
categories:
  - IIS
  - Codinggg
tags:
  - Codinggg/IIS
date: 2025-02-06 09:39:29
updated: 2025-03-27 15:32:10
---
參考是找了很多，但到最後依舊無法成功利用虛擬目錄接網芳。

於是利用一點 trick：

帶參考 [如何清除連線共用資源帳戶的資訊 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/questions/10007065)  
<!-- more -->

程式碼內直接呼叫 `cmd` 利用 `net use` 指令做連接 & 斷連  
不管是本機還是 iis 都很成功，但斷開連接倒是一直怪異的爆「找不到網路連線」  
這個就繼續尋找方法了。

最後放點正統寫法：

1. 有自帶帳密者，記得確認權限至少要有讀取
2. iis 網站下新增虛擬目錄
	1. 設定別名：自訂就好 (例如：123)
	2. 設定實體路徑：輸入網芳路徑 `\\ip\資料夾名稱`
	3. 點選 連線身分，若有帳密則選擇「特定使用者」，帳號有 domain 的記得要加 (domain\帳號)
	4. 測試連線結果
3. ~~在 iis 網站對應的 應用程式集區 > [進階設定] > [處理序模型] >~~
	1. ~~[載入使用者設定檔] 設為 true~~
	2. ~~[識別] 同 1.-3. 設定自己的帳密~~
4. 在 iis 管理員 內自己網站點選展開 1. 新增的虛擬目錄觀察是否可見  
	 ![](../../../../assets/images/IIS%20虛擬目錄.jpg)
5. 瀏覽器開啟網站根目錄，接上 "/(別名)" 嘗試連接。

以及期間參考的：  
* [How to create a virtual directory on an existing Web site to a folder that resides on a remote computer - Windows Server | Microsoft Learn](https://learn.microsoft.com/en-us/troubleshoot/windows-server/networking/create-virtual-directory-folder-remote-computer)  
* [IIS 如何建立網路芳鄰之虛擬目錄 - 噗落格的資訊空間-網路技術分享-Asp.Net.VB.C#.程式開發.微網誌.網路行銷.facebook 行銷.噗浪行銷.社群行銷](https://king971119.blogspot.com/2010/07/iis.html)  
* [[ASP.NET] IIS、Web Page 網頁 存取連線網路磁碟機－伊のspace～芳香精油＊美容保養＊程式設計｜痞客邦](https://takamai.pixnet.net/blog/post/42794896)  
* [Max Coding BLOG. (^_^)y: IIS 7掛載網芳的虛擬目錄](https://maxtellyou.blogspot.com/2010/02/iis-7.html)  
* [ASP.NET 如何將檔案寫入到網路芳鄰的分享目錄 | The Will Will Web](https://blog.miniasp.com/post/2007/11/10/How-to-write-file-to-net-share-folder-using-ASPNET)

我是在 2.-d. 測試連線全掛 (明明帳密是對的？)  
而且 5. 發現根目錄爆
```
Service Unavailable 
HTTP Error 503. The service is unavailable.
```

也沒查到甚麼解決方法，決定放棄QQ

在此留一下我寫的連線與切斷連線網芳寫法：(C#、WebForm)  
(xxx 請替換成自己的 \\ip\folder、ooo 是帳號 domain\acount)

```cs
#region 網芳  
  
/// <summary>  
/// 網芳連線  
/// </summary>  
public void ConnNetworkDrive()  
{  
	try  
	{  
		Process process = new Process  
		{  
			StartInfo = new ProcessStartInfo  
			{  
				FileName = "cmd.exe",   
				Arguments = $@"/c net use xxx /user:ooo pwd",  
				RedirectStandardOutput = true,  
				RedirectStandardError = true,  
				UseShellExecute = false,  
				CreateNoWindow = true,  
				StandardOutputEncoding = Encoding.Default,  // 設定輸出為 UTF-8
				// 設定錯誤輸出為 UTF-8  
				StandardErrorEncoding = Encoding.Default    
			}  
		};  

		process.Start();  
		string output = process.StandardOutput.ReadToEnd();  
		byte[] errorBytes = 
			process.StandardError.CurrentEncoding.GetBytes(
				process.StandardError.ReadToEnd()
			);  
		process.WaitForExit();  

		//結果轉 utf8  
		string error = 
			Encoding.UTF8.GetString(
				Encoding.Convert(
					Encoding.GetEncoding("big5"), Encoding.UTF8, errorBytes
				)
			);  

		if (!String.IsNullOrEmpty(error))  
			SiteMaster.logger.Error(@"xxx 網芳執行 net use 失敗！" + error);  
		else  
			SiteMaster.logger.Info(@"xxx 網芳執行 net use 結果：" + output);  

		// 測試存取 NAS 內的檔案  
		if (Directory.Exists(...))  
			SiteMaster.logger.Info("NAS 連線成功，可存取檔案。");  
		else  
			SiteMaster.logger.Error("NAS 連線失敗或檔案不存在！");  
	}  
	catch (Exception ex)  
	{  
		SiteMaster.logger.Error(@"xxx 網芳執行 net use 失敗！錯誤訊息：" + ex);  
	}  
}  

/// <summary>  
/// 網芳解連線  
/// </summary>  
public void DisconnNetworkDrive()  
{  
	try  
	{  
		Process process = new Process  
		{  
			StartInfo = new ProcessStartInfo  
			{  
				FileName = "cmd.exe",  
				Arguments = $@"/c net use xxx /delete",  
				RedirectStandardOutput = true,  
				RedirectStandardError = true,  
				UseShellExecute = false,  
				CreateNoWindow = true,  
				StandardOutputEncoding = Encoding.Default,  // 設定輸出為 UTF-8  
				// 設定錯誤輸出為 UTF-8  
				StandardErrorEncoding = Encoding.Default    
			}  
		};  

		process.Start();  
		string output = process.StandardOutput.ReadToEnd();  
		byte[] errorBytes = 
			process.StandardError.CurrentEncoding.GetBytes(
				process.StandardError.ReadToEnd()
			);  
		process.WaitForExit();  

		//結果轉 utf8  
		string error = 
			Encoding.UTF8.GetString(
				Encoding.Convert(
					Encoding.GetEncoding("big5"), Encoding.UTF8, errorBytes
				)
			);  

		if (!String.IsNullOrEmpty(error))  
			SiteMaster.logger.Error(@"xxx 網芳刪除 net use 失敗！" + error);  
		else  
			SiteMaster.logger.Info(@"xxx 網芳刪除 net use 結果：" + output);  
	}  
	catch (Exception ex)  
	{  
		SiteMaster.logger.Error(@"xxx 網芳刪除 net use 失敗！錯誤訊息：" + ex);  
	}  
}  

#endregion
```

---

*** 113.02.10 下午更：

程式碼版刪除找到解法，參考：  
[[ Windows Command ]Use the NET USE /DELETE command to remove a mapped drive](https://gist.github.com/elvischen/edf16d8286e702771c439ab4e06c0e14)

改用磁碟機代號刪除，這次就有成功。



*** 下午二更：

虛擬目錄解法成功！  
前提是請在 IIS 機器內加上同 NAS 使用者帳密 (WIN11 加帳密的那種喔)  

這樣再試  
1. 虛擬目錄就可以過了，虛擬目錄連的帳號不用加上 domain (以個人例子而言)  
2. 應用程式集區可以切回原先預設，不用動到 (個人不太了解它，所以能不動就不動最好)  
3. 2.-d. 測試連線全過，好耶

但奇怪的事還是很多... (待續)  
[存取被拒絕，因此無法開始監視 \\x.x.x.x\XXX 的變更 (IIS7) | The Will Will Web](https://blog.miniasp.com/post/2009/05/20/Failed-to-start-monitoring-directory-changes-when-using-UNC-virtual-directory-IIS7)

目前暫時改用 FTP 處理。  
[【FTP】](../【FTP】.md)