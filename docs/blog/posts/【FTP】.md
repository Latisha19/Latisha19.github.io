---
categories:
  - Codinggg
tags:
  - Codinggg/FTP
  - Codinggg/Lan/Cs
date: 2025-02-12 09:23:50
updated: 2025-02-13 03:18:56
---

## 基礎

取得遠端某資料夾內所有檔案清單：  
[Follow Fang!: C#.Net 透過FtpWebRequest取得FTP檔案清單](https://cyfangnotepad.blogspot.com/2017/02/cnet-ftpwebrequestftp.html)

自遠端下載檔案：  
[［C#］從ftp server下載檔案＠[C#] 資料結構與影像處理｜PChome Online 個人新聞台](https://mypaper.pchome.com.tw/middlehuang/post/1321704699)

循環從本機上傳資料夾內所有檔案到遠端：

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
            SiteMaster.logger.Info($"檔案 {uploadfile} 上傳至 FTP 成功。({response.StatusDescription})");
        }
    }
    catch (Exception ex)
    {
        SiteMaster.logger.Error($"檔案 {uploadfile} 上傳至 FTP 失敗！錯誤訊息：{ex}");
    }
}
```

## 進階

檢查資料夾是否存在：  
[c# - How to Determine if FTP Directory Exists - Stack Overflow](https://stackoverflow.com/questions/24761583/how-to-determine-if-ftp-directory-exists)

於遠端建立新資料夾：  
[.net - How do I create a directory on FTP server using C#? - Stack Overflow](https://stackoverflow.com/questions/860638/how-do-i-create-a-directory-on-ftp-server-using-c)

建立多層子資料夾：

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
            SiteMaster.logger.Error($"建立 {ftpDirectory} 資料夾失敗！WebException 錯誤訊息：{ex}");
        }
    }
    catch (Exception ex)
    {
        SiteMaster.logger.Error($"建立 {ftpDirectory} 資料夾失敗！其他錯誤訊息：{ex}");
    }
}
```

本機上傳整個資料夾到遠端：  
[recursion - C# Upload whole directory using FTP - Stack Overflow](https://stackoverflow.com/questions/13311975/c-sharp-upload-whole-directory-using-ftp)