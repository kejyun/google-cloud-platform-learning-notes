# 負載平衡（Load Balancer） http 301 轉址至 https

通常我們會使用 Nginx 的 `schema` 是否為 `http` 去判斷是否需要做 301 轉址到 `https`，Nginx 就會像下方設定

```
server {
    if ($scheme = http) {
        return 301 https://$server_name$request_uri;
    }
}
```

但使用 GCP 的 `負載平衡（Load Balancer）` 時，當 `https` 的請求進來時， GCP 轉送封包到 `負載平衡後端服務` 時，會使用 `http` 去做轉送，導致請求產生無窮迴圈的狀況


> `https (Load Balancer)` => `http (Nginx)` => `301 轉址 https 至 Load Balancer` （無窮迴圈）

目前 (2018-11-20) 這個狀況還沒解決，在 [Google Cloud Platform Feedback](https://googlecloudplatform.uservoice.com/forums/302616-load-balancing/suggestions/31951531-allow-http-to-redirect-to-https-automatically) 被討論中

為了解決這個問題，可以改用 `$http_x_forwarded_proto` 去判斷轉送過來的封包是否為 `http` 再去做 301 轉址

```
server {
        listen 80 default_server;
        listen [::]:80 default_server ipv6only=on;

        listen 443 ssl default_server;
        listen [::]:443 ssl default_server;

        if ($http_x_forwarded_proto = "http") {
           return 301 https://$host$request_uri;
        }

        ssl_certificate /etc/nginx/ssl/nginx.crt;
        ssl_certificate_key /etc/nginx/ssl/nginx.key;
}
```



## 參考資料
* [google cloud platform - How can you redirect HTTP to HTTPS (GCP Load Balancing)? - Server Fault](https://serverfault.com/questions/862725/how-can-you-redirect-http-to-https-gcp-load-balancing/866912)
* [Allow HTTP to redirect to HTTPS automatically – Google Cloud Platform Feedback](https://googlecloudplatform.uservoice.com/forums/302616-load-balancing/suggestions/31951531-allow-http-to-redirect-to-https-automatically)
