# 🚀 Install Jenkins on Kubernetes with Helm 🌟

This guide walks you through installing Jenkins on a Kubernetes cluster using Helm. Jenkins will be exposed via a NodePort, and we'll use a pre-created Kubernetes secret for admin credentials.

---

## 🛠 Prerequisites
Before you begin, ensure the following: 

1. ✅ Kubernetes cluster is up and running.
2. ✅ `kubectl` is installed and configured to communicate with your cluster.
3. ✅ Helm is installed (v3+ recommended).

---

## 📋 Steps to Install Jenkins

### 1️⃣ Add the Jenkins Helm Chart Repository
Add the official Jenkins Helm chart repository and update it:

    helm repo add jenkins https://charts.jenkins.io

    helm repo update

---

### 2️⃣ Create a Namespace for Jenkins
Create a Kubernetes namespace to isolate Jenkins resources:

    kubectl create namespace jenkins

---

### 3️⃣ Create a Kubernetes Secret for Admin Credentials
Create a secret in the `jenkins` namespace to store admin credentials:

    kubectl create secret generic jenkins-admin-credentials \
      --from-literal=username=your-username \
      --from-literal=password=your-password \
      -n jenkins

You can verify the secret creation:

    kubectl get secrets -n jenkins

---

### 4️⃣ Create a Custom `values.yaml` File
Create a `values.yaml` file to configure the Jenkins Helm chart.
- For the service type, choose **LoadBalancer** or **NodePort**.
- Note: On Windows, **LoadBalancer** might not work.

---

### 5️⃣ Install Jenkins with Helm
Run the following Helm command to install Jenkins with the custom `values.yaml`:

    helm upgrade --install jenkins jenkins/jenkins -n jenkins -f values.yaml

- **jenkins/jenkins**: Refers to the Helm chart for Jenkins.
- **-n jenkins**: Deploys Jenkins into the `jenkins` namespace.
- **-f values.yaml**: Uses your custom configuration.

---

### 6️⃣ Verify the Deployment
Check the status of the Jenkins pod:

    kubectl get pods -n jenkins

Expected output will list the Jenkins pods. Next, verify the services:

    kubectl get svc -n jenkins

---

### 7️⃣ Access Jenkins
Use the following URL in your browser to access Jenkins:

    http://<localhost>:<port-of-the-node>

Replace `<port-of-the-node>` with the actual port number. For example: `30514`.

---

## 🧹 Optional: Clean Up
To uninstall Jenkins and clean up all resources:

    helm uninstall jenkins -n jenkins

Then delete the namespace:

    kubectl delete namespace jenkins

---

## 🎉 Conclusion
🎊 Congratulations! You have successfully installed Jenkins on Kubernetes using Helm. Customize Jenkins further by adding plugins and configuring pipelines to suit your needs. 🚀
+++
