# Cilium

CNI, NetworkPolicy enforcement, Gateway API, and Hubble observability.

## Install

```bash
helm repo add cilium https://helm.cilium.io/
helm repo update
helm install cilium cilium/cilium --namespace kube-system -f values.yaml
```

## Prerequisites

k3s must be started with:
- `--flannel-backend=none`
- `--disable-network-policy`
- `--disable-kube-proxy`
