# 複製本地端檔案到 GCS

## 安裝 `gcloud` 指令

```
curl https://sdk.cloud.google.com | bash
```

```
exec -l $SHELL
```

```
gcloud init
```

## 複製檔案到 GCS

**本機圖片複製至 GCS**

```
gsutil cp -r {local_file_path} gs://{bucket-name}/{gcs_file_path}
```

```
gsutil cp -r {本地端路徑}  gs://{bucket 名稱}/{GCS 路徑}
```

**略過已存在檔案並設定為公開**

```
gsutil -m cp -n -a public-read -R {local_file_path}  gs://{bucket-name}/{gcs_file_path}
```

```
gsutil -m cp -n -a public-read -R {本地端路徑}  gs://{bucket 名稱}/{GCS 路徑}
```

## 查詢 GCS 檔案數

```
gsutil ls -lR gs://{bucket-name}/{gcs_file_path}
```

```
gsutil ls -lR gs://{bucket 名稱}/{GCS 路徑}
```


## 參考資料
* [安裝 gsutil  |  Cloud Storage  |  Google Cloud](https://cloud.google.com/storage/docs/gsutil_install?hl=zh-tw#linux)
