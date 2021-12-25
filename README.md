# Kubernetes + Docker For Mac Setup

## NGINX Ingress Controller

```shell
$ helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
``` 

### Create test (dummy) services

```shell
$ kubectl create namespace hello-world && \
    kubectl apply -n hello-world \
      -f dummies/hello1.yaml \
      -f dummies/hello2.yaml
$ kubectl apply -n hello-world -f dummies/hello_ingress.yaml
$ sudo echo '127.0.0.1 hw1.localhost hw2.localhost' >> /etc/hosts
$ curl hw1.localhost
```

---

## cert-manager

```shell
$ helm repo add jetstack https://charts.jetstack.io
$ helm repo update
$ helm upgrade --install cert-manager jetstack/cert-manager \
    --namespace cert-manager \
    --create-namespace \
    --version v1.6.1 \
    --set installCRDs=true
```

### Setup trusted root certificate

```shell
$ mkcert -install
```

### Install CA Issuer

```shell
$ CAROOT_DIR="$(mkcert -CAROOT)" && \
    kubectl -n cert-manager create secret tls localhost-ca-tls-secret --key="${CAROOT_DIR}/rootCA-key.pem" --cert="${CAROOT_DIR}/rootCA.pem" 
$ kubectl create -f cert-manager/localhost_issuer.yaml
```

### Test (dummy) services ingress with TLS

```shell
$ kubectl apply -n hello-world -f dummies/hello_ingress_tls.yaml
```

Wait and visit [https://hw1.localhost](https://hw1.localhost)

## Restart

To delete everything and start fresh, try running:

```
$ kubectl delete ns cert-manager hello-world ingress-nginx --ignore-not-found=true
$ kubectl delete -f cert-manager/localhost_issuer.yaml --ignore-not-found=true
```

- https://itnext.io/deploying-tls-certificates-for-local-development-and-production-using-kubernetes-cert-manager-9ab46abdd569
- https://reposhub.com/linux/system-utilities/unfor19-kubernetes-localdev.html
- https://github.com/jnewland/local-dev-with-docker-for-mac-kubernetes
- https://github.com/cheslijones/tls-minikube#-adding-tls-certificate
