# ArgoCD Installation Guide
- [ArgoCD Installation Guide](#argocd-installation-guide)
  - [Overview](#overview)
  - [Prerequisites](#prerequisites)
  - [Step 1: Install ArgoCD](#step-1-install-argocd)
  - [Step 2: Expose the ArgoCD Server](#step-2-expose-the-argocd-server)
  - [Step 3: Retrieve the ArgoCD Initial Admin Password](#step-3-retrieve-the-argocd-initial-admin-password)
  - [Step 4: Access the ArgoCD UI](#step-4-access-the-argocd-ui)
    - [Cleanup](#cleanup)

![ArgoCD Logo](https://argo-cd.readthedocs.io/en/stable/assets/logo.png)

## Overview
This guide will help you set up an instance of **ArgoCD** on your Kubernetes cluster, allowing you to manage your applications with ease. Follow the steps carefully to ensure a smooth installation and configuration process.

> **Note**: This guide is based on the official [ArgoCD Getting Started documentation](https://argo-cd.readthedocs.io/en/stable/getting_started/).

## Prerequisites

Ensure you have the following before starting:

1. **kubectl command-line tool** installed and configured.
2. Access to a **Kubernetes cluster**.
3. A **kubeconfig file** (default location: `~/.kube/config`) that is connected to your Kubernetes cluster.

## Step 1: Install ArgoCD

First, create a new namespace where ArgoCD will reside:

```shell
kubectl create namespace argocd
```

Then, apply the ArgoCD installation manifest to set up ArgoCD components within this namespace:

```shell
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

This will set up the ArgoCD services and necessary application resources in the `argocd` namespace.

## Step 2: Expose the ArgoCD Server

To access the ArgoCD UI externally, change the `argocd-server` service type to `LoadBalancer` or `NodePort`:

```shell
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
```

> **Note**: If you're on a cloud platform, the `LoadBalancer` type will automatically provision an external IP. On other environments, you might need to set up port-forwarding. If you are on windows use NodePort.

## Step 3: Retrieve the ArgoCD Initial Admin Password

Use the following command to obtain the default password for the `admin` user:

```shell
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath='{.data.password}' | base64 -d
```

## Step 4: Access the ArgoCD UI

1. Open your browser and navigate to the ArgoCD UI at:

   ```
   http://<IP-address-or-server-name>
   ```

   Replace `<IP-address-or-server-name>` with the external IP assigned to the `argocd-server` service.

2. Log in to the ArgoCD UI with the following credentials:

   - **Username**: `admin`
   - **Password**: The password retrieved in Step 3.

---

With these steps, you have successfully installed ArgoCD on your Kubernetes cluster! You can now start deploying and managing applications using GitOps with ArgoCD.

### Cleanup
1. To uninstall ArgoCD, run:
   ```bash
   kubectl delete namespace argocd
   ```
2. Confirm the deletion:
   ```bash
   kubectl get namespaces 
   ```