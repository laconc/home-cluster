# home-cluster

## ansible

This directory contains a set of Ansible files to provision fresh Raspberry Pis and set them up as a Kubernetes cluster.

```bash
pushd ansible
ansible-playbook playbook.yml --verbose
popd
```

## helmfile

Once the cluster is up, we'll install the in-cluster applications using Helm. We're using Helmfile to simplify the deployment.

```bash
pushd helmfile
helmfile sync
popd
```
