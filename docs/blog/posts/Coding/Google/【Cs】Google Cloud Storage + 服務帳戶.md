---
comments: true
categories: []
tags:
date: 2026-01-09 11:05:01
updated: 2026-01-15 10:53:14
---
## 文前言

前篇：[【Cs】Google Drive 自動上傳檔案並共享](【Cs】Google%20Drive%20自動上傳檔案並共享.md)

這次要改成把檔案塞去 Google Cloud Storage，一樣用服務帳戶

起因是需求要把所有檔案通通塞雲端，不要夾帶檔案。  
需求：pdf-上傳、開檔案共享連結、刪除 

## 主文

一樣先建好服務帳戶。  
服務帳戶建立方法可參見前篇，有加上如何建立的段落。

接下來是...  
開 Bucket、上傳、開檔案共享連結、刪除 

使用刪除的原因是檔案留存在 Cloud Storage 越久，費用越高  
不如在不需要用到該檔案時，用程式觸發刪除。

那麼因為雖然存放時間不一定，但一定不會超過半個月，所以這裡就選擇 Standard 了。  
詳細想自行決定可參考：[Cloud Storage 教學―費用節省訣竅、使用方式完整介紹 - Cloud Ace](https://blog.cloud-ace.tw/infrastructure/data-backup/talk-about-cloud-storage/)  
裡面有表格告知儲存空間級別的差別。

### Bucket

需要 Bucket 的原因是檔案要留在 Cloud Storage，就需要有個 Bucket 存放。

1. 從下圖點開  
![](../../../../assets/images/CsGoogle%20Cloud%20Storage%20+%20服務帳戶_bucket.png)

2. 按 [建立]  
![](../../../../assets/images/CsGoogle%20Cloud%20Storage%20+%20服務帳戶_bucket-create.png)

3. 填寫，這裡個人幾乎都保留預設值  
![](../../../../assets/images/CsGoogle%20Cloud%20Storage%20+%20服務帳戶_bucket-create%20detail.png)  
右側面板可以簡單預估可能產生的費用

### 安裝套件

以下是個人有用到的套件：

`Google.Apis.Auth`  
(需要：`Google.Apis`、`Google.Apis.Core`)

`Google.Cloud.Storage.V1`  
(需要：`Google.Apis.Storage`、`Google.Api.Gax`、`Google.Api.Gax.Rest`)

`Google.Api.Gax`            v4.9.0  
`Google.Api.Gax.Rest`       v4.9.0  
`Google.Apis`               v1.73.0  
`Google.Apis.Auth`          v1.73.0  
`Google.Apis.Core`          v1.73.0  
`Google.Apis.Storage.v1`    v1.69.0.3707  
`Google.Cloud.Storage.V1`   v4.13.0

### 驗證身分

沒有身分給驗證是不能做事的！

回到開好的 Bucket，點選 [授予存取權]  
![](../../../../assets/images/CsGoogle%20Cloud%20Storage%20+%20服務帳戶_bucket-detail.png)

在打開的右側面板，填上剛剛建好的服務帳戶的電子郵件  
![](../../../../assets/images/CsGoogle%20Cloud%20Storage%20+%20服務帳戶_bucket-auth.png)

指派角色，這裡先嘗試選擇使用者  
![](../../../../assets/images/CsGoogle%20Cloud%20Storage%20+%20服務帳戶_bucket-auth-access.png)

好了按 [儲存]。


程式中如何取得服務帳戶，原本想參照前篇 [【Cs】Google Drive 自動上傳檔案並共享-取得權限](【Cs】Google%20Drive%20自動上傳檔案並共享.md#主文#簡易範例#取得權限)  
然而發現完全不能通用QQ 那版是走 Drive 的，不是 GCS

諮詢了 Claude 給出的解法：

```cs
string jsonPath = @"C:\path\to\your-service-account-key.json"; 
GoogleCredential credential	= GoogleCredential.FromFile(jsonPath);
```

具體就修改為自己的資訊就好了。

*** 在我研究的期間，Google.Apis.Auth.OAuth2 從 v1.69.0 更新到 v1.73.0  
因此憑證撈取寫法稍微改動了，在 `.FromFile()` 那出現了警告：
> CS0618 'GoogleCredential.FromJson(string)' is obsolete: 
> 
> 'This method is being deprecated because of a potential security risk.  
> Use the methods in the CredentialFactory class instead.  
> A GoogleCredential object can then be created by calling the .ToGoogleCredential() method on the returned specific credential. '

處理來源 Pull request：  
[feat: Add credential type parameter to CredentialFactory by robertvoinescu-work · Pull Request #3064 · googleapis/google-api-dotnet-client](https://github.com/googleapis/google-api-dotnet-client/pull/3064)

如果想改成最新版寫法，請見：  
[c# - Firebase version 3.4.0 not support GoogleCredential.FromFile("") - Stack Overflow](https://stackoverflow.com/questions/79822110/firebase-version-3-4-0-not-support-googlecredential-fromfile)

### 上傳

官方ref: [上傳和下載  |  Cloud Storage  |  Google Cloud Documentation](https://docs.cloud.google.com/storage/docs/uploads-downloads?hl=zh-tw)

因為是第一次嘗試，所以先用**單一要求上傳**試試看。

接著，因為是從本機做檔案上傳，所以繼續參照  
官方ref: [從檔案系統上傳物件  |  Cloud Storage  |  Google Cloud Documentation](https://docs.cloud.google.com/storage/docs/uploading-objects?hl=zh-tw)

所需權限的部分  
因為上傳跟刪除是不同系統，所以分開需求  
1. `storage.objects.create` 跟 `storage.objects.delete`
2. `storage.objects.delete`  
(依照官方文件，delete 是在覆蓋檔案用的。)

!!! warning  
	GCS 跟 Google Drive 不同的是同名檔案在 GCS 內會被覆蓋  
	若不想覆蓋，要另外啟用功能 [物件版本管理](https://docs.cloud.google.com/storage/docs/object-versioning?hl=zh-tw)。

純上傳的部分，官方文件就有給出：  
[從檔案系統上傳物件  |  Cloud Storage  |  Google Cloud Documentation](https://docs.cloud.google.com/storage/docs/uploading-objects?hl=zh-tw#storage-upload-object-client-libraries)  
個人是用 C#，照抄稍微調一下就好。  
*** 記得指定 `content type` 參數！以 pdf 為例，指定好可以利用連結直接預覽檔案。

上傳成功的話，會出現

![](../../../../assets/images/CsGoogle%20Cloud%20Storage%20+%20服務帳戶_upload%20file.png)

再次上傳一次，因為個人沒有特別需求，所以同樣檔名上傳會蓋掉原檔案。

### 列表

官方文件：[List Objects  |  Cloud Storage  |  Google Cloud Documentation](https://docs.cloud.google.com/storage/docs/listing-objects?hl=zh-tw)

很基本的使用了：  
[## 列出值區中的物件 | List Objects  |  Cloud Storage  |  Google Cloud Documentation](https://docs.cloud.google.com/storage/docs/listing-objects?hl=zh-tw#list-objects)  
官方文件給出的 C# 一丟就能用了，因此不在此做過多描述。

#### 篩選

官方提供了兩個可供篩選的選項：前綴、delimiter(定界符)

前綴：給定前綴字串，可以篩選列出所有前綴為它的物件們  
delimiter(定界符): 可限制是否只在給定的目錄底下搜尋

這裡因為個人只需要用到前綴，所以只研究前綴撰寫。  
撰寫方式跟列表差不多，只差在加上一個前綴參數就好。

原本：
```cs
var storageObjects = storage.ListObjects(bucketName);
```
加上前綴：
```cs
var storageObjects = storage.ListObjects(bucketName, **prefix**);
```

### 預覽

需求是跟前篇一樣，使用連結可以打開 pdf 預覽。

先看一眼檔案：  
![](../../../../assets/images/CsGoogle%20Cloud%20Storage%20+%20服務帳戶_bucket%20file%20detail.png)  
其中「已通過驗證的網址」是我可能需要的預覽 pdf 連結，所以目標是抓到它。

程式面上，只找到最相近的官方文件是 [下載物件  |  Cloud Storage  |  Google Cloud Documentation](https://docs.cloud.google.com/storage/docs/downloading-objects?hl=zh-tw)

因此決定直接摸索從 [列表](#列表) 那撈出的 `Google.Apis.Storage.v1` dll (?)

`.Data.Object` 內觀察：

- 先抓出有 link 的參數
	- `.MediaLink`: Media download link. =>應該是下載用的連結？
		- https://storage.googleapis.com/download/storage/v1/b/...
		- 詳細可看：[json - Can't retrieve objects from Google Cloud Storage by their mediaLink - Stack Overflow](https://stackoverflow.com/questions/24024099/cant-retrieve-objects-from-google-cloud-storage-by-their-medialink)
	- `.SelfLink`: The link to this object. =>沒看懂
		- https://www.googleapis.com/storage/v1/b/...


發現那兩個參數給出的結果跟前面兩個網址都不一樣@@  
所以也不對。


於是問了 Claude，得到以下解法：

> 方案 1：使用 Signed URL（推薦）

相關官方文件：

- [經簽署的網址  |  Cloud Storage  |  Google Cloud Documentation](https://docs.cloud.google.com/storage/docs/access-control/signed-urls?hl=zh-tw)
- [使用 Cloud Storage 工具進行 V4 簽署程序  |  Google Cloud Documentation](https://docs.cloud.google.com/storage/docs/access-control/signing-urls-with-helpers?hl=zh-tw#storage-signed-url-object-csharp)

因此：

1. 變更服務帳戶權限，加上 token creator  
	![](../../../../assets/images/CsGoogle%20Cloud%20Storage%20+%20服務帳戶_權限變更.png)
2. 確認服務帳戶對 bucket 依舊有「Storage 物件使用者」權限
   - (應該算是誤打誤撞w)
3. 撰寫程式碼
	```
	// Signed URL 生成流程
	// 1. 使用服務帳戶的私鑰對 URL 進行簽署
	// 2. 生成包含簽章和過期時間的臨時 URL
	// 3. 任何人在有效期內都可透過此 URL 存取檔案
	// 4. 過期後 URL 自動失效
	
	// 需要的參數：
	// - Bucket 名稱
	// - Object 名稱（PDF 檔案路徑）
	// - 有效期限（如 1 小時、7 天等）
	// - 服務帳戶 credential（含私鑰）
	```
4. 重要提醒
	- Signed URL 特性	
		- ✅ 任何人都可以透過此 URL 存取檔案（無需登入）
		- ✅ 有時間限制（最長 7 天）
		- ✅ 無法撤銷（過期前一直有效）
		- ✅ 不需要 bucket 設為公開
		- ✅ 更安全（相比永久公開）	
	- 有效期限建議	
		- **臨時預覽**：1-2 小時
		- **分享連結**：24 小時
		- **長期存取**：7 天（最大值）


具體實作：

- 其中 `objectName` 是檔名喔。(應該...？總之個人用檔名送有成功)
```cs
/// <summary>
/// 取得 Signed Url
/// </summary>
/// <param name="objectName"></param>
/// <returns></returns>
private string GenerateSignedUrl(string objectName)
{
    // 載入憑證
    GoogleCredential credential = GetGcsCredential();

    // 建立 UrlSigner
    UrlSigner urlSigner = UrlSigner.FromCredential(credential);

    // 設定有效期限 (這裡個人設7天)
    TimeSpan duration = TimeSpan.FromDays(7);

    // 產生 Signed URL
    return urlSigner.Sign(bucketName, objectName, duration);
}
```

### 刪除

雖說預覽有做有效期限，但個人需求並不是永久保留檔案在 GCS 上  
(考量到檔案留越久費用越高問題)

因此需要做個刪除處理。


官方文件：[刪除物件  |  Cloud Storage  |  Google Cloud Documentation](https://docs.cloud.google.com/storage/docs/deleting-objects?hl=zh-tw)

!!! info  
	這裡只需求一次刪一筆，所以不探討 [## 大量刪除物件 | 刪除物件  |  Cloud Storage  |  Google Cloud Documentation](https://docs.cloud.google.com/storage/docs/deleting-objects?hl=zh-tw#delete-objects-in-bulk)。


刪除前記得先確認好官方提示：  
![](../../../../assets/images/CsGoogle%20Cloud%20Storage%20+%20服務帳戶_del%20notice.png)

官方文件給出的 C# 一丟就能用了，因此不在此做過多描述。

## UPDATE LOG

115.

01/09 開新文