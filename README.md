# k3s Homelab Cluster

A 3-node k3s cluster built for learning Kubernetes. Deployed on Debian 12 VMs via [Ludus](https://ludus.cloud) using the `netpenguins.ludus_k3s` Ansible role.

## Cluster

| Node | Role | IP |
|---|---|---|
| aw-k3s-server | control-plane, etcd | 10.2.10.10 |
| aw-k3s-agent-1 | worker | 10.2.10.11 |
| aw-k3s-agent-2 | worker | 10.2.10.12 |

**k3s version:** v1.34.5+k3s1
**k3s flags:** `--flannel-backend=none --disable-network-policy --disable-kube-proxy --cluster-init --disable traefik`

## Node Prerequisites

All nodes require the following packages installed at the OS level:

| Package | Required by | Purpose |
|---|---|---|
| `open-iscsi` | Longhorn | Block storage volume attachment |

## Stack

| Layer | Tool | Purpose |
|---|---|---|
| CNI | Cilium + Hubble | Networking, NetworkPolicy, Gateway API, flow visibility |
| Load Balancer | MetalLB | Bare-metal LB, IP pool assignment |
| GitOps | Flux CD | Cluster-as-code, reconciles from this repo |
| Policy Engine | Kyverno | Admission control, pod security standards |
| Runtime Security | Falco | Syscall-level anomaly detection |
| Vuln Scanning | Trivy Operator | Continuous image scanning, CIS benchmarks |
| TLS | cert-manager | Automated certificate management |
| Metrics | Prometheus + Grafana | Metrics collection and dashboards |
| Logging | Loki + Promtail | Centralized log aggregation |

## Repo Structure

```
k3s/
├── infrastructure/       # Core cluster infrastructure (Helm installs)
│   ├── cilium/
│   ├── metallb/
│   ├── cert-manager/
│   ├── monitoring/       # Prometheus + Grafana
│   ├── logging/          # Loki + Promtail
│   └── security/         # Falco, Kyverno, Trivy
├── apps/                 # Application workloads
└── clusters/
    └── local/            # Flux cluster-specific config
```

Each directory under `infrastructure/` contains a `values.yaml` and a `README.md` with the install command.

## Bootstrap Order

1. **Cilium** — must be first, cluster is `NotReady` without a CNI
2. **MetalLB** — required before any `LoadBalancer` services work
3. **Flux CD** — bootstraps GitOps; everything after is managed from the repo
4. Everything else via Flux
