# Auto Scaling

## 建立 Auto Scaling 群組

### A. 執行個體範本（Instance templates）

GCP 的 `建立執行個體範本（Instance templates）` 就像是 AWS 的 `Auto Scaling Launch Configurations`，若要執行在 GCP Auto Scaling 必須先設定要執行的機器是哪一種，建立一個可執行的範本


#### ***1. 建立執行個體範本（Instance templates）***

在 `執行個體範本` 頁面點選 `建立執行個體範本` 按鈕，即可開始建立 Auto Scaling 需要的 VM 範本

![建立執行個體範本（Instance templates）](./images/google-vm-instance-templates-create.png)

#### ***2. 設定執行個體範本***

在設定執行個體範本頁面設定範本 `名稱`，並選擇範本的 `機器類型`

若建立的機器為 web 服務，則可以勾選允許 HTTP 及 HTTPS 流量

![設定執行個體範本（Instance templates）](./images/google-vm-instance-templates-create-setting.png)


#### ***3. 設定執行個體範本***

在映像檔案可以選擇自己建立的映像檔案，並設定硬碟大小

![設定執行個體範本（Instance templates）映像檔](./images/google-vm-instance-templates-create-setting-images.png)

#### ***4. 設定執行個體範本開機指令碼***

若有需要再開機時需要執行的程式可以在 `開機指令碼` 輸入相關的 shell script

這樣在範本主機建立時就會執行此區塊的程式

![設定執行個體範本（Instance templates）開機指令碼](./images/google-vm-instance-templates-create-boot-scripts.png)

#### ***5. 完成設定執行個體範本***

在完成設定後就可以看到 `執行個體範本` 已經設定完成

> 範本設定不會被 GCP 收取任何費用

![完成設定執行個體範本（Instance templates）](./images/google-vm-instance-templates-create-finish.png)



## 參考資料
* [Google Cloud Load balancer & Autoscaling in ACTION!! (Udemy link below with discount code!) - YouTube](https://www.youtube.com/watch?v=Gn7pGQYkKnA)
