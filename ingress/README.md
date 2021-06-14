# 手順
最新の情報、手順は以下を参照してください   
[nginxinc/kubernetes-ingress/examples/appprotect](https://github.com/nginxinc/kubernetes-ingress/tree/master/examples/appprotect)

## 1. Syslog ServerのDeploy
```
kubectl create -f syslog.yaml
```

Service IPを確認してください
このSyslog ServerをIngress , Per-Service NAP , Per-Pod NAPで利用します
```
$ kubectl get svc -o wide  syslog-svc
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE     SELECTOR
syslog-svc   ClusterIP   **Service IP**  <none>        514/TCP   5h48m   app=syslog

$ kubectl describe svc syslog-svc
Name:              syslog-svc
Namespace:         default
Labels:            <none>
Annotations:       cloud.google.com/neg: {"ingress":true}
Selector:          app=syslog
Type:              ClusterIP
IP Families:       <none>
IP:                **Service IP**
IPs:               <none>
Port:              <unset>  514/TCP
TargetPort:        514/TCP
Endpoints:         **pod IP**:514
Session Affinity:  None
Events:            <none>
```

## 2. Cafe ApplicationのDeploy
```
kubectl create -f cafe.yaml -n nginx-ingress
```

## 3. SSL証明書・鍵の作成
```
kubectl create -f cafe-secret.yaml
```

## 4. NGINX App Protect設定の設定

```
kubectl create -f ap-dataguard-alarm-policy.yaml
kubectl create -f ap-logconf.yaml
kubectl create -f ap-apple-uds.yaml
```

### 5. Ingress Resourceの作成
cafe-ingress.yaml内、syslog serverのIPアドレスを[手順1.](https://github.com/hiropo20/nginx-nap-container-deployment-sample/tree/master/ingress#1-syslog-server%E3%81%AEdeploy)で確認したServiceのIPアドレスに変更ください
```yaml
appprotect.f5.com/app-protect-security-log-destination: "syslog:server=127.0.0.1:514"
```
Ingress Resouceの作成
```
kubectl create -f cafe-ingress.yaml
```
## 
