# home-cluster

The tooling required to spin up a home network running in Kubernetes. This is a bare metal deployment to some Raspberry Pis. The services are all available through HTTPS and only from inside the network.

## Core Components

## External Dependencies

* Cloudflare manages the DNS records for my domain. In conjunction with `cert-manager`, this allows me to generate Let's Encrypt TLS certificates and perform DNS domain validation for free and automatically.
* A router that can be configured to use BGP, such as the Ubiquiti UDM-SE. ([Instructions.](ROUTER.md))

## Ansible

This directory contains a set of Ansible files to provision fresh Raspberry Pis and set them up as a Kubernetes cluster.

```bash
pushd ansible
ansible-playbook playbook.yml --verbose
popd
```

## Manual Changes Needed for Prometheus

* Update the metrics bind address for kube-proxy
```bash
$ kubectl -n kube-system edit cm kube-proxy

# Update `metricsBindAddress: 0.0.0.0:10249`

$ kubectl -n kube-system rollout restart ds/kube-proxy
```
* SSH into the control node and update the following
```bash
$ sudo sed -i 's/--bind-address=127.0.0.1/--bind-address=0.0.0.0/' /etc/kubernetes/manifests/kube-controller-manager.yaml

$ sudo sed -i 's/--bind-address=127.0.0.1/--bind-address=0.0.0.0/' /etc/kubernetes/manifests/kube-scheduler.yaml

$ control_node_ip=10.4.0.101
$ sudo sed -i "s#--listen-metrics-urls=.*#--listen-metrics-urls=http://127.0.0.1:2381,http://$control_node_ip:2381#" /etc/kubernetes/manifests/etcd.yaml
```

## Helm

Once the cluster is up, we'll install the in-cluster apps with Helm. We're using Helmfile to simplify the deployment.

```bash
pushd helm
helmfile sync
popd
```

## Available Routes

* `adguard.64f.dev`
* `alertmanager.64f.dev`
* `grafana.64f.dev`
* `hubble.64f.dev`
* `longhorn.64f.dev`
* `prometheus.64f.dev`

## Notes

* At [https://github.com/laconc/pdns](https://github.com/laconc/pdns), we're building custom images for the PowerDNS services. The images are being built for ARM64 and the webserver password has been removed to simplify scraping metrics with Prometheus. We've also fixed an issue preventing the `auth` image from building successfully.
