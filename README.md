# Ansible-Kubernetes

A collection of Ansible roles to deploy a baremetal kubernetes cluster (and optionally set up a local container registry)

NOTE: These roles are currently developed for Debian distributions

## Prerequisites

First you obviously need to install Ansible:
```
sudo apt-get install ansible
```

### Creating an inventory

You will need at least 2 groups, each of them containing at least 1 machine. Example:

```
[control]
control-1 ansible_host=192.168.0.168 ansible_user=root

[worker]
worker-1 ansible_host=192.168.0.140 ansible_user=root
```

## Initialize a new cluster

Before initializing the cluster you will need a user with sudo access on all hosts.

If you already have a user set up, you can skip this step.

Otherwise you can use `setup-users` role:

```
ansible -i inventory setup-users
```

Now, to create a new cluster the first role you'll need is `install-dependencies`.

This role installs `docker`, `containerd`, `kubeadm` and other dependencies on all hosts

```
ansible -i inventory install-dependencies
```

After that we have to disable swap on all hosts `due to the inherent difficulty in guaranteeing and accounting for pod memory utilization when swap memory was involved` (although its currently [supported](https://kubernetes.io/blog/2023/08/24/swap-linux-beta/) in beta mode in 1.28):

```
ansible -i inventory disable-swap
```

Finally we initialize the cluster on control plane:

```
ansible -i inventory initialize-cluster
```

And connect worker nodes to the cluster:

```
ansible -i inventory join-cluster
```
