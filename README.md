```
$ helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
  
$ kubectl create namespace hello-world
$ kubectl apply --namespace hello-world -f dummies/hello1.yaml
$ kubectl apply --namespace hello-world -f dummies/hello2.yaml
$ kubectl apply --namespace hello-world -f dummies/hello_ingress.yaml

$ helm repo add jetstack https://charts.jetstack.io
$ helm repo update
$ helm upgrade --install cert-manager jetstack/cert-manager \
    --namespace cert-manager \
    --create-namespace \
    --version v1.6.1 \
    --set installCRDs=true


$ kubectl create -f cert-manager/staging_issuer.yaml
$ kubectl create -f cert-manager/prod_issuer.yaml

$ kubectl apply --namespace hello-world -f dummies/hello_ingress_tls.yaml
$ curl hw1.localdev.me
$ curl https://hw1.localdev.me
```


- https://reposhub.com/linux/system-utilities/unfor19-kubernetes-localdev.html
- https://github.com/jnewland/local-dev-with-docker-for-mac-kubernetes
