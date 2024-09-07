# vagrant-k8s

Create a local kubernetes cluster in Vagrant. Created and used for learning kubernetes, as support for CKAD, CKA, and CKS preparation.

The `vagrant` user in the master node is configured to access the cluster. You can use `kubectl` to interact with the cluster, or `helm` to manage charts.

## Requirements

- Ubuntu Desktop 24.04 (probably works on any Linux)
- VirtualBox 7.0
- Vagrant 2.4
- Ansible 2.16

## Starting the cluster

Clone the repo and run `vagrant up`:

```
sergiu@nuc:~/git-repos/vagrant-k8s$ vagrant up
Bringing machine 'm01' up with 'virtualbox' provider...
Bringing machine 'w01' up with 'virtualbox' provider...
==> m01: Importing base box 'ubuntu/jammy64'...
==> m01: Matching MAC address for NAT networking...

...

PLAY RECAP *********************************************************************
w01                        : ok=29   changed=22   unreachable=0    failed=0    skipped=4    rescued=0    ignored=0   

sergiu@nuc:~/git-repos/vagrant-k8s$
```

## Connect to VMs

Use `vagrant ssh <node>` to connect to one of the cluster nodes:

```
sergiu@nuc:~/git-repos/vagrant-k8s$ vagrant ssh m01

Last login: Sat Sep  7 19:11:40 2024 from 10.0.2.2
sergiu@nuc:~/git-repos/vagrant-k8s$
```

## Customize setup

By default the cluster is composed by one master/control-plane and one worker/data-plane node.

If you need multiple workers, edit the `Vagrantfile` file and add a new line in the `vms` array. Make sure to have unique hostname and IP addresses, and use the `worker` role.

> For now multiple masters cluster is not supported.

The provisioning of the cluster is achieved using `ansible`. Feel free to update the roles in the `./roles/` folder to correspond to your needs.

Check the `./vars/main.yaml` file and customize if you need different versions, or some extra packages installed.

## Shared folder

The local `shared-folder` is shared with all the cluster nodes, mounted in the `/mnt/shared-folder` path. You can use it to share files (e.g.) between your host and cluster nodes.
