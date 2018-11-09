# 映像檔（Images）


## 建立映像檔

### ***1. 關閉 VM 實例（VM Instances）***

在建立映像檔前，需要關閉 VM 主機才能夠建立映像檔

勾選需要建立映像檔案的 VM Instances，然後點選 `停止` 終止運行

![關閉 VM 實例（VM Instances）](./images/google-compute-engine-vm-images-stop-instance.png)


### ***2. 建立映像檔***

點選選單 `映像檔（Images）` 頁面並建立映像檔

輸入 `映像檔名稱`，並選擇映像檔的 `來源磁碟`，這邊的來源磁碟為剛剛關閉的 `VM 實例（VM Instances）`

完成後點選 `建立` 即可建立映像檔

![建立映像檔](./images/google-compute-engine-vm-images-create.png)

### ***3. 建立映像檔完成***

建立映像檔完成後，即可在 `映像檔（Images）` 頁面中看到剛剛建立的映像檔

![建立映像檔完成](./images/google-compute-engine-vm-images-create-finish.png)
