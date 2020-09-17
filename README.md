【深入 Azure 雲端服務】
----

<!--【深入 Azure 雲端服務】30天主題、目錄 -->
# 目錄

延續上一系列  [三十天.NET❤️Azure漸進式開發專案 :: 2019 iT 邦幫忙鐵人賽](https://ithelp.ithome.com.tw/users/20105988/ironman/1622)

> 主要寫給有 .NET(C#)、SQL-Server 開發基礎的讀者
> 如何在三十天使用 .NET 配合 Azure 做漸進式從規劃、開發、測試、上線
> 其中會運用到 Azure : WebApp 部屬 AP 環境、SQL-Server資料庫、Redis...等
> 認真跟完全部文章，可以做出屬於自己的網頁專案

今年將繼續帶讀者深入學習 Azure , 建議還沒有 Azure 基礎概念讀者先去閱讀上一系列文章 ^_^

預計主題、目錄 : 

1. Azure 簡單範本化 ARM (Azure Resource Manager) 
2. Azure CLI 基礎
3. Azure CLI 實作
4. Azure CLI 進階
5. Power Shell + Azure CLI 編寫批量腳本 - 基礎
6. Power Shell + Azure CLI 編寫批量腳本 - 進階
7. 牛刀小試 : 客製編寫 Azure 服務架設、優化腳本 - 概念
8. 牛刀小試 : 客製編寫 Azure 服務架設、優化腳本 - 實作
9. 牛刀小試 : 客製編寫 Azure 服務架設、優化腳本 - 上線
10. 牛刀小試 : 動態設定 IP 白名單
11. 牛刀小試 : 生成老闆最愛的營運報表 - 概念
12. 牛刀小試 : 生成老闆最愛的營運報表 - 實作
13. 牛刀小試 : 生成老闆最愛的營運報表 - 上線
14. 自製費用警報器 : 超過預算,發訊息到手機、Line、Mail
15. 自製費用排程器 : 超過上限批量停止服務,避免帳單爆炸 - 概念
16. 自製費用排程器 : 超過上限批量停止服務,避免帳單爆炸 - 實作
17. 自製費用排程器 : 超過上限批量停止服務,避免帳單爆炸 - 上線
18. 自製警報器 : 安全漏洞自動通知
19. Azure 與 Docker 配合使用 - 概念
20. Azure 與 Docker 配合使用 - 實作
21. Azure 與 Docker 配合使用 - 上線
22. Azure 自動申請 Let’s Encrypt Https 憑證 - 概念
23. Azure 自動申請 Let’s Encrypt Https 憑證 - 實作
24. Azure 自動申請 Let’s Encrypt Https 憑證 - 上線
25. 應用 : 網頁用戶點點按鈕就可以建立獨立產品 demo 環境 - 基礎概念
26. 應用 : 網頁用戶點點按鈕就可以建立獨立產品 demo 環境 - 實作1
27. 應用 : 網頁用戶點點按鈕就可以建立獨立產品 demo 環境 - 實作2
28. 應用 : 網頁用戶點點按鈕就可以建立獨立產品 demo 環境 - 上線
29. 應用: 客制公司專屬戰情中心 - 概念
30. 應用: 客制公司專屬戰情中心 - 實作
31. 應用: 客制公司專屬戰情中心 - 上線

--- 

### Azure 簡單範本化 ARM(Azure Resource Manager)

使用 Azure Portal 建立資源雖然有方便的GUI，但是需要經過一些專業的設定後才能正式上線，這些對於客戶、新人`需要有學習成本`，讓他們自行設定又會有`人為出錯的風險`。為此微軟提供 [ARM (Azure Resource Manager) ](https://docs.microsoft.com/zh-tw/azure/azure-resource-manager/templates/overview) 讓維運、開發人員能建立標準化範本`只需要幾個文件，任何人都可以簡單部屬類似環境`。



ARM 入門的使用方式非常簡單，以建立一個 VM 為例子，只需要在 Azure Portal 使用 GUI 選好自己想要的規格、設定，之後在`「檢閱 + 建立」` 的畫面點選`「下載自動化的範本」`就可以，如圖片。

![image](https://user-images.githubusercontent.com/12729184/93490483-1b063600-f93b-11ea-9c83-9feab5686c4c.png)



![image-20200917211934773](https://i.loli.net/2020/09/17/lhKtQ93pvZ6UIM8.png)





接下來會遇到`如何分享給其他人進行部屬問題`

傳統方式 : 傳 template json 資料給客戶，請對方在 Azure Portal 搜尋 `範本` > 上傳 JSON 檔案 > 還需要填寫一般資訊、名稱、描述、確認、新增，有點麻煩 (另外還有 Azure CLI 發布方式，但還要請對方在本地安裝)

簡便方式 : 這是筆者意外發現的小技巧，可以在 [Github QuickStart Templates](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux) 發現每個頁面都有一個 `Deploy to Azure` 按鈕 ， 點一下就能自動跳轉到發佈頁面，非常方便。

![image-20200917221904865](https://i.loli.net/2020/09/17/FQtAgvXEYB6RnWa.png)

發現其實是使用 URL + API 做到此方便功能，只需要遵循以下規則 : 

1. API URL : `https://portal.azure.com/#create/Microsoft.Template/uri/{deploy.json的URL}`
2. 必是 public 公開連結

假如是資源放在 Github 必須要

1. 把 `https://github.com/` 替代為`https://raw.githubusercontent.com/`
2. `blob/` 要刪掉
3. `:` 要替代為 `%3A`
4. `/` 要替代為 `%2F`

舉例 : `https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-simple-linux/azuredeploy.json` 要換成 `https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2F%2Fmaster%2F101-vm-simple-linux%2Fazuredeploy.json`

有點小麻煩嗎? 沒關係，筆者這邊有用 Vue 寫一個小轉換工具 [【Github URL 轉 Azure Deploy URL】](https://shps951023.github.io/Learn-Azure/tools/GithubUrlToAzureDeployUrl.html) ，貼上去就可以自動替換 ^_^

![image](https://user-images.githubusercontent.com/12729184/93486136-6a963300-f936-11ea-9a8d-47ba46b2bc69.png)

假如考慮`資安問題`，可以放在公司的 File Server , Url 後面記得帶個 `UUID 隨機生成的 token 參數` ，限制特定使用者才能查看。



最後簡單讓客戶、新人簡單點擊`驗證 + 建立`即可創建範本資源 ^_^

![image](https://user-images.githubusercontent.com/12729184/93489349-fcec0600-f939-11ea-8fb2-928befa1ed86.png)
