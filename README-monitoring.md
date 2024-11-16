# 🚀 Prometheus and Grafana Installation Guide

Easily set up **Prometheus** 🛠️ and **Grafana** 📊 on your Kubernetes cluster using Helm. These tools are perfect for monitoring and visualizing cluster metrics. Follow this beautifully organized guide for a seamless setup experience! ✨

---

## 🌟 Step 1: Create Namespaces

To keep your installations clean and organized, let's create dedicated namespaces for Prometheus and Grafana:

```bash
# Create namespace for Prometheus
kubectl create namespace prometheus

# Create namespace for Grafana
kubectl create namespace grafana
```

> 🎯 **Tip**: Namespaces help isolate resources and manage configurations more effectively.

---

## ⚙️ Step 2: Install Prometheus with Helm

Install Prometheus into the `prometheus` namespace with this simple Helm command:

```bash
helm install prometheus prometheus-community/prometheus --namespace prometheus
```

### ✅ Verify Installation

Check if all Prometheus resources are up and running:

```bash
kubectl get all -n prometheus
```

### 🌐 Access Prometheus

#### 🔄 Port Forwarding (Local Access)

For local testing, forward the Prometheus service port:

```bash
kubectl port-forward -n prometheus service/prometheus-server 9090:80
```

Access Prometheus via your browser:

```
http://localhost:9090
```


#### 🌍 Expose Prometheus Externally (NodePort or LoadBalancer)

Change the service type to `NodePort` or `LoadBalancer`:

```bash
kubectl patch service prometheus-server -n prometheus -p '{"spec": {"type": "NodePort"}}'
```

Retrieve the NodePort with the following command:

```bash
kubectl get service prometheus-server -n prometheus -o=jsonpath='{.spec.ports[0].nodePort}'
```

Access Prometheus at:

```
http://<NodeIP>:<NodePort>
```

#### 🌍 Expose Grafana Externally (NodePort or LoadBalancer)

Change the service type to `NodePort` or `LoadBalancer`:

```bash
kubectl patch service grafana -n grafana -p '{"spec": {"type": "NodePort"}}'
```

Retrieve the NodePort with the following command:

```bash
kubectl get service grafana -n grafana -o=jsonpath='{.spec.ports[0].nodePort}'
```

Access Grafana at:

```
http://<NodeIP>:<NodePort>
```


---

## 🎨 Step 3: Install Grafana with Helm

Install Grafana into the `grafana` namespace using this command:

```bash
helm install grafana grafana/grafana --namespace grafana
```

### ✅ Verify Installation

Ensure all Grafana resources are up and running:

```bash
kubectl get all -n grafana
```

### 🌐 Access Grafana

#### 🔄 Port Forwarding (Local Access)

Forward the Grafana service port locally:

```bash
kubectl port-forward -n grafana service/grafana 3000:80
```

Access Grafana via your browser:

```
http://localhost:3000
```

#### 🌍 Expose Grafana Externally (NodePort or LoadBalancer)

Modify the service type for external access:

```bash
kubectl patch service grafana -n grafana -p '{"spec": {"type": "NodePort"}}'
```

Retrieve the NodePort and access Grafana at:

```
http://<NodeIP>:<NodePort>
```

---

## 🔑 Step 4: Retrieve Grafana Admin Credentials

Grafana generates default admin credentials during installation. Retrieve them as follows:

### 📝 Get the Secret

```bash
kubectl get secret -n grafana grafana -o yaml
```

### 🔓 Decode the Credentials

Decode the base64-encoded admin username and password:

#### Decode Admin Username

```bash
echo "<username_value>" | openssl base64 -d ; echo
```

#### Decode Admin Password

```bash
echo "<password_value>" | openssl base64 -d ; echo
```

> Replace `<username_value>` and `<password_value>` with the corresponding values from the secret.

### Example Output

Here's how the decoded credentials might look:

```yaml
data:
  admin-user: YWRtaW4=    # (encoded value for "admin")
  admin-password: cGFzc3dvcmQ=  # (encoded value for "password")
```

After decoding:

- **Username**: `admin` 👤
- **Password**: `password` 🔐

---

## 🧹 Step 5: Cleanup (Optional)

If you need to uninstall Prometheus and Grafana, here’s how to do it safely.

### ❌ Uninstall Prometheus

```bash
helm uninstall prometheus --namespace prometheus
kubectl delete namespace prometheus
```

### ❌ Uninstall Grafana

```bash
helm uninstall grafana --namespace grafana
kubectl delete namespace grafana
```

### 🧪 Verify Cleanup

Ensure all resources have been removed:

```bash
kubectl get all -n prometheus
kubectl get all -n grafana
```

Expected output:

```
Error from server (NotFound): namespaces "prometheus" not found
Error from server (NotFound): namespaces "grafana" not found
```

---

