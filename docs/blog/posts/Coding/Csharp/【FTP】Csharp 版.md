---
categories:
  - Codinggg
tags:
  - Codinggg/FTP
  - Codinggg/Lan/Cs
date: 2025-02-12 09:23:50
updated: 2025-05-12 14:41:48
---
## 文前言

意外發現原來 FTP 跟 SFTP 寫法是不同的...  
來擴充版面吧！

FTP: 使用 `FtpWebRequest`  
SFTP: 使用 [sshnet/SSH.NET: SSH.NET is a Secure Shell (SSH) library for .NET, optimized for parallelism.](https://github.com/sshnet/SSH.NET)

<!-- more -->

## 主文

### FTP

#### 基礎

##### 取得遠端某資料夾內所有檔案清單  

[Follow Fang!: C#.Net 透過FtpWebRequest取得FTP檔案清單](https://cyfangnotepad.blogspot.com/2017/02/cnet-ftpwebrequestftp.html)

##### 自遠端下載檔案

[［C#］從ftp server下載檔案＠[C#] 資料結構與影像處理｜PChome Online 個人新聞台](https://mypaper.pchome.com.tw/middlehuang/post/1321704699)

##### 循環從本機上傳資料夾內所有檔案到遠端

```CS
/// <summary>
/// 複製本機資料夾至遠端 (不含子資料夾)
/// </summary>
/// <param name="srcDir"></param>
/// <param name="ftpDir"></param>
/// <param name="usrname"></param>
/// <param name="pwd"></param>
private void CopyLocal2Ftp(string srcDir, string ftpDir, string usrname, string pwd)
{
	string[] files = Directory.GetFiles(srcDir, "*", SearchOption.AllDirectories);
	foreach (string file in files)
	{
		UploadFileToFTP(file, ftpDir, usrname, pwd);
	}
}

/// <summary>
/// 單檔上傳至 FTP 指定資料夾
/// </summary>
/// <param name="localFilePath"></param>
/// <param name="ftpFilePath">FTP 指定資料夾</param>
/// <param name="username"></param>
/// <param name="password"></param>
private void UploadFileToFTP(string localFilePath, string ftpFilePath, string username, string password)
{
    string uploadfile = ftpFilePath + Path.GetFileName(localFilePath);
    try
    {
        FtpWebRequest request = (FtpWebRequest)WebRequest.Create(uploadfile);
        request.Method = WebRequestMethods.Ftp.UploadFile;
        request.Credentials = new NetworkCredential(username, password);
        request.UsePassive = true;
        request.UseBinary = true;
        request.KeepAlive = false;

        byte[] fileContents = File.ReadAllBytes(localFilePath);
        request.ContentLength = fileContents.Length;

        using (Stream requestStream = request.GetRequestStream())
        {
            requestStream.Write(fileContents, 0, fileContents.Length);
        }

        using (FtpWebResponse response = (FtpWebResponse)request.GetResponse())
        {
            logger.Info($"檔案 {uploadfile} 上傳至 FTP 成功。({response.StatusDescription})");
        }
    }
    catch (Exception ex)
    {
        logger.Error($"檔案 {uploadfile} 上傳至 FTP 失敗！錯誤訊息：{ex}");
    }
}
```

#### 進階

##### 檢查資料夾是否存在

[c# - How to Determine if FTP Directory Exists - Stack Overflow](https://stackoverflow.com/questions/24761583/how-to-determine-if-ftp-directory-exists)

##### 於遠端建立新資料夾

[.net - How do I create a directory on FTP server using C#? - Stack Overflow](https://stackoverflow.com/questions/860638/how-do-i-create-a-directory-on-ftp-server-using-c)

##### 建立多層子資料夾

```CS
/// <summary>
/// 建立多層子資料夾
/// </summary>
/// <param name="ftpDirectory">以 "/" 分割，不用帶最後一條 "/"</param>
/// <param name="username"></param>
/// <param name="password"></param>
private void CreateFtpDirectories(string ftp, string ftpDirectory, string username, string password)
{
    try
    {
        string[] folders = ftpDirectory.Split('/');
        string currentPath = ftp;

        foreach (string folder in folders)
        {
            currentPath = $"{currentPath}/{folder}/";

            if (!sharedFunction.DirectoryExists(currentPath, username, password))
            {
                FtpWebRequest request = (FtpWebRequest)WebRequest.Create(currentPath);
                request.Method = WebRequestMethods.Ftp.MakeDirectory;
                request.Credentials = new NetworkCredential(username, password);
                request.UsePassive = true;
                request.UseBinary = true;
                request.KeepAlive = false;

                using (FtpWebResponse response = (FtpWebResponse)request.GetResponse()) { }
            }
        }
    }
    catch (WebException ex)
    {
        FtpWebResponse response = (FtpWebResponse)ex.Response;
        if (response != null && response.StatusCode == FtpStatusCode.ActionNotTakenFileUnavailable)
        {
            // 目錄已存在，忽略錯誤
        }
        else
        {
            logger.Error($"建立 {ftpDirectory} 資料夾失敗！WebException 錯誤訊息：{ex}");
        }
    }
    catch (Exception ex)
    {
        logger.Error($"建立 {ftpDirectory} 資料夾失敗！其他錯誤訊息：{ex}");
    }
}
```

##### 本機上傳整個資料夾到遠端

[recursion - C# Upload whole directory using FTP - Stack Overflow](https://stackoverflow.com/questions/13311975/c-sharp-upload-whole-directory-using-ftp)

##### 資料夾檔名更新

參考：
1. [c# - Rename directory on FTP server using FtpWebRequest - Stack Overflow](https://stackoverflow.com/questions/38846542/rename-directory-on-ftp-server-using-ftpwebrequest)
2. [c# - How to rename a file after upload - Stack Overflow](https://stackoverflow.com/questions/13026170/how-to-rename-a-file-after-upload)

```cs
string ftpDirectory = $"{ftpServer}/old";  
  
FtpWebRequest request = (FtpWebRequest)WebRequest.Create(ftpDirectory);  
request.Method = WebRequestMethods.Ftp.Rename;  
request.Credentials = new NetworkCredential(usrname, pwd);  
request.RenameTo = $"new";  

try  
{  
	// 發送請求並取得回應  
	using (FtpWebResponse response = (FtpWebResponse)request.GetResponse())  
	{  
		logger.Info(
			$"資料夾名稱已成功更改為：new；"+
			$"伺服器回應：{response.StatusDescription}"
		);
	}  
}  
catch (Exception ex)  
{  
	logger.Error($"檔案資料夾名稱改名失敗！錯誤訊息：{ex.ToString()}");  
}
```

##### 下載檔案後附檔寄信

ref: [asp.net - Attach file existing on FTP server to mail in C# .NET - Stack Overflow](https://stackoverflow.com/questions/50550945/attach-file-existing-on-ftp-server-to-mail-in-c-sharp-net)

### SFTP

#### 前置

ChatGPT 推薦安裝套件：  
[sshnet/SSH.NET: SSH.NET is a Secure Shell (SSH) library for .NET, optimized for parallelism.](https://github.com/sshnet/SSH.NET)

#### 使用

目前只用來撈指定資料夾的最新更新時間。  
然而 FTP & SFTP 似乎都不太好撈資料夾的時間，所以只能 loop 資料夾內所有檔案。  
belike 費波納契數列 那樣的 recursive。

(以下程式碼來自 ChatGPT、me 調整修修)
```cs
static void Main()
{
	string host = "192.168.X.X";
	string username = "yourUsername";
	string password = "yourPassword";
	string rootPath = "/your/target/folder";

	using (var sftp = new SftpClient(host, 22, username, password))
	{
		sftp.Connect();
		DateTime latest = GetLatestModifiedTimeRecursive(sftp, rootPath);
		sftp.Disconnect();

		Console.WriteLine("Latest modified time: " + latest);
	}
}

static DateTime GetLatestModifiedTimeRecursive(SftpClient sftp, string path)
{
	DateTime latestTime = DateTime.MinValue;

	IEnumerable<ISftpFile> entries = sftp.ListDirectory(path);

	foreach (ISftpFile entry in entries)
	{
		// 略過目前資料夾(.) 與上一層資料夾(..) 的項目
		if (entry.Name == "." || entry.Name == "..")
			continue;

		if (entry.IsDirectory)
		{
			// recursive!!
			DateTime subTime 
				= GetLatestModifiedTimeRecursive(sftp, entry.FullName);
			
			if (subTime > latestTime)
				latestTime = subTime;
		}
		else if (entry.IsRegularFile)
		{
			DateTime fileTime = entry.LastWriteTime;
			if (fileTime > latestTime)
				latestTime = fileTime;
		}
	}

	return latestTime;
}
```

以上程式碼不含 try-catch，有需要的請記得加上。


---
## UPDATE LOG

嘗試寫點更新紀錄？

114.
- 0213 首次新增
- 0507 加上 h 分隔標、擴充調整文章
- 0512 加上 SFTP 版擴充文章、調整文章版面、移動文章內部路徑