【深入 Azure 雲端服務】
---
<a name="page1"></a>
## 簡介、目錄



延續上一系列  [三十天.NET❤️Azure漸進式開發專案 :: 2019 iT 邦幫忙鐵人賽](https://ithelp.ithome.com.tw/users/20105988/ironman/1622)

> 主要寫給有 .NET(C#)、SQL-Server 開發基礎的讀者
> 如何在三十天使用 .NET 配合 Azure 做漸進式從規劃、開發、測試、上線
> 其中會運用到 Azure : WebApp 部屬 AP 環境、SQL-Server資料庫、Redis...等
> 認真跟完全部文章，可以做出屬於自己的網頁專案

今年將繼續帶讀者深入學習 Azure , 建議還沒有 Azure 基礎概念讀者先去閱讀上一系列文章 ^_^



----



## 本系列簡介

不知道大家有沒有用過線上測試服務、或是雲端軟體?

以 [Odoo](https://www.odoo.com/?utm_source=google&utm_medium=cpc&utm_campaign=Branding-English_Brand-Odoo&utm_term=odoo&utm_matchtype=e&utm_device=c&utm_gclid=Cj0KCQjwnqH7BRDdARIsACTSAdt1dDeuJWxuihPtlEpTirgSfnM_Z_tAd5BWdrLLGMamNh_iES9lMWQaArEuEALw_wcB&gclid=Cj0KCQjwnqH7BRDdARIsACTSAdt1dDeuJWxuihPtlEpTirgSfnM_Z_tAd5BWdrLLGMamNh_iES9lMWQaArEuEALw_wcB)、為例子，假如自己在電腦架設需要安裝 python、postgresql 花很多時間做一堆設定

但在 Odoo 官網只需要 Mail 註冊、點幾個按鈕，一個線上的ERP服務就架設起來，並且時間一到自動刪除資源，好不方便!

像這樣的功能，藉由 Azure 我們也可以辦，並且細部控制商業環境最重要的`成本`問題，從`免費到低到漸進式費用增長`。



本系列將會帶大家如何`從無到有、腳本自動化`建立類似的服務，但需要讀者們循序漸進跟著文章動手實作。



---

## 預計主題、目錄 : 

1. Azure 範本 ARM Template - 基礎 
2. Azure 範本 ARM Template - 進階
3. Azure 自動化入門- 動態屬性生成
4. Azure CLI 入門、工具使用
5. Azure CLI + Linux  - Bash 批量操作
6. Azure CLI + Linux + PowerShell  - 基礎
8. Azure CLI + Powershell + ARM Template + Linux + Docker  整合從無到有快速布置網頁
9. 開發 Azure 自動化服務 - 自動化帳戶、排程
10. 開發 Azure 自動化服務 - 實作
11. 開發 Azure 自動化服務  - 上線
12. 牛刀小試 : 動態設定 IP 白名單
13. 牛刀小試 : 生成老闆最愛的營運報表 - 概念
14. 牛刀小試 : 生成老闆最愛的營運報表 - 實作
15. 牛刀小試 : 生成老闆最愛的營運報表 - 上線
16. 自製費用警報器 : 超過預算,發訊息到手機、Line、Mail
17. 自製費用排程器 : 超過上限批量停止服務,避免帳單爆炸 - 概念
18. 自製費用排程器 : 超過上限批量停止服務,避免帳單爆炸 - 實作
19. 自製費用排程器 : 超過上限批量停止服務,避免帳單爆炸 - 上線
20. 自製警報器 : 安全漏洞自動通知
21. Azure 與 Docker 配合使用 - 概念
22. Azure 與 Docker 配合使用 - 實作
23. Azure 與 Docker 配合使用 - 上線
24. Azure 自動申請 Let’s Encrypt Https 憑證 - 概念
25. Azure 自動申請 Let’s Encrypt Https 憑證 - 實作
26. Azure 自動申請 Let’s Encrypt Https 憑證 - 上線
27. 應用 : 網頁用戶點點按鈕就可以建立獨立產品 demo 環境 - 基礎概念
28. 應用 : 網頁用戶點點按鈕就可以建立獨立產品 demo 環境 - 實作1
29. 應用 : 網頁用戶點點按鈕就可以建立獨立產品 demo 環境 - 實作2
30. 應用 : 網頁用戶點點按鈕就可以建立獨立產品 demo 環境 - 上線
31. 應用: 客制公司專屬戰情中心 - 概念
32. 應用: 客制公司專屬戰情中心 - 實作
33. 應用: 客制公司專屬戰情中心 - 上線



---
<a name="page2"></a>
## Azure 範本 ARM Template - 基礎

使用 Azure Portal 建立資源雖然有方便的GUI，但是需要經過一些專業的設定後才能正式上線，這些對於客戶、新人`需要有學習成本`，讓他們自行設定又會有`人為出錯的風險`。為此微軟提供 [ARM (Azure Resource Manager) Template](https://docs.microsoft.com/zh-tw/azure/azure-resource-manager/templates/overview)  讓維運、開發人員能建立標準化範本`只需要幾個文件，任何人都可以簡單部屬類似環境`。



ARM Template 入門的使用方式非常簡單，以建立一個 VM 為例子，只需要在 Azure Portal 使用 GUI 選好自己想要的規格、設定，之後在`「檢閱 + 建立」` 的畫面點選`「下載自動化的範本」`就可以，如圖片。

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

---
<a name="page3"></a>
## Azure 範本 ARM Template -  進階

前面介紹了給客戶、新人的`簡單範本`分享跟建立方式，跟了解基本範本化的概念、好處，接下來進階了解 Template 的一些細節概念。

對 Azure 的維運跟開發人員，需要了解 ARM Template 不只是 JSON 資料而已，可以增加`微邏輯、防呆限制`，甚至可以使用`微代碼生成出客製化 Azure Portal 頁面`。



舉例 :  創建一個低配、簡單的 storage 帳號 ，我們可以這樣寫 Template JSON

意思是使用 2019-04-01 標準 API 創建 V2 版本 storage ，名稱為 demo01，地點在東亞(香港)，使用 SKU Standard_LRS (該SKU代表支持網路、容器、文件加密、變更通知、存檔預覽)。

```json
{
    "$schema":"https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts", 
            "apiVersion":"2019-04-01", 
            "name": "weist20200919",
            "location": "eastasia",
            "sku": {
                "name": "Standard_LRS" 
            },
            "kind": "StorageV2"
        }
    ]
}
```

> 備註1 : SKU = Stock-keeping-Unit，主要是由微軟想定義特定產品編碼用，可以在[Skus - List](https://docs.microsoft.com/en-us/rest/api/storagerp/skus/list)、[SKU Types](https://docs.microsoft.com/en-us/rest/api/storagerp/srp_sku_types)查詢所有SKU對應的服務功能。
>
> 備註2 : 雖然 Azure Portal 範本群組區域跟資源區域可以設定不同，但建議`同一群組區域一樣為好`。
>
> 備註3 : 儲存體名稱要求`全球唯一`，否則會出現 `名稱為 amdemostorageintro01 的儲存體帳戶已在使用。`錯誤。



接著對上述的 JSON Template 進行思考 :   

Q1. 每次都只能選擇 Storage V2 不能選擇 V1，不能自訂義名稱每次都要修改 JSON `是否缺少一些彈性`? 

Q2. 資源名字大家可以隨便亂取，是否會造成`規範上的混亂? `   

  

對於Q1的問題，我們可以使用  `parameters` + `allowedValues` 關鍵字達到效果  

舉例 :  如下增加自訂義參數 kinds，型別為字串只准許選擇 `StorageV2`、`StorageV1`；新增參數 storageName 讓使用者可以`自訂名稱`。



```json
    "parameters": {
        "kinds": {
            "type": "string",
            "allowedValues": [
                "StorageV2", 
                "StorageV1"
            ]
        },  
        "storageName": {
            "type": "string"
        }          
    }
```

完整例子 :  

```json
{
    "$schema":"https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "kinds": {
            "type": "string",
            "allowedValues": [
                "StorageV2", 
                "StorageV1"
            ]
        },  
        "storageName": {
            "type": "string"
        }        
    },    
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts", 
            "apiVersion":"2019-04-01", 
            "name": "[parameters('storageName')]",
            "location": "eastasia",
            "sku": {
                "name": "Standard_LRS" 
            },
            "kind": "[parameters('kinds')]"
        }
    ]
}
```



在 Azure Portal 範本就能`自動生成自訂頁面`，按照參數生成對應的下拉式選單、輸入框。

![image-20200919110703814](https://i.loli.net/2020/09/19/NBwOL3GdWHhqUDn.png)



> 備註 : 使用Azure Portal 範本方式 
> 1. 在搜尋欄輸入`範本` > 新增輸入名稱、描述 > 將 JSON 複製貼上 > 新增範本 > 點擊範本 - 部屬 
> 2. 在搜尋欄輸入`部署自訂範本` > 點 `Build your own template in the editor` > 貼上JSON Template，缺點是每次要重新複製貼上一次JSON
>

影片 :  
[![Yes](https://img.youtube.com/vi/h5lD76JDS5s/0.jpg)](https://www.youtube.com/watch?v=h5lD76JDS5s)  


Q2的問題，我們可以使用  `parameters` + `defaultValue、min(max)Length`  關鍵字達到限制規範效果

舉例 : 限制必須要輸入15-17 個字，並一開始自動帶出`IAmDefaultValue`的預設值

```json
    "parameters": {
        "storageName": {
            "type": "string",
            "minLength": 8, 
            "maxLength": 9,
            "defaultValue": "IAmDefaultValue"
        }
    }
```

完整 JSON 如下

```json
{
    "$schema":"https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageName": {
            "type": "string",
            "minLength": 8, 
            "maxLength": 9,
            "defaultValue": "IAmDefaultValue"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts", 
            "apiVersion":"2019-04-01", 
            "name": "[parameters('storageName')]",
            "location": "eastasia", 
            "sku": {
                "name": "Standard_LRS" 
            },
            "kind": "StorageV2"
        }
    ]
}
```

![image-20200919133412955](https://i.loli.net/2020/09/19/NqrERpo2IiTFCZY.png)



> 備註 :  目前可用的區域對應的編號
>
> ```
> centralus,eastasia,southeastasia,eastus,eastus2,westus,westus2,northcentralus,southcentralus,westcentralus,northeurope,westeurope,japaneast,japanwest,brazilsouth,australiasoutheast,australiaeast,westindia,southindia,centralindia,canadacentral,canadaeast,uksouth,ukwest,koreacentral,koreasouth,francecentral,southafricanorth,uaenorth,australiacentral,switzerlandnorth,germanywestcentral,norwayeast
> ```
>
> 備註 : 目前 API 版本
>
> ```
> '2019-06-01, 2019-04-01, 2018-11-01, 2018-07-01, 2018-03-01-preview, 2018-02-01, 2017-10-01, 2017-06-01, 2016-12-01, 2016-05-01, 2016-01-01, 2015-06-15, 2015-05-01-preview'
> ```
>





---
<a name="page4"></a>
<!--Azure 自動化入門- 動態屬性生成-->


演示影片 : 
[![Yes](https://img.youtube.com/vi/jKiGKZ5ZtwY/0.jpg)](https://www.youtube.com/watch?v=jKiGKZ5ZtwY)


在了解 Azure 自動化先讓我們從 ARM Template expression / function 開始，Template `支援有限 expression / function`，而這些 API 可以讓我們做到有限自動化效果。



舉例 : 在批量自動生成時，每次建立、調整 Azure 資源我們`不會想人為調整`，像是名稱、地點。

1.自動生成名稱

因為 Azure 上很多服務的`名稱是要求全球唯一`，重複會無法創建，所以自動化部屬時如何生成唯一不衝突的 Name 是一個小學問。

Azure 在此有提供 `concat + uniqueString + guid`方式可以解決此問題

邏輯 : 

1. 先 groupid 生成 guid 取得`難以重複的值`

2. 再交由 uniqueString 做一次哈希動作，縮短長度，原因是 Azure `名稱有長度限制`，請看備註
3. 再交由 concat 連接特徵開頭詞`auto`，方便後面辨認是自動生成的資源，並減少與其他全球 Name 衝突的機率。

如以下 JSON : 

```json
{
    "resources": [
        {
            "name": "[concat('auto',uniqueString(guid(resourceGroup().id)))]",
        }
    ]
}
```



>  備註 : 為何要使用uniqueString而不是guid，原因是有些資源`有長度限制`，像儲存體名稱就只能`3-24長度`，假如使用`"name": "[concat('auto',guid(resourceGroup().id))]"`會出現錯誤`autoe4c2f084-f34f-59c0-8e90-31c7d6d1c304 不是有效的儲存體帳戶名稱。儲存體帳戶名稱的長度必須介於 3 到 24 個字元之間，且只可使用數字與小寫字母。`



2.自動以群組的地點區域作為創建區域

維運人員通常`不會想要自己創建的資源區域跟群組不同`， 對此 Azure 有提供 `resourceGroup().location`解決此問題，`自動以群組地區為準`創建。

```
{
    "resources": [
        {
            "location": "[resourceGroup().location]"
        }
    ]
}
```



完整 JSON Template :  

```
{
    "$schema":"https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",  
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts", 
            "apiVersion":"2019-04-01", 
            "name": "[concat('auto',uniqueString(guid(resourceGroup().id)))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS" 
            },
            "kind": "StorageV2"
        }
    ]
}
```




> 備註 : 想查詢所有 funtion 可以在 [Template functions ](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-functions)查詢



---
<a name="page5"></a>
## Azure CLI 入門、工具使用



本節建議看演示影片最快  :  
[![Yes](https://img.youtube.com/vi/SjNnXwrLT1U/0.jpg)](https://www.youtube.com/watch?v=SjNnXwrLT1U)



---



Azure 想要做到完全自動化，最強的原裝搭配就是 `ARM Template + Azure CLI + PowerShell (or Bash)` ! 

所以接著先介紹 Azure CLI，首先工欲善其事，必先利其器，總共有兩個工具推薦使用 

1. Azure Portal Cloud Shell
2. 本機 Visaul Studio Code + Azure CLI 安裝



我們先從前者 Cloud Shell 開始介紹，其特點是 `開箱即用`、`Azure CLI環境都先安裝好`，只需要打開網頁 [https://shell.azure.com/](https://shell.azure.com/) 並配上 `code .` 打開 VSCode Online。

並依照之前的範本創建一個 Template JSON，並輸入以下 Azure CLI 命令

```
az group create --name demo --location eastasia
az deployment group create --resource-group demo --template-file demo.json
```

![image-20200920221055135](https://i.loli.net/2020/09/20/Wg6BkfENxsRjyIM.png)



接著介紹微軟強大的開源工具 `Visual Studio Code 加 Azure CLI `搭建本機的開發環境

1.安裝 [Visual Studio Code](https://code.visualstudio.com/)

2.安裝 [Azure CLI ](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows?view=azure-cli-latest&tabs=azure-cli)

3.安裝整合套件 [Azure Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack)



![image-20200920233056490](https://i.loli.net/2020/09/20/DfdZGwRH6Qck3as.png)

 

另外 Azure CLI 也有提供很棒的查詢工具`az find "查詢條件"`，比起一般的 `--help` 更加智能，像是我想查詢如何創建資源群組，只需要打`how to create azure group`，就會幫我找出範例 :  

```bash
PS /home/weihan> az find "how to create azure group"
Finding examples...

Here are the most common ways to use [how to create azure group]:

Create a container in a container group that mounts an Azure File share as volume.
az container create --resource-group MyResourceGroup --name myapp --image myimage:latest --command-line "cat /mnt/azfile/myfile" --azure-file-volume-share-name myshare --azure-file-volume-account-name mystorageaccount --azure-file-volume-account-key mystoragekey --azure-file-volume-mount-path /mnt/azfile

Create a new resource group in the West US region. (autogenerated)
az group create --location westeurope --resource-group MyResourceGroup

Create a container in a container group using container image from Azure Container Registry.
az container create --resource-group MyResourceGroup --name myapp --image myAcrRegistry.azurecr.io/myimage:latest --registry-password password

Please let us know how we are doing: https://aka.ms/azureclihats
```

![image-20200923170107930](https://i.loli.net/2020/09/23/MvtheDKSIlQ5B87.png)

只是多國語言支持還是有問題  (英文很重要 ~)

![image-20200923170027136](https://i.loli.net/2020/09/23/l1nsEzufxqUmpID.png)



---
<a name="page6"></a>
## Azure CLI + Linux  - Bash 批量操作



演示影片 : 

[![Yes](https://img.youtube.com/vi/GaOIFixC3JA/0.jpg)](https://www.youtube.com/watch?v=GaOIFixC3JA)



---

近年來隨著微軟擁抱開源社群 : Microsoft ❤ Linux ，在 Azure 上面我們也可以使用 Linux 的 Bash 來幫助我們完成維運腳本的編寫。

為什麼能這樣做呢? 

一個重要的概念 :  `Azure Cloud Shell 底層就是 Ubuntu Linux`

<!--

```
cat /etc/os-release
```

-->

![image-20200921224250563](https://i.loli.net/2020/09/21/JTjiEoeBFDxpVUs.png)

這也代表說，管理 Azure 不必要綁死在 Windows，其他支持 Bash 的平台都能使用，達到`跨平台管理`的效果，這點真的很棒 !



接著讓我們向 Azure 說你好

最簡單方式 : 在 [Azuer Cloud Shell](https://shell.azure.com/) 選擇 Bash 就可以簡單使用 Shell 指令

```bash
#!/bin/bash
for (( i=1; i<=10; i++ ))
do  
   echo "Hello Azure IT Help $i times"
done
```

![image-20200921221835818](https://i.loli.net/2020/09/21/FBTL6RZW7uyxOab.png)



再來做一個批量管理資源的例子 :  在 Azure 做`批量建立、刪除`的動作，一次性建立、刪除10個群組，我們可以這樣做

```bash
# 創建批量建立群組shell
weihan@Azure:~$ vim demo.sh
#!/bin/bash
for (( i=1; i<=10; i++ ))
do  
   az group create --name "demo$i" --location eastasia
   echo "Create $i Azure Group"
done

# 執行創建
weihan@Azure:~$ bash demo.sh
Create 1 Azure Group
..
Create 10 Azure Group

# 查詢結果
weihan@Azure:~$ az group list --query "[]" -o table
Name    Location
------  ----------
demo1   eastasia
demo2   eastasia
...
demo10  eastasia

# 批量刪除
weihan@Azure:~$ touch demo1.sh
weihan@Azure:~$ code demo1.sh

# 創建批量刪除群組shell
#!/bin/bash
for (( i=1; i<=10; i++ ))
do  
   az group delete --name "demo$i" --no-wait -y 
   echo "Delete demo$i Azure Group"
done

weihan@Azure:~$ ls
demo1.sh  demo.json  demo.sh

weihan@Azure:~$ bash demo1.sh
Delete demo1 Azure Group
..
Delete demo10 Azure Group
```



因為是基於 Ubuntu 所以你想要在 Azure Cloud 上面寫 Python 也沒問題 ，非常方便維運人員延續使用`之前的python腳本工具` ^_^

![image-20200921224618683](https://i.loli.net/2020/09/21/vyCswHohYEdZDuT.png)



---
<a name="page7"></a>
## Azure CLI + Linux + PowerShell  - 基礎



演示影片 : 

上傳中...



---

介紹完簡單 Bash 後接著介紹 Azure 的 Linux PowerShell，沒錯，PowerShell 不只能在 Windows 底下，現在也能在 Linux 環境中使用，喊的口號就是 `PowerShell for every system`。能做到這件事歸功於近年微軟推廣的 `.NET Core 跨平台` ，可以在開源的 Github [PowerShell.Core](https://github.com/PowerShell/PowerShell) 官網看到全部是由 C# 編寫。

![image](https://user-images.githubusercontent.com/12729184/93893053-c9392380-fd1f-11ea-9086-3f9470c15c8e.png)



PowerShell 可以讓我們用物件化的概念來寫腳本，像是之前的 Template 我們可以這樣寫，用到 .NET Core 的 Guid Function，動態生成唯一字串，拿來生成資源。

```powershell
$guid = [System.guid]::NewGuid().toString("N");
(New-AzResourceGroup -Name $guid -Location "eastasia").ResourceGroupName;
```



![image-20200923000745016](https://i.loli.net/2020/09/23/HPTgfaZX2NCj3Yp.png)



不喜歡 AZ PowerShell  ?  也可以使用 Azure CLI 混搭配 ，像是使用 Azure CLI 建立資源後使用 PowerShell 的 `ConvertFrom-Json` function 轉成物件 

```powershell
az group list | ConvertFrom-Json | select-Object  name,type
```

![image-20200923155523487](https://i.loli.net/2020/09/23/mkQrXlGVg8qzvHo.png)



批量動作 :  

```powershell
# PowerShell + Az PowerShell 
foreach($i in 1..5){
  $guid = [System.guid]::NewGuid().toString("N");
  $group = New-AzResourceGroup -Name $guid -Location "eastasia" | select-Object  ResourceGroupName,Location ;
  Write-Host("創建 group $group");
}

# 等同於以下 PowerShell + Azure CLI
foreach($i in 1..5){
  $guid = [System.guid]::NewGuid().toString("N");
  $newGroup = az group create --name $guid --location "eastasia" | ConvertFrom-Json | select-Object  name,location ;
  Write-Host("創建 group $newGroup");
}
```



因為是在 Linux 環境，我們甚至可以搭配使用 Linux 的`工具` ，舉例使用 `grep`篩選後，使用 powershell 的 foreach 批量做管理

```powershell
$gs = az group list  -o table | grep demo
foreach ($g in $gs) {
  # 管理
    Write-Host($g.Name)
}
```

![image-20200923115747652](https://i.loli.net/2020/09/23/TVs6Ky1vif3mBHh.png)





>  備註 :
>
> - [Azure PowerShell 介紹](https://docs.microsoft.com/zh-tw/powershell/azure/?view=azps-4.6.1)
> - [Az PowerShell Resources 所有命令](https://docs.microsoft.com/en-us/powershell/module/az.resources/?view=azps-4.7.0#resources)
> - [Azure CLI Resources 所有命令](https://docs.microsoft.com/en-us/cli/azure/reference-index?view=azure-cli-latest)



<!--

介紹使用 JMESPath語法

The Azure CLI, if you’re used to powershell, is not very familiar syntax wise, it has very limited tab completion and the more in depth you go you have to start referencing the docs for things like [JMESPath](http://jmespath.org/) query language.

-->

---
<a name="page8"></a>
## Azure CLI + Powershell + ARM Template + Linux + Docker  整合從無到有快速布置網頁

---

影片 :  
上傳中...

----

這次我們整合前面幾章所學，將 Azure CLI + Powershell + ARM Template + Linux + Docker  整合做一個網頁從無到有快速布置。

這樣的功能動態建立的 script 要寫多少命令呢? 

只需要`短短`幾行命令 : 

```Powershell
$guid = [System.guid]::NewGuid().toString("N");
az group create --name $guid --location eastasia
$json = az deployment group create --resource-group $guid --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-aci-linuxcontainer-public-ip/azuredeploy.json" ;
$docker = $json | ConvertFrom-Json ;
$ip = $obj.properties.outputs.containerIPv4Address.value ;
curl "http://$ip/" | grep "<title>.*</title>"
```



完整 log : 

```Powershell

PS /home/weihan> $guid = [System.guid]::NewGuid().toString("N");
PS /home/weihan> az group create --name $guid --location eastasia
{
  "id": "/subscriptions/../resourceGroups/c9963c964b8f4bb08b98998a885db502",
  "location": "eastasia",
  "managedBy": null,
  "name": "c9963c964b8f4bb08b98998a885db502",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": "Microsoft.Resources/resourceGroups"
}

PS /home/weihan> $docker = az deployment group create --resource-group $guid --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-aci-linuxcontainer-public-ip/azuredeploy.json"
{- Finished ..
  "id": "/subscriptions/../resourceGroups/../providers/Microsoft.Resources/deployments/azuredeploy",
  "location": null,
  "name": "azuredeploy",
  "properties": {
    #...
    "outputs": {
      "containerIPv4Address": {
        "type": "String",
        "value": "52.184.73.244"
      }
    }
    #...
}

PS /home/weihan> $ip = $newDocker.properties.outputs.containerIPv4Address.value;
PS /home/weihan> curl "http://$ip/" | grep "<title>.*</title>"
  <title>Welcome to Azure Container Instances!</title>
  <title>ContainerInstances_rgb_UI</title>
```

![image-20200923224048012](https://i.loli.net/2020/09/23/zDUc8R3hY9TlEs4.png)


