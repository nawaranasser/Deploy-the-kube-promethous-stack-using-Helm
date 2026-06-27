# Troubleshooting Notes

## 1. Minikube memory allocation issue

Minikube failed because the requested memory was higher than the system limit.

Error:

```text
RSRC_OVER_ALLOC_MEM: Requested memory allocation 8192MB is more than your system limit
```
Solution:

I used Docker Desktop Kubernetes / kind cluster instead of Minikube to save time.

## 2. Prometheus port 9090 conflict
When trying to expose Prometheus on localhost:9090, the port was already used by Red Hat Cockpit.

Solution:

I used local port 9091 instead:

    kubectl -n monitoring port-forward svc/monitoring-stack-kube-prom-prometheus 9091:9090
    
## 3. Some monitoring targets appeared DOWN

Some local-cluster targets appeared DOWN, such as:

kube-proxy
etcd
kube-controller-manager
kube-scheduler

This can happen in local Docker Desktop / kind clusters because these metrics endpoints may not be exposed as expected.

The required lab targets were verified successfully.

## 4. nginx pod took time to appear

The nginx deployment was created, but the pod appeared after some delay.

Reason:

The cluster was pulling the nginx:latest image and the system was under load.

The pod eventually became Running.


