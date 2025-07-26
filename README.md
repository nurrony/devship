# Kubernetes Multi-Cluster Dev Setup

This repository contains my local Kubernetes dev environment powered by [k3d from Rancher][k3d-site]

## Prerequisites

1. [Kubernetes CLI][k8s-cli]
2. [Docker Desktop][docker-desktop]
3. [k3d][k3d-site]
4. [Helm][helm-site]

> ℹ️ **The script will install all required missing softwares**

## Features
 - Kubernetes cluster with multi Control Plane (Server) and multi Agents (Workers)
 - Host directory mounted as `PersistentVolume` called `<cluster-name>-pv` (Opt-In)
 - Use existing network for cluster
 - Seamless Local Registry Integration
 - Ability to install TLS for services that ensures E2E Secure connection even for local environment
 - Use `podman` as container runtime (experimental)

 More coming soon...

## Generate certificates and HostMapping
While creating cluster the script search for `{CLUSTER_DOMAIN}.key` and `{CLUSTER_DOMAIN}.crt` as key and certificate file during setting up `tls` secret. You need to create SSL key and certificate for your domain and put it into `certs` directory following the naming pattern.

## Start Voyage on Kubernetes Devship
Run the following command to start the voyage. The script will guide you to setup your cluster

```bash
$ ./devship
```

## Using local registry
To use the local registry, please follow the instructions below

1. Check if registry contrainer is up and running
```bash
$ docker ps | grep "registry"
```
2. Pull `nginx:alpine` image
```bash
$ docker pull nginx:alpine
```
3. Tag `nginx:alpine` as follows and push it
```bash
 $ docker tag nginx:alpine <registry-container-name>:5000/nginx:alpine
 $ docker push <registry-container-name>:5000/nginx:alpine
```
4. Deploy a `Pod` referencing the image pushed into the local registry in the step above as follows
```bash
cat <<EOF | kubectl apply -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-test-registry
  labels:
    app: nginx-test-registry
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-test-registry
  template:
    metadata:
      labels:
        app: nginx-test-registry
    spec:
      containers:
      - name: nginx-test-registry
        image: <registry-container-name>:5000/nginx:alpine
        ports:
        - containerPort: 80
EOF
```
6. Check if the pod is running fine or not
```sh
$ kubectl get pods -l "app=nginx-test-registry"
```

## Using Ingress with TLS

The script prompt you to setup [Nginx Ingress Controller][nginx-ingress] using [Bitnami Nginx Ingress Helm Chart][bitnami-nginx-ingress-chart] with the `TLS` support.

If you did not setup ingress during creation of the cluster, you can install it following the steps described in [Setup Nginx Ingress Controller](/examples/ingress/README.md)

## Todo
- [ ] Add Docker Registry UI
- [ ] Add [Kube-VIP](https://kube-vip.io/) support


[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
[bitnami-nginx-ingress-chart]: https://github.com/bitnami/charts/tree/main/bitnami/nginx-ingress-controller/#nginx-ingress-controller-packaged-by-bitnami
[traefik]: https://traefik.io/
[k3d-site]: https://k3d.io
[helm-site]: https://helm.sh/docs/intro/install/
[k8s-cli]: https://kubernetes.io/docs/tasks/tools/
[docker-desktop]: https://www.docker.com/products/docker-desktop/
