# Kubernetes Multi-Cluster Dev Setup

This repository contains my local Kubernetes dev environment powered by [k3d from Rancher][k3d-site]

## Prerequisites

1. [Kubernetes CLI][k8s-cli]
2. [Docker Desktop][docker-desktop]
3. [k3d][k3d-site]
4. [Helm][helm-site]

> ℹ️ **The script will install necessary missing softwares**

## Features
 - Kubernetes cluster with multi Control Plane (Server) and multi Agents (Workers)
 - Host directory mounted as `PersistentVolume` called `devship-pv`
 - Seamless Local Registry Integration **[Optional]**
 - Ingress Nginx with custom certificates **[Optional]**

 More coming soon...

## Generate certificates and HostMapping
While creating cluster the script search for `{CLUSTER_DOMAIN}-key.pem` and `{CLUSTER_DOMAIN}.pem` as key and certificate file during setting up `tls` secret for `Nginx Ingress Controller`. You need to create SSL key and certificate for your domain and put it into certs directory following the naming pattern.

## Start Voyage on Kubernetes Devship
Run the following command to start the voyage. The script will guide you to setup your cluster

```bash
$ ./k3dcluster
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
4. Deploy a Pod referencing the image above as follows
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

tbd

[k3d-site]: https://k3d.io
[helm-site]: https://helm.sh/docs/intro/install/
[k8s-cli]: https://kubernetes.io/docs/tasks/tools/
[docker-desktop]: https://www.docker.com/products/docker-desktop/
