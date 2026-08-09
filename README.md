# Infrastructure Lab (Vagrant + k3s)

This repository provisions a single Ubuntu VM with [k3s](https://k3s.io/) using Vagrant.

## What this setup creates

- VM box: `bento/ubuntu-24.04`
- Hostname: `k3s-server`
- Provider: VirtualBox
- Resources: 4 CPU, 8192 MB RAM
- Private IP: `192.168.56.10`
- Kubernetes: k3s installed via the official install script

During provisioning, kubeconfig is prepared at:

- `/home/vagrant/.kube/config`

It is also updated to use `192.168.56.10` instead of localhost so tools on the VM can reach the cluster API correctly.

## Prerequisites

Install these on your host machine:

- [Vagrant](https://developer.hashicorp.com/vagrant/downloads)
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)

Optional but useful on the host:

- `kubectl`

## Quick start

From this folder:

```bash
vagrant up
```

This will:

1. Create the VM
2. Install dependencies (`curl`, `git`)
3. Install k3s (if not already installed)
4. Configure kubeconfig for the `vagrant` user

## Access the VM

```bash
vagrant ssh
```

## Verify the cluster (inside VM)

k3s includes `kubectl`:

```bash
sudo k3s kubectl get nodes -o wide
sudo k3s kubectl get pods -A
```

Or if your shell is using kubeconfig:

```bash
kubectl get nodes
```

## Common commands

Start or recreate environment:

```bash
vagrant up
```

Stop VM:

```bash
vagrant halt
```

Restart VM:

```bash
vagrant reload
```

Destroy VM completely:

```bash
vagrant destroy -f
```

Re-run provisioning:

```bash
vagrant provision
```

## Notes

- This is a single-node lab environment intended for local development/testing.
- If provisioning fails due to network/package issues, run `vagrant provision` again.
- VM state is stored under `.vagrant/`.
