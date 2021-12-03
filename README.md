# Kubernetes + Docker For Mac Setup

## NGINX Ingress Controller

```
$ helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
``` 

### Create test (dummy) services

```  
$ kubectl create namespace hello-world
$ kubectl apply --namespace hello-world -f dummies/hello1.yaml
$ kubectl apply --namespace hello-world -f dummies/hello2.yaml
$ kubectl apply --namespace hello-world -f dummies/hello_ingress.yaml
```

---

## cert-manager 

```
$ helm repo add jetstack https://charts.jetstack.io
$ helm repo update
$ helm upgrade --install cert-manager jetstack/cert-manager \
    --namespace cert-manager \
    --create-namespace \
    --version v1.6.1 \
    --set installCRDs=true
```

### Setup trusted root certificate

```
$ mkcert -install
```

### Install CA Issuer

```
$ CAROOT_DIR="$(mkcert -CAROOT)" && \
    kubectl -n cert-manager create secret tls localhost-ca-tls-secret --key="${CAROOT_DIR}/rootCA-key.pem" --cert="${CAROOT_DIR}/rootCA.pem" 
$ kubectl create -f cert-manager/localhost_issuer.yaml
```

### Test (dummy) services ingress with TLS

```
$ kubectl apply --namespace hello-world -f dummies/hello_ingress_tls.yaml
$ curl hw1.localdev.me
$ curl https://hw1.localdev.me
```

- https://itnext.io/deploying-tls-certificates-for-local-development-and-production-using-kubernetes-cert-manager-9ab46abdd569
- https://reposhub.com/linux/system-utilities/unfor19-kubernetes-localdev.html
- https://github.com/jnewland/local-dev-with-docker-for-mac-kubernetes
- https://github.com/cheslijones/tls-minikube#-adding-tls-certificate
