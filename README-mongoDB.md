# 🌟 MongoDB Installation with Helm (Windows Only) 🌟

This guide walks you through the process of installing **MongoDB** in a Kubernetes cluster using Helm. It is designed specifically for **Windows users**. MacOS users should instead install **PostgreSQL**, with instructions provided below.

---

## 🚀 Prerequisites

Before you get started, ensure the following tools are installed on your system:

- **Helm**: 🛠️ Install Helm if it's not already available.
- **kubectl**: 📡 Install kubectl and configure it to interact with your Kubernetes cluster.

---

## 🧑‍💻 Steps to Install MongoDB with Helm

### 1️⃣ Install MongoDB
Run the following command to install MongoDB and create its namespace:

```bash
helm install my-release oci://registry-1.docker.io/bitnamicharts/mongodb --namespace mongodb --create-namespace
```

This command:
- 📂 Installs MongoDB in the `mongodb` namespace.
- ✨ Automatically creates the namespace if it doesn't exist.

---

### 🔑 Retrieving the MongoDB Password
The MongoDB password is generated automatically during installation. Retrieve it using:

```bash
export MONGODB_ROOT_PASSWORD=$(kubectl get secret --namespace mongodb my-release-mongodb -o jsonpath="{.data.mongodb-root-password}" | base64 --decode)
```

---

### 🔗 Accessing MongoDB Inside the Cluster

#### 1️⃣ Start a MongoDB Client Pod
To connect to MongoDB within the Kubernetes cluster, execute:

```bash
kubectl run --namespace mongodb my-release-mongodb-client --rm --tty -i --restart='Never' --env="MONGODB_ROOT_PASSWORD=$MONGODB_ROOT_PASSWORD" --image docker.io/bitnami/mongodb:8.0.3-debian-12-r0 --command -- bash
```

This command:
- 🏗️ Starts a temporary pod named `my-release-mongodb-client` in the `mongodb` namespace.
- 🔐 Sets the `MONGODB_ROOT_PASSWORD` environment variable with the retrieved password.

#### 2️⃣ Connect to MongoDB
Inside the pod, connect to MongoDB using:

```bash
mongosh admin --host "my-release-mongodb" --authenticationDatabase admin -u root -p $MONGODB_ROOT_PASSWORD
```

🎉 You are now connected to the MongoDB instance inside the cluster!

---

## 🖥️ Note for MacOS Users
Unfortunately, MongoDB is not supported in this setup for MacOS. MacOS users should use **PostgreSQL** instead.

---

### 🐘 Installing PostgreSQL on MacOS

Follow the official PostgreSQL installation guide for MacOS:

1. 🍎 [Install PostgreSQL on MacOS](https://www.postgresql.org/download/macos/).
2. ✅ Once installed, use `psql` to interact with your PostgreSQL database.

---

## 📚 Additional Documentation

- 📖 [Bitnami MongoDB Helm Chart Documentation](https://bitnami.com/stack/mongodb/helm)
- 📘 [PostgreSQL Official Documentation](https://www.postgresql.org/docs/)

🌟 Happy Coding! 🌟

---

## 🧹 Cleanup: Deleting the MongoDB Installation

If you want to remove the MongoDB installation from your Kubernetes cluster, follow these steps:

### 1️⃣ Uninstall MongoDB
Run the following command to uninstall MongoDB and delete its associated resources:

```bash
helm uninstall my-release --namespace mongodb
```

This command removes all resources associated with the MongoDB Helm release.

### 2️⃣ Delete the MongoDB Namespace
To clean up the namespace created during the installation, run:

```bash
kubectl delete namespace mongodb
```

This command deletes the `mongodb` namespace and all remaining resources within it.

💡 **Note:** Ensure you have backed up any important data before running these commands as they will permanently delete the MongoDB installation and its data.
 
---
