# 手順

# 環境構築
## 1. Name Spaceの作成
```
kubectl apply -f ns-perservice.yaml
```
## 2. Cafe Applicationの作成
```
kubectl apply -f cafe.yaml -n perservice
```
## 2. Service の作成
ap-perservice.yaml内、syslog serverのIPアドレスを[Ingress 手順1.](https://github.com/hiropo20/nginx-nap-container-deployment-sample/tree/master/ingress#1-syslog-server%E3%81%AEdeploy)で確認したServiceのIPアドレスに変更ください
```
app_protect_security_log "/etc/nginx/custom_log_format.json" syslog:server=127.0.0.1:514;
```
ap-perservice.yaml内、imageを予め作成したContainer Imageを指定してください
```yaml
    spec:
      containers:
      - name: perservice-nap
        image: nginx-plus-nap:latest
        ports:
        - containerPort: 80
```
Serviceの作成
```
kubectl apply -f ap-perservice.yaml -n perservice
```
## 3. Virtual Server Resourceの作成
```
kubectl apply -f cafe-virtual-server.yaml -n perservice
```


# 動作確認
## サンプルリクエストの実行
```
curl -H "Host: perservice-cafe.example.com" https://**宛先IPアドレス**/coffee --insecure
curl -H "Host: perservice-cafe.example.com" "https://**宛先IPアドレス**/coffee?<script>" --insecure
curl -H "Host: perservice-cafe.example.com" https://**宛先IPアドレス**/tea --insecure
curl -H "Host: perservice-cafe.example.com" "https://**宛先IPアドレス**/tea?<script>" --insecure
```
## Security Logの確認
```
kubectl exec -it **SYSLOG_POD** -- cat /var/log/messages
```

```
