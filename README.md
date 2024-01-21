
# Setup of Kubernetes Cluster with Different Kubernetes Distributions

This document provides the commands for setting up a Kubernetes (K8s) cluster using different K8s distributions.

## 1. Setting Up a K3s Cluster with integrated ETCD

### Master Node Setup

To initialize the master node in your K3s cluster, execute the following command:

```
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="--node-ip=MASTER_IP_ADD --flannel-iface=NETWORK_INTERFACE" sh -
```

### Worker Node Setup

To add a worker node to your K3s cluster, use the following command:

```
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="--node-ip=WORKER_IP_ADD --flannel-iface=NETWORK_INTERFACE" K3S_URL=https://MASTER_IP_ADD:6443 K3S_TOKEN="TOKEN" sh -
```
_Note: The value to use for `K3S_TOKEN` is stored at `/var/lib/rancher/k3s/server/node-token`._