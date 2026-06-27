# Commands Used in the kube-prometheus-stack Helm Lab

## Prerequisites Check

```bash
kubectl config current-context
```

```bash
kubectl get nodes
```

```bash
kubectl auth can-i '*' '*' --all-namespaces
```

```bash
helm version
```

---

# Task 1 – Configure Helm

Add the Prometheus Community Helm repository:

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

Update Helm repositories:

```bash
helm repo update
```

List Helm repositories:

```bash
helm repo list
```

Search for the kube-prometheus-stack chart:

```bash
helm search repo prometheus-community/kube-prometheus-stack
```

Result used in this lab:

```text
Chart Version: 87.2.1
App Version: v0.92.0
```

---

# Task 2 – Prepare the Namespace

Create the monitoring namespace:

```bash
kubectl create namespace monitoring
```

Verify namespace creation:

```bash
kubectl get namespaces
```

```bash
kubectl get namespace monitoring
```

---

# Task 3 – Deploy the Stack

Install kube-prometheus-stack using Helm:

```bash
helm install monitoring-stack prometheus-community/kube-prometheus-stack \
  --namespace monitoring \
  --version 87.2.1 \
  --wait \
  --timeout 10m
```

Verify the Helm release:

```bash
helm list -n monitoring
```

Check pods:

```bash
kubectl get pods -n monitoring
```

```bash
kubectl get pods -n monitoring -o wide
```

---

# Task 4 – Verify the Installation

Verify all required Kubernetes resources:

```bash
kubectl get pods,deployments,statefulsets,daemonsets,services,configmaps,secrets,serviceaccounts -n monitoring
```

Optional individual checks:

```bash
kubectl get pods -n monitoring
kubectl get deployments -n monitoring
kubectl get statefulsets -n monitoring
kubectl get daemonsets -n monitoring
kubectl get services -n monitoring
kubectl get configmaps -n monitoring
kubectl get secrets -n monitoring
kubectl get serviceaccounts -n monitoring
```

---

# Task 5 – Identify the Stack Components

List monitoring pods:

```bash
kubectl get pods -n monitoring
```

Check validating webhooks:

```bash
kubectl get validatingwebhookconfigurations | grep monitoring
```

Check mutating webhooks:

```bash
kubectl get mutatingwebhookconfigurations | grep monitoring
```

---

# Task 6 – Explore the Helm Release

Show Helm release information:

```bash
helm list -n monitoring
```

Show Helm release information in YAML:

```bash
helm list -n monitoring -o yaml
```

Show detailed release status:

```bash
helm status monitoring-stack -n monitoring
```

---

# Task 7 – Access Grafana

Get Grafana admin password:

```bash
kubectl --namespace monitoring get secrets monitoring-stack-grafana \
  -o jsonpath="{.data.admin-password}" | base64 -d ; echo
```

Important: Do not commit the password output to GitHub.

Port-forward Grafana:

```bash
kubectl -n monitoring port-forward svc/monitoring-stack-grafana 3000:80
```

Open Grafana:

```text
http://localhost:3000
```

Login:

```text
Username: admin
Password: retrieved from the Kubernetes secret
```

---

# Task 8 – Access Prometheus

Port 9090 was already used by Red Hat Cockpit on the local machine, so Prometheus was exposed using local port 9091:

```bash
kubectl -n monitoring port-forward svc/monitoring-stack-kube-prom-prometheus 9091:9090
```

Open Prometheus:

```text
http://localhost:9091
```

Run PromQL query:

```promql
up
```

Open targets page:

```text
Status → Targets
```

Open service discovery page:

```text
Status → Service Discovery
```

---

# Task 9 – Verify Monitoring Targets

Main PromQL query:

```promql
up
```

Check Node Exporter:

```promql
up{job=~".*node-exporter.*"}
```

Check kube-state-metrics:

```promql
up{job=~".*kube-state-metrics.*"}
```

Check Prometheus:

```promql
up{job=~".*prometheus.*"}
```

Check Prometheus Operator:

```promql
up{job=~".*operator.*"}
```

Check DOWN targets:

```promql
up == 0
```

Summary query:

```promql
count by (job) (up)
```

---

# Task 10 – Explore Custom Resources

List Prometheus Operator CRDs:

```bash
kubectl get crds | grep monitoring.coreos.com
```

List custom resources in the monitoring namespace:

```bash
kubectl get prometheus,alertmanager,servicemonitor,podmonitor,prometheusrule,probe,thanosruler -n monitoring
```

Individual checks:

```bash
kubectl get prometheus -n monitoring
kubectl get alertmanager -n monitoring
kubectl get servicemonitor -n monitoring
kubectl get podmonitor -n monitoring
kubectl get prometheusrule -n monitoring
kubectl get probe -n monitoring
kubectl get thanosruler -n monitoring
```

---

# Task 11 – Deploy a Sample Application

Create namespace:

```bash
kubectl create namespace sample-app
```

Verify namespace:

```bash
kubectl get ns sample-app
```

Create nginx deployment:

```bash
kubectl create deployment nginx-demo \
  --image=nginx:latest \
  --port=80 \
  -n sample-app
```

Verify pod:

```bash
kubectl get pods -n sample-app
```

Verify deployment:

```bash
kubectl get deployments.apps -n sample-app
```

Expose deployment as a ClusterIP service:

```bash
kubectl expose deployment nginx-demo \
  --type=ClusterIP \
  --port=80 \
  --target-port=80 \
  -n sample-app
```

Verify service:

```bash
kubectl get svc -n sample-app
```

Port-forward nginx service:

```bash
kubectl -n sample-app port-forward svc/nginx-demo 8088:80
```

Open nginx:

```text
http://localhost:8088
```

---

# Cleanup Commands

Uninstall the Helm release:

```bash
helm uninstall monitoring-stack -n monitoring
```

Delete monitoring namespace:

```bash
kubectl delete namespace monitoring
```

Delete sample application namespace:

```bash
kubectl delete namespace sample-app
```

Optional: delete the whole local Kubernetes cluster from Docker Desktop if no longer needed.

