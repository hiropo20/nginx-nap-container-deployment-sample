# このページの目的
NGINX App Protect(以下NAP)のKubernetes環境における Ingress , Per-Service Proxy , Per-Pod Proxyのデプロイサンプルを示す
<br><img src="https://www.nginx.com/wp-content/uploads/2020/09/app-services-Kubernetes-pt2_four-locations.png" width="600"><br>

## フォルダ
- ingress：Ingressのサンプル設定
- nginx-nap-dockerfile：Perservice、PerpodでNAPを利用する際のサンプルDocker ImageのBuild関連ファイル
- perservice：Per-Service Proxyとして利用する際のサンプル設定
- perpod：Per-Pod Proxyとして利用する際のサンプル設定

# 作業手順
## 1. 事前作業
1. ライセンスファイルの取得   
Trialまたはご契約Subscriptionのライセンスファイルを取得ください   
NGINX PlusおよびNGINX App Protectの利用が可能なライセンスファイルをご準備ください
1. Ingress Controller Imageの作成   
[NGINX Doc:Building the Ingress Controller Image](https://docs.nginx.com/nginx-ingress-controller/installation/building-ingress-controller-image/)   
MakeFileのTargetとして以下を選択ください   
`debian-image-nap-plus: for building a debian-based image with NGINX Plus and the appprotect module.`
1. Ingress Controller のデプロイ   
[NGINX Doc:Installation with Manifests](https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-manifests/)

## 2. NGINX App Protect Docker Imanageを作成する
[NGINX App Protect Dockerfile](https://github.com/hiropo20/nginx-nap-container-deployment-sample/tree/master/nginx-nap-dockerfile)
## 3. IngressでNAPを利用する
[Ingress](https://github.com/hiropo20/nginx-nap-container-deployment-sample/tree/master/ingress)
## 4. Per-Service ProxyでNAPを利用する
[Per-Service Proxy](https://github.com/hiropo20/nginx-nap-container-deployment-sample/tree/master/perservice)
## 5. Per-Pod ProxyでNAPを利用する
[Per-Pod Proxy](https://github.com/hiropo20/nginx-nap-container-deployment-sample/tree/master/perpod)
