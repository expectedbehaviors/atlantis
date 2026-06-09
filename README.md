# Atlantis Helm Chart

Baseline Helm chart for [Atlantis](https://www.runatlantis.io) Terraform PR automation. Wraps the [upstream runatlantis Helm chart](https://github.com/runatlantis/helm-charts) with optional [OnePasswordItem-helm](https://github.com/expectedbehaviors/OnePasswordItem-helm) for webhook and cloud credentials.

## Subcharts

| Subchart | Source | Values prefix | Description |
|----------|--------|---------------|-------------|
| **atlantis-server** | [runatlantis/helm-charts](https://github.com/runatlantis/helm-charts) | `atlantis-server.*` | Atlantis server, ingress, PVC for repo clones/plans. |
| **onepassworditem** | [OnePasswordItem-helm](https://github.com/expectedbehaviors/OnePasswordItem-helm) | `onepassworditem.*` | Optional sync of GitHub App key and AWS creds into `atlantis-webhook`. |

## External credentials

Atlantis stores plan/clone state on a **local PVC** (`volumeClaim`). GitHub App credentials and optional AWS keys for remote Terraform backends are supplied via Kubernetes Secret **`atlantis-webhook`**:

1. **OnePasswordItem:** set `onepassworditem.items` with your vault item (maps to `atlantis-webhook`).
2. **External Secrets:** create `atlantis-webhook` manually or via ESO before install.
3. **Manual:** `kubectl create secret generic atlantis-webhook ...` with `key.pem`, `github_secret`, and optional AWS keys.

Override `githubApp.*`, `orgAllowlist`, `atlantisUrl`, and ingress host for your GitHub org and domain.

## Prerequisites

- Kubernetes cluster with default StorageClass (Atlantis data PVC)
- Ingress controller (when ingress enabled)
- GitHub App with webhook URL matching `atlantisUrl`
- 1Password Connect or External Secrets (when syncing credentials)

## Install

```bash
helm dependency update .
helm template release . -f values.yaml
```

## Support this project

I build tools to get the best homelab experience I can from what's available and to grow as a programmer along the way. If you'd like to contribute, donations go toward homelab operating costs and subscriptions that keep this tooling maintained. Optional and appreciated.

[![Donate with PayPal](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://www.paypal.com/donate/?business=9RHVW92WMWQNL&no_recurring=0&item_name=Optional+donations+help+support+Expected+Behaviors%E2%80%99+open+source+work.+Thank+you.&currency_code=USD)
