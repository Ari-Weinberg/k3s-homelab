# Claude Instructions

## Working Style

This repo is a learning environment. When helping with this project:

- **Explain commands before or after running them** — what they do, what the output means, why it matters
- **Run commands autonomously only for troubleshooting/diagnostics** — give the user meaningful commands to run themselves
- **Explain Kubernetes concepts** when they come up naturally — don't just fix things silently

## Repo Purpose

Learning Kubernetes through building a real cluster. The goal is understanding, not just a working cluster.

## Cluster Access

- kubectl is configured and connects to the cluster at 10.2.10.10
- Helm is installed
- Nodes are on VLAN 10 (10.2.10.0/24)

## Conventions

- Infrastructure tools live in `infrastructure/<tool>/` with a `values.yaml` and `README.md`
- Each README contains the exact `helm install` command for that tool
- Prefer `helm install ... -f values.yaml` over long `--set` flag chains
