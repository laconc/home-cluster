# home-cluster

## Assumptions

The k8s cluster nodes have 10.4.0.0/24 addresses, and the user is connecting from 10.1.0.0/24.

## ansible

This directory contains a set of Ansible files to provision fresh Raspberry Pis and set them up as a Kubernetes cluster.

```bash
pushd ansible
ansible-playbook playbook.yml --verbose
popd
```

## helmfile

Once the cluster is up, we'll install the in-cluster apps with Helm. We're using Helmfile to simplify the deployment.

```bash
pushd helm
helmfile sync
popd
```

_Note: It might be best to install Calico first and reboot all the nodes, and then install the rest._
