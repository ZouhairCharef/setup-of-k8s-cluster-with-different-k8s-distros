
# Setup of Kubernetes Cluster with Different Kubernetes Distributions

This document provides the commands for setting up a Kubernetes (K8s) cluster using different K8s distributions.

## 1. Setting Up a K3s Cluster with an integrated datastore using ETCD as pod

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


## 2. Setting Up a K3s Cluster with an external datastore using MySQL

For setting up a K3s cluster with an external ETCD using MySQL, follow the steps below:

### Master Node Setup with External ETCD (MySQL)

To initialize the master node in your K3s cluster with an external ETCD using MySQL, execute the following command:

```
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="--node-ip=MASTER_IP_ADD --flannel-iface=NETWORK_INTERFACE" sh -s - server --datastore-endpoint="mysql://USER:PASSWORD@tcp(MYSQL_IP:3306)/K8S_DATABASE"
```
_Note: Replace `USER`, `PASSWORD`, `MYSQL_IP`, and `K8S_DATABASE` with your MySQL user credentials, IP, and database name respectively._
