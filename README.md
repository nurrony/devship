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
 - Host directory mounted as `PersistentVolume`
 - Local Registry **[Optional]**
 - Ingress Nginx with custom certificates **[Optional]**

 More coming soon...

## Sail your K8s Devship

// TBD

[k3d-site]: https://k3d.io
[helm-site]: https://helm.sh/docs/intro/install/
[k8s-cli]: https://kubernetes.io/docs/tasks/tools/
[docker-desktop]: https://www.docker.com/products/docker-desktop/
