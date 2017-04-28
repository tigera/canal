# Kubernetes self-hosted install

Installation is simple, for Kubernetes 1.5:

```
kubectl apply -f https://raw.githubusercontent.com/projectcalico/canal/master/k8s-install/canal.yaml
```

For Kubernetes 1.6:

```
kubectl apply -f https://raw.githubusercontent.com/projectcalico/canal/master/k8s-install/1.6/rbac.yaml

kubectl apply -f https://raw.githubusercontent.com/projectcalico/canal/master/k8s-install/1.6/canal.yaml
```

This directory includes two manifests for deploying canal on Kubernetes - one that requires its own etcd and one
that doesn't.

Requirements:
- Make sure your k8s cluster is configured to provide serviceaccount tokens to pods.
- Make sure your kubelets have been started with `--network-plugin=cni` and
  have `--cni-conf-dir` and `--cni-bin-dir` properly set
- Make sure your controller manager has been started with `--cluster-cidr=10.244.0.0/16` and `--allocate-node-cidrs=true`.

## Configuration

### Kubernetes

Canal uses the Kubernetes API to enforce policy, and so needs to authenticate with the Kubernetes API.  The provided
ConfigMap [configures the Calico CNI plugin](https://github.com/projectcalico/calico-cni/blob/master/configuration.md#kubernetes-specific)
automatically to use service account token authentication and the Kubernetes Service clusterIP. 

### Etcd

We have provided an example etcd with TLS manifest `canal_etcd_tls.yaml`.

When using an etcd datastore, the provided manifest allows you to specify the etcd endpoints for your etcd cluster,
which must be configured independently.

By default, the manifest expects an etcd proxy to be running on each Kubernetes node at `http://127.0.0.1:2379`.
