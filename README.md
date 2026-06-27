# Deploy kube-prometheus-stack Using Helm

This repository documents a hands-on Kubernetes monitoring lab where I deployed the `kube-prometheus-stack` using Helm on a local Kubernetes cluster.

The goal of this lab was not only to run commands, but also to understand what each component does and how Kubernetes monitoring works in practice.

## Lab Objective

Deploy a complete Kubernetes monitoring stack using the `kube-prometheus-stack` Helm chart and verify that all main monitoring components are running correctly.

## Tools Used

* Kubernetes
* Docker Desktop Kubernetes / kind cluster
* kubectl
* Helm
* kube-prometheus-stack
* Prometheus
* Grafana
* Alertmanager
* kube-state-metrics
* Node Exporter
* Prometheus Operator

## Environment

| Item                 | Value                 |
| -------------------- | --------------------- |
| Kubernetes context   | docker-desktop        |
| Kubernetes version   | v1.31.1               |
| Cluster nodes        | 2                     |
| Helm version         | v3.21.2               |
| Monitoring namespace | monitoring            |
| Helm release name    | monitoring-stack      |
| Chart                | kube-prometheus-stack |
| Chart version        | 87.2.1                |
| App version          | v0.92.0               |

## Completed Tasks

### Task 1 – Configure Helm

Added the Prometheus Community Helm repository and searched for the `kube-prometheus-stack` chart.

### Task 2 – Prepare the Namespace

Created a Kubernetes namespace named `monitoring`.

### Task 3 – Deploy the Stack

Installed the `kube-prometheus-stack` Helm chart using the release name `monitoring-stack`.

### Task 4 – Verify the Installation

Verified the created Kubernetes resources:

* Pods
* Deployments
* StatefulSets
* DaemonSets
* Services
* ConfigMaps
* Secrets
* ServiceAccounts

### Task 5 – Identify the Stack Components

Identified and explained the main monitoring components:

* Prometheus Server
* Grafana
* Alertmanager
* kube-state-metrics
* Prometheus Operator
* Node Exporter
* Admission Webhooks

### Task 6 – Explore the Helm Release

Retrieved Helm release information including:

* Release name
* Namespace
* Chart version
* App version
* Revision
* Status
* Deployment time

### Task 7 – Access Grafana

Exposed Grafana temporarily using `kubectl port-forward`, logged in, checked dashboards, and verified the Prometheus data source.

### Task 8 – Access Prometheus

Exposed Prometheus temporarily using `kubectl port-forward`, opened the Prometheus UI, checked targets, and executed PromQL queries.

### Task 9 – Verify Monitoring Targets

Verified that Prometheus was scraping the required monitoring targets:

* Kubernetes API Server
* kubelet
* CoreDNS
* kube-state-metrics
* Node Exporter
* Prometheus Operator
* Prometheus Server

### Task 10 – Explore Custom Resources

Explored Prometheus Operator CRDs and custom resources such as:

* Prometheus
* Alertmanager
* ServiceMonitor
* PodMonitor
* Probe
* PrometheusRule
* ThanosRuler

### Task 11 – Deploy a Sample Application

Deployed a sample `nginx` application, created a service, and accessed it locally using port-forwarding.

## Important Notes

This lab was done on a local Docker Desktop Kubernetes cluster. Some local-cluster targets such as `kube-proxy`, `kube-scheduler`, `kube-controller-manager`, or `etcd` may appear as DOWN depending on how the local Kubernetes environment exposes metrics endpoints.

This does not mean the monitoring stack failed. The required lab targets were verified successfully.

## Result

The kube-prometheus-stack was deployed successfully.

Grafana and Prometheus were accessed successfully.

A sample nginx application was deployed and exposed successfully.

The lab helped me understand Helm, Kubernetes monitoring, Prometheus, Grafana, CRDs, ServiceMonitors, PrometheusRules, and basic troubleshooting in a real Kubernetes environment.
