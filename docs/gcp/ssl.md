# SSL
記錄 gcp ssl 設定

## ssl 理論相關
> [ssl explained](https://www.youtube.com/watch?v=r1nJT63BFQ0)

## 資安相關
> [ssl pinning](https://ithelp.ithome.com.tw/articles/10252867?sc=hot)
> [ssl stripping](https://www.youtube.com/watch?v=99YNg8UAesI)

## 先找機器申請非對稱的 key
> 有兩種方式，一種是購買憑證有表格會幫忙處理，下列示範如果本機端發起請求
```
openssl req -new -newkey rsa:2048 -nodes -keyout server.key -out server.csr
```
以下為需填寫問題
- `.crs` : 此為發出憑證請求的檔案  
- `.key` : 這是 key 一定要保留

## 驗證 domain 擁有權 & 上傳 crs 檔案
> 每家第三方不同，常見的為，下載 ssl 廠商所給的指定檔案上傳到需申請的 domain 提供下載

## 驗證完成後會有檔案提供下載
> 此時需要將自身的 `SSL Certificate Chain` 與 CA 核發的證明解密檔案 `crt` 放在同一個檔案後即可使用，可參考下面實作方式  
> [gcp-converting_private_keys_and_concatenating_ssl_certificates](https://cloud.google.com/appengine/docs/standard/python/securing-custom-domains-with-ssl#converting_private_keys_and_concatenating_ssl_certificates)

```
cat [MY_DOMAIN_CERT].crt [MY_SecureServerCA].crt [MY_TrustCA].crt [MY_TrustExternalCARoot].crt > [MY_CONCAT_CERT].crt
```

- `.pem`：此檔案為 crs 與 CA 機構的 Key 所簽發的 `SSL Certificate Chain`
- `.crt`：通常會有若干 crt 檔案，通常與 `pem` 名稱相同的為 `SSL Certificate Chain` 其餘的為 CA 機構所核發的證明解密檔案

## ssl 設定，看情境，以下提供 app Engine 以及 load balancer 的設定教學
- [load balancer](https://cloud.google.com/load-balancing/docs/ssl-certificates/self-managed-certs)
- [app engine](https://tw.godaddy.com/help/manually-install-an-ssl-certificate-on-my-google-app-engine-32074)

## 測試工具
- [Certificate Checker](https://ssltools.godaddy.com/views/certChecker)

## 如果需要做 ssl pinning android 與 ios 分別需要不同的檔案
1. 哈哈哈
2. openssl x509 -outform der -in file_name.pem -out file_name.der
> [apple ssl pinning](https://iter01.com/72987.html)






