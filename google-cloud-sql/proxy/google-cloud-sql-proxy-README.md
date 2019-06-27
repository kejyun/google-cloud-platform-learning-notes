# Cloud SQL Proxy

因為 Cloud SQL 預設是將所有連線擋掉，所以要允許機器連線至 Cloud SQL 時，需要手動設定哪些 IP 允許連線才可以順利連線到資料庫

但是若主機設定 Auto Scaling，則每次的主機 IP 都不同，這樣就沒辦法做到自動 Auto Scaling 還能夠連線到 Cloud SQL 了

為了解決 Auto Scaling 連線到 Cloud SQL 的問題，可以透過 Cloud SQL Proxy 在主機內建立一個跳板，連線到 `本機（127.0.0.1）` 透過憑證驗證授權後再連線到 Cloud SQL


## 下載 Cloud SQL Proxy

> 作業系統 Ubuntu 16.04

```
cd /home/ubuntu
wget https://dl.google.com/cloudsql/cloud_sql_proxy.linux.amd64 -O cloud_sql_proxy
chmod +x cloud_sql_proxy
```

## 建立服務帳號

### ***A. 建立服務帳戶***

前往 [GCP 服務帳戶頁面](https://console.cloud.google.com/iam-admin/serviceaccounts/)，建立服務帳戶

![建立服務帳戶](./images/google-cloud-iam-create-service-account.png)

設定服務帳戶設定資料，輸入服務帳戶名稱 `cloud-sql-proxy-service`

![設定服務帳戶設定資料](./images/google-cloud-iam-setting-service-account.png)


### ***B. 設定服務帳戶角色***

設定服務帳戶角色為 `Cloud SQL 管理員`

![設定服務帳戶角色](./images/google-cloud-iam-choose-service-account-role.png)

設定完成後即可看到角色為 `Cloud SQL 管理員`

![設定服務帳戶角色完成](./images/google-cloud-iam-choose-service-account-role-finish.png)


### ***C. 建立服務帳戶金鑰***

點選 `+ 建立金鑰` 建立此服務帳戶權限可存取的金鑰

![建立服務帳戶金鑰](./images/google-cloud-iam-create-service-account-certificate.png)

選擇服務帳戶金鑰類型為 `JSON` 檔案，點選 `建立` 按鈕後即可下載金鑰檔案

![選擇服務帳戶金鑰類型](./images/google-cloud-iam-create-service-account-certificate-create.png)

下載金鑰完成畫面如下

![儲存服務帳戶金鑰](./images/google-cloud-iam-create-service-account-certificate-save.png)


### ***D. 建立服務帳戶金鑰完成***

完成後就可以看到剛剛建立的服務帳戶出現在帳戶列表了

![建立服務帳戶金鑰完成](./images/google-cloud-iam-create-service-account-finish.png)


## 透過 Cloud SQL Proxy 連線到資料庫

在 `cloud_sql_proxy` 執行檔參數可以設定 `-instances` 指定 Cloud SQL 的連線名稱，透過 `-credential_file` 指定憑證 JSON 檔案路徑

```
./cloud_sql_proxy -instances=<INSTANCE_CONNECTION_NAME>=tcp:3306 \
                  -credential_file=<PATH_TO_KEY_FILE> &
```


### ***A. 找到你的 Cloud SQL instance 連線名稱***

打開 GCP Console 進入 Cloud SQL instance 主機頁面，在下方的 `執行個體連線名稱（Instance connection name）` 就是 `Cloud SQL Proxy` 的 `-instances` 連線名稱，長得會像下面這樣：


> `<專案名稱>:<區域名稱>:<Cloud SQL Instance 名稱>`


> `kejyun-project:asia-east1:kejyun-mysql-asia-east`


![找到你的 Cloud SQL instance 連線名稱](./images/google-cloud-sql-find-instance-name.png)


### ***B. 設定 Cloud SQL 服務帳戶金鑰***


將剛剛下載的 `Cloud SQL 服務帳戶金鑰` 放到 `Google Compute Engine（GCE）` 主機中

```
vim /home/ubuntu/key/kejyun-cloud-sql-proxy.json
```


```json
{
  "type": "service_account",
  "project_id": "kejyun-research",
  "private_key_id": "a03909nwoefnwo0a7a142da5446ea9b14fee2b097",
  "private_key": "-----BEGIN PRIVATE KEY-----\nMIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQCsCw2zRr31w0rM\nTVaQlyzXBy7qoYInmVXMiZ77ti4yEswc/GYBW9K/6jjb0GgpU7FzIkcd+DuFCWNR/Kjv4k2qkbtecjgVsd4OG/ZA\n-----END PRIVATE KEY-----\n",
  "client_email": "cloud-sql-proxy-service@kejyun-research.iam.gserviceaccount.com",
  "client_id": "12345678901234567890",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://oauth2.googleapis.com/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/kejyun"
}
```


### ***C. 建立 Cloud SQL Proxy***


```
/home/ubuntu/cloud_sql_proxy -instances=kejyun-project:asia-east1:kejyun-mysql-asia-east=tcp:3306 -credential_file="/home/ubuntu/key/kejyun-cloud-sql-proxy.json" &
[1] 2379
```


### ***D. 使用 Cloud SQL Proxy 連線到資料庫***

因為 Cloud SQL 是在本機建立 Proxy，透過金鑰去驗證身份連線到資料庫，所以當我們要連線到資料庫時，是直接連線本機 IP 位置 `127.0.0.1`

```
mysql -u root -h 127.0.0.1 -p
```

看到下列畫面表示可以正常透過 Cloud SQL Proxy 連線到資料庫了！！

```shell
$ mysql -u root -h 127.0.0.1 -p

Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 37244
Server version: 5.7.14-google-log (Google)

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```


## 設定開機自動開啟 Cloud SQL Proxy

因為程式是設定固定連線到 `127.0.0.1` 的 `Cloud SQL Proxy` 去連線到資料庫，而我們的 `Cloud SQL Proxy` 是用指令去執行的，所以當 ***主機重新開機*** 或 ***Auto Scaling 開啟新機器*** 時，`Cloud SQL Proxy` 服務又會不見了，所以我們需要在主機開機能夠自動執行 `Cloud SQL Proxy`


### ***A. 建立 Cloud SQL Proxy 可執行 script 檔案***


使用預設 `ubuntu` 帳號身份建立 `vim /home/ubuntu/scripts/boot.sh` 檔案

```
sudo su ubuntu -s /bin/bash
vim /home/ubuntu/scripts/boot.sh
```

在 `vim /home/ubuntu/scripts/boot.sh` 檔案中加入 `Cloud SQL Proxy` 指令

```
/home/ubuntu/cloud_sql_proxy -instances=kejyun-project:asia-east1:kejyun-mysql-asia-east=tcp:3306 -credential_file="/home/ubuntu/key/kejyun-cloud-sql-proxy.json" &
```

將 `/home/ubuntu/scripts/boot.sh` 設定為可執行檔


```
chmod +x /home/ubuntu/scripts/boot.sh
```

### ***B. 加入開機自動執行***

編輯開機自動執行指令檔案 `/etc/rc.local`

```
vim /etc/rc.local
```

在最後方加入執行 `/home/ubuntu/scripts/boot.sh` 檔案存擋即可

```
bash /home/ubuntu/scripts/boot.sh
```

這樣在 ***主機重新開機*** 或 ***Auto Scaling 開啟新機器*** 時，就可以自動開啟 `Cloud SQL Proxy` 了


## 自訂 Cloud SQL Proxy 本地端 port

```
./cloud_sql_proxy -instances=myProject:us-central1:myInstance=tcp:3306,myProject:us-central1:myInstance2=tcp:3307 &
mysql -u myUser --host 127.0.0.1  --port 3307
```


## 參考資料
* [Connecting to Google Cloud SQL with the Cloud SQL Proxy - YouTube](https://www.youtube.com/watch?v=25XIGXbw_GY)
* [使用 Cloud SQL Proxy 連結 mysql 用戶端  |  MySQL 適用的 Cloud SQL  |  Google Cloud](https://cloud.google.com/sql/docs/mysql/connect-admin-proxy)
* [從外部應用程式連線至 Cloud SQL  |  MySQL 適用的 Cloud SQL  |  Google Cloud](https://cloud.google.com/sql/docs/mysql/connect-external-app)
* [startup - How to run scripts on start up? - Ask Ubuntu](https://askubuntu.com/questions/814/how-to-run-scripts-on-start-up)
* [About the Cloud SQL Proxy  |  Cloud SQL for MySQL  |  Google Cloud](https://cloud.google.com/sql/docs/mysql/sql-proxy)
