# Learning Notes – kube-prometheus-stack Helm Lab

This file documents the main questions I asked during the lab and the simplified answers I learned.

---

## What is Helm?

Helm is a package manager for Kubernetes.

Instead of writing many Kubernetes YAML files manually, Helm allows us to install a ready-made package called a Chart.

Simple comparison:

```text
Linux has apt/dnf
Kubernetes has Helm
```

---

## What is a Helm Chart?

A Helm Chart is a package or template that contains Kubernetes resource definitions.

For example, the `kube-prometheus-stack` chart contains definitions for:

* Prometheus
* Grafana
* Alertmanager
* kube-state-metrics
* Node Exporter
* Prometheus Operator
* Services
* ConfigMaps
* Secrets
* CRDs

Simple meaning:

```text
Chart = a ready recipe for installing an application on Kubernetes
```

---

## What is a Helm Release?

A Helm Release is the installed copy of a Helm Chart inside the cluster.

In this lab:

```text
Chart = kube-prometheus-stack
Release = monitoring-stack
```

The chart is the package, and the release is the actual installation created in the cluster.

---

## What is a Stack?

A stack is a group of tools or services that work together to provide a complete function.

In this lab, `kube-prometheus-stack` is not only Prometheus. It includes multiple tools:

* Prometheus for collecting and storing metrics
* Grafana for dashboards
* Alertmanager for alerts
* Node Exporter for node metrics
* kube-state-metrics for Kubernetes object state
* Prometheus Operator for managing the monitoring system
* Admission Webhooks for validation

Simple meaning:

```text
Tool = one component
Stack = multiple components working together
```

---

## What is Prometheus?

Prometheus collects and stores metrics from Kubernetes targets.

It can query metrics using PromQL.

Simple meaning:

```text
Prometheus = metrics collector and metrics database
```

---

## What is Grafana?

Grafana displays metrics using dashboards and charts.

It does not collect metrics by itself. It reads data from Prometheus.

Simple meaning:

```text
Prometheus collects data
Grafana displays data
```

---

## What is Alertmanager?

Alertmanager receives alerts from Prometheus and manages notification routing.

It can send alerts to systems like:

* Email
* Slack
* Webhook
* PagerDuty

Simple meaning:

```text
Alertmanager = alerts manager
```

---

## What is Node Exporter?

Node Exporter is like a monitoring agent installed on every Kubernetes node.

It collects node-level metrics such as:

* CPU
* Memory
* Disk
* Filesystem
* Network

It runs as a DaemonSet, which means Kubernetes runs one copy on each node.

Simple meaning:

```text
Node Exporter = agent on every node
```

---

## What is a DaemonSet?

A DaemonSet ensures that one pod runs on every suitable node in the cluster.

In this lab, Node Exporter was deployed as a DaemonSet.

Because the cluster had 2 nodes, 2 Node Exporter pods were created.

Simple rule:

```text
2 nodes = 2 Node Exporter pods
5 nodes = 5 Node Exporter pods
```

---

## What is kube-state-metrics?

kube-state-metrics exposes metrics about Kubernetes objects.

It does not monitor CPU or RAM directly. Instead, it monitors the state of Kubernetes resources like:

* Pods
* Deployments
* ReplicaSets
* StatefulSets
* DaemonSets
* Nodes
* Jobs

Simple difference:

```text
Node Exporter watches the server/node
kube-state-metrics watches Kubernetes objects
```

---

## What is Prometheus Operator?

Prometheus Operator manages Prometheus and related monitoring resources in a Kubernetes-native way.

It manages resources such as:

* Prometheus
* Alertmanager
* ServiceMonitor
* PodMonitor
* PrometheusRule

Simple meaning:

```text
Prometheus Operator = manager of the Prometheus monitoring stack inside Kubernetes
```

---

## What are Admission Webhooks?

Admission Webhooks check or modify some Kubernetes resources before they are accepted by the Kubernetes API server.

Simple meaning:

```text
Admission Webhook = gatekeeper before a resource enters Kubernetes
```

---

## What is port-forward?

`kubectl port-forward` creates a temporary tunnel from the local machine to a pod or service inside the Kubernetes cluster.

Example:

```bash
kubectl -n monitoring port-forward svc/monitoring-stack-grafana 3000:80
```

This means:

```text
localhost:3000 on my machine
forwards traffic to Grafana service port 80 inside Kubernetes
```

It is temporary. If the terminal is closed, the tunnel stops.

---

## Why did Prometheus use localhost:9091 instead of localhost:9090?

Port `9090` was already used by Red Hat Cockpit on the local machine.

So Prometheus was exposed using a different local port:

```bash
kubectl -n monitoring port-forward svc/monitoring-stack-kube-prom-prometheus 9091:9090
```

This means:

```text
localhost:9091 on my machine
forwards to Prometheus port 9090 inside Kubernetes
```

---

## What does the PromQL query `up` mean?

The `up` query shows whether Prometheus can scrape each target successfully.

Result values:

```text
1 = target is UP
0 = target is DOWN
```

This query was used to verify monitoring targets.

---

## Why were some targets DOWN?

Some local-cluster targets such as kube-proxy, etcd, kube-controller-manager, and kube-scheduler appeared as DOWN.

This can happen in Docker Desktop / kind local clusters because some control-plane metrics endpoints are not reachable on the expected ports.

The required lab targets were still verified as UP.

---

## What is a CRD?

CRD means Custom Resource Definition.

It allows Kubernetes to understand new resource types that are not built-in by default.

Prometheus Operator installed CRDs such as:

* Prometheus
* Alertmanager
* ServiceMonitor
* PodMonitor
* Probe
* PrometheusRule
* ThanosRuler

Simple meaning:

```text
CRD = a way to teach Kubernetes a new resource type
```

---

## What is a ServiceMonitor?

A ServiceMonitor tells Prometheus which Kubernetes services to scrape for metrics.

Simple meaning:

```text
ServiceMonitor = tells Prometheus to monitor a service
```

---

## What is a PrometheusRule?

A PrometheusRule contains alerting rules or recording rules for Prometheus.

Simple meaning:

```text
PrometheusRule = rules for alerts or precomputed metrics
```

---

## What did I learn from this lab?

I learned how to:

* Use Helm to install a Kubernetes monitoring stack
* Understand the difference between chart and release
* Verify Kubernetes resources after deployment
* Access internal services using port-forward
* Use Prometheus queries
* Explore Grafana dashboards
* Understand monitoring targets
* Understand CRDs and custom resources
* Deploy and expose a sample nginx application
* Troubleshoot local Kubernetes issues

