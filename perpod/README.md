# 手順

# 環境構築
## 1. Name Spaceの作成
```
kubectl apply -f ns-perpod.yaml
```
## 2. Service の作成
ap-perpod.yaml内、syslog serverのIPアドレスを[Ingress 手順1.](https://github.com/hiropo20/nginx-nap-container-deployment-sample/tree/master/ingress#1-syslog-server%E3%81%AEdeploy)で確認したServiceのIPアドレスに変更ください
```
app_protect_security_log "/etc/nginx/custom_log_format.json" syslog:server=127.0.0.1:514;
```
ap-perservice.yaml内、予め作成したContainer Imageを以下image欄に指定してください
```yaml
      - name: perpod-nap
        image: nginx-plus-nap:latest 
        ports:
        - containerPort: 8080
```
Serviceの作成
```
kubectl apply -f ap-perpod.yaml -n perpod
```
## 3. Virtual Server Resourceの作成
```
kubectl apply -f cafe-virtual-server.yaml -n perpod
```


# 動作確認
## サンプルリクエストの実行
```
curl -H "Host: perpod-cafe.example.com" https://**宛先IPアドレス**/coffee --insecure
curl -H "Host: perpod-cafe.example.com" "https://**宛先IPアドレス**/coffee?<script>" --insecure
curl -H "Host: perpod-cafe.example.com" https://**宛先IPアドレス**/tea --insecure
curl -H "Host: perpod-cafe.example.com" "https://**宛先IPアドレス**/tea?<script>" --insecure
```
## Security Logの確認
```
kubectl exec -it **SYSLOG_POD** -- cat /var/log/messages
```

# TIPS
## コマンド
```
kubectl get pods -n perpod
kubectl logs **Per-Pod POD名** **Container名** -n perpod
kubectl exec -c **Container名** -it **Per-Pod POD名** bash -n perpod
```
