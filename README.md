# Ansible-Kubernetees

A collection of Ansible roles to deploy a baremetal kubernetes cluster (and optionally set up a local container registry)

## Prerequisites

First you obviously need to install Ansible:
```
sudo apt-get install ansible
```

### Creating an inventory

You will need at least 2 groups containing at least 1 machine. Example:

```
[control]
control-1 ansible_host=192.168.0.168 ansible_user=root

[worker]
worker-1 ansible_host=192.168.0.140 ansible_user=root
```

## Usage

