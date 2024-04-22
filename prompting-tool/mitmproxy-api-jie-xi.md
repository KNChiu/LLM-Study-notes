---
description: API 傳輸資料攔截解析
---

# mitmproxy API 解析

## 下載安裝

下載連結 : [https://mitmproxy.org/](https://mitmproxy.org/)

參考文檔 : [https://docs.mitmproxy.org/stable/](https://docs.mitmproxy.org/stable/)



## 環境設定

### Proxy 設定

#### 尋找 Proxy

<figure><img src="../.gitbook/assets/image (12).png" alt="" width="341"><figcaption><p>Proxy 設定</p></figcaption></figure>

#### 編輯 Proxy 伺服器

設定 Proxy IP 位址 : `127.0.0.1:8080`

<figure><img src="../.gitbook/assets/image (13).png" alt="" width="271"><figcaption></figcaption></figure>

### mitmweb&#x20;

啟動 `mitmweb`後在網址列輸入 [http://127.0.0.1:8081/](http://127.0.0.1:8081/) 開啟監聽視窗

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

### Pyrhon 設定

設定憑證與 Proxy

```python
cert_file = "C:/Users/{UserName}/.mitmproxy/mitmproxy-ca-cert.pem" 
os.environ['REQUESTS_CA_BUNDLE'] = cert_file
os.environ['SSL_CERT_FILE'] = cert_file
os.environ['HTTPS_PROXY'] = 'http://127.0.0.1:8080'
```

