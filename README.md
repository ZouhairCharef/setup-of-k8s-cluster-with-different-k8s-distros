
# Setup of Kubernetes Cluster with Different Kubernetes Distributions

This document provides the commands for setting up a Kubernetes (K8s) cluster using different K8s distributions.

## 1. Setting Up a K3s Cluster with an integrated datastore using ETCD as a pod

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


## 3. Setting Up a Kubernetes Cluster with RKE (Rancher Kubernetes Engine)

### Download RKE Binary
1. Download the latest release from the [Rancher RKE Releases - Github](https://github.com/rancher/rke/releases).

### Set up Passwordless SSH Logins on All Nodes
- We will use SSH Keys for root account login on all the Kubernetes nodes.
- Create an ssh keypair on the host machine:
  ```
  ssh-keygen -t ed25519
  ```
- Copy SSH Keys to all the Kubernetes nodes (password: kubeadmin):
  ```
  ssh-copy-id root@172.16.16.101
  ssh-copy-id root@172.16.16.102
  ssh-copy-id root@172.16.16.103
  ```

### Prepare the Kubernetes Nodes (node1, node2, node3)
1. Disable Firewall:
   ```
   ufw disable
   ```
2. Disable swap:
   ```
   swapoff -a; sed -i '/swap/d' /etc/fstab
   ```
3. Update sysctl settings for Kubernetes networking:
   ```
   cat >>/etc/sysctl.d/kubernetes.conf<<EOF
   net.bridge.bridge-nf-call-ip6tables = 1
   net.bridge.bridge-nf-call-iptables = 1
   EOF
   sysctl --system
   ```

### Install Docker Engine
Execute the following commands to install Docker:
```
{
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu   $(. /etc/os-release && echo "$VERSION_CODENAME") stable" |   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update -y
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
}
```

### Bring Up Kubernetes Cluster
1. Create cluster configuration:
   ```
   rke config
   ```
   - Follow the interactive cluster configuration to end up with a `cluster.yml` file in the current directory.
2. Provision the cluster:
   ```
   rke up
   ```
   - After completion, you will have a cluster state file (`cluster.rkestate`) and kube config file (`kube_config_cluster.yml`) in the current directory.
