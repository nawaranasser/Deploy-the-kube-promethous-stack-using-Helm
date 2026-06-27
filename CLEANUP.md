
# Cleanup

After completing the lab, the monitoring stack can be removed to free local machine resources.

## Remove kube-prometheus-stack

    helm uninstall monitoring-stack -n monitoring
## Delete monitoring namespace
    kubectl delete namespace monitoring

## Delete sample application namespace
    kubectl delete namespace sample-app

    
￼Optional
If the local Kubernetes cluster is no longer needed, delete it from Docker Desktop Kubernetes settings.
