# Setup Nginx Ingress using Bitnami Helm Chart

This document describes how to manually install [Nginx Ingress][nginx-ingress] using [Bitnami Nginx Ingress Helm Chart][bitnami-nginx-ingress-chart] with TLS support.

## Add Helm Repo

```
$> helm repo add bitnami https://charts.bitnami.com/bitnami
$> helm repo update
$> kubectl create namespace ingress
```

The commands above creates a namespace for `ingress` and add Bitname Helm Repository 

## Create Certificates

You need to create certificates for the domain you want to access the services for your cluster. I use `mkcert` to generate this signed certificates and install CA root in my computer. In this case the certificate matches `*.devship.localhost` domains. See [mkcert](https://mkcert.dev/) doc for more info.

> ℹ️ I use `dnsmasq` to resolve all domains for `*.localhost` to `127.0.0.1` Please check [dnsmasq documentation](https://thekelleys.org.uk/dnsmasq/doc.html) to setup and configure it for your OS

## Create TLS Secrets

Execute the following command to create TLS secret in your cluster under `ingress` namespace

```sh
kubectl create secret tls nginx-server-certs --key certs/devship.localhost-key.pem --cert certs/devship.localhost.pem --namespace ingress
```



[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
[bitnami-nginx-ingress-chart]: https://github.com/bitnami/charts/tree/main/bitnami/
