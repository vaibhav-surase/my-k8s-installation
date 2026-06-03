# Kubernetes Starter Kit & Automated Installation

An automated production-ready setup guide and collection of shell scripts to provision and initialize a multi-node Kubernetes (k8s) cluster. This repository serves as a starter kit for learning, testing, and deploying containerized applications with absolute control over the infrastructure.

---

## 🚀 Features

- **Automated Bootstrap:** Clean and modular shell scripts to handle dependencies.
- **Production-Ready Configuration:** Pre-configured settings for container runtimes, networking, and security parameters.
- **Architecture Ready:** Clear separation for Master (Control Plane) and Worker Node setups.
- **Optimized for DevOps:** Perfect baseline repository for integrating with CI/CD tools like Jenkins, GitHub Actions, or ArgoCD.

---

## 🛠️ Prerequisites & System Requirements

Before starting the installation, ensure your instances (AWS EC2, GCP VM, or Bare Metal) meet the following minimum specs:

| Node Type | Minimum CPU | Minimum RAM | Recommended OS |
| :--- | :--- | :--- | :--- |
| **Master Node** | 2 vCPUs | 4 GB | Ubuntu 22.04 / 24.04 LTS or RHEL |
| **Worker Node** | 1 vCPU | 2 GB | Ubuntu 22.04 / 24.04 LTS or RHEL |

> ⚠️ **Important:** Ensure unique hostnames for all nodes and keep swap space completely disabled on all Linux environments.

---

## 💻 Quick Installation Steps

Follow these sequential steps to boot your cluster:

### 1. Clone the Repository
```bash
git clone [https://github.com/vaibhav-surase/my-k8s-installation.git](https://github.com/vaibhav-surase/my-k8s-installation.git)
cd my-k8s-installation

2. Configure Prerequisites (All Nodes)
Run the initial setup scripts to configure the container runtime (containerd/docker), network bridge parameters, and disable swap.
# Make scripts executable
chmod +x scripts/*.sh

# Execute common prerequisites configuration
./scripts/common-prereqs.sh

3. Initialize Control Plane (Master Node Only)
Run the initialization script on your Master node to set up the control plane components.
./scripts/master-init.sh
Note: Copy the kubeadm join token generated at the end of this script output.

4. Join Worker Nodes (Worker Nodes Only)
Paste the join token copied from the Master node output on all your worker nodes as root:
sudo kubeadm join <master-node-ip>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>

🌐 Cluster Verification
Once the network plugin (CNI) is applied, verify that your cluster nodes are healthy and interacting successfully:
kubectl get nodes
kubectl get pods -A
