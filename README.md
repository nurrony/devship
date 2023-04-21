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

[k3d-site]: https://k3d.io
[helm-site]: https://helm.sh/docs/intro/install/
[k8s-cli]: https://kubernetes.io/docs/tasks/tools/
[docker-desktop]: https://www.docker.com/products/docker-desktop/
