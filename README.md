# homelab-cluster
# ☁️ Kubernetes GitOps Edge Cluster (Homelab)

![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)
![Flux](https://img.shields.io/badge/flux-%23416CA9.svg?style=for-the-badge&logo=flux&logoColor=white)
![Raspberry Pi](https://img.shields.io/badge/-Raspberry%20Pi-C51A4A?style=for-the-badge&logo=Raspberry%20Pi&logoColor=white)
![Cloudflare](https://img.shields.io/badge/Cloudflare-F38020?style=for-the-badge&logo=Cloudflare&logoColor=white)

A production-grade **Edge Kubernetes Cluster** running on bare metal (Raspberry Pi 5). This repository contains the **Infrastructure as Code (IaC)** and **GitOps** configurations to bootstrap and manage the entire state of the cluster.

> **Goal:** To simulate a real-world enterprise environment where cluster state is declarative, secrets are encrypted, and deployment is 100% automated via Flux CD.

---

## 🏗️ Architecture Overview

This cluster follows strict **GitOps principles**. Changes are pushed to this repository, and **Flux CD** automatically reconciles the cluster state.

### Key Features
* **GitOps Core:** Powered by **Flux v2**. No manual `kubectl apply` commands allowed.
* **Edge Computing:** Running on ARM64 architecture (Raspberry Pi 5) with **K3s**.
* **Secret Management:** Secrets are encrypted in-repo using **SOPS** and **Age**. Decryption happens in-memory inside the cluster.
* **Zero-Trust Networking:** No open ports on the router. Public access is handled via **Cloudflare Tunnels**.
* **Storage:** Dynamic Volume Provisioning for stateful workloads (PostgreSQL).

---

## 🛠️ Tech Stack

| Component | Technology | Description |
| :--- | :--- | :--- |
| **Hardware** | Raspberry Pi 5 | 8GB RAM, NVMe Storage |
| **OS / K8s** | K3s | Lightweight Kubernetes distribution for Edge |
| **GitOps** | Flux CD | Continuous Delivery and Reconciliation |
| **Config Mgmt** | Kustomize | Base/Overlay pattern for environment management |
| **Secrets** | SOPS + Age | Encrypted secrets stored safely in Git |
| **Networking** | Cloudflare Tunnel | Secure outbound-only exposure |
| **Apps** | Linkding, etc. | Self-hosted services |

---

## 📂 Repository Structure

The repo is organized to separate the **System Infrastructure** from the **User Applications**.

```text
├── clusters/
│   └── staging/
│       ├── flux-system/        # Flux internal components
│       ├── apps.yaml           # The "Manager" Kustomization for apps
│       └── .sops.yaml          # Encryption rules for SOPS
├── apps/
│   ├── base/                   # Upstream application manifests
│   │   └── linkding/
│   └── staging/                # Environment-specific overlays
│       ├── linkding/
│       │   ├── deployment.yaml
│       │   ├── tunnel.yaml     # Cloudflared ConfigMap & Deployment
│       │   └── kustomization.yaml
└── README.md
