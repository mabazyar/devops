# Installing Prometheus on Kubernetes

Prometheus is a popular open-source monitoring and alerting solution that can be used to monitor the health and performance of Kubernetes clusters. In this guide, we will walk through the steps required to install Prometheus on a Kubernetes cluster.

## Prerequisites

Before installing Prometheus on Kubernetes, you must have:

- A running Kubernetes cluster
- `kubectl` command-line tool installed and configured to communicate with your cluster

## Step 1: Create a Namespace
First, create a namespace for Prometheus to run in. 
You can use the following `kubectl` command to create a new namespace called `prometheus`:
```
kubectl create namespace prometheus
```

## Step 2: Install Prometheus
To install Prometheus, you will need to create a Kubernetes manifest file. You can create a file called `prometheus.yaml` with the following contents:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: prometheus
  namespace: prometheus
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRole
metadata:
  name: prometheus
rules:
- apiGroups:
  - ""
  resources:
  - nodes
  - endpoints
  - services
  - pods
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - extensions
  resources:
  - ingresses
  verbs:
  - get
  - list
  - watch
- nonResourceURLs:
  - "/metrics"
  verbs:
  - get
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: prometheus
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: prometheus
subjects:
- kind: ServiceAccount
  name: prometheus
  namespace: prometheus
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: prometheus-server-conf
  namespace: prometheus
data:
  prometheus.yml: |-
    global:
      scrape_interval: 15s
      evaluation_interval: 15s
    scrape_configs:
    - job_name: 'kubernetes-apiservers'
      kubernetes_sd_configs:
      - role: endpoints
      scheme: https
      tls_config:
        ca_file: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
      bearer_token_file: /var/run/secrets/kubernetes.io/serviceaccount/token
      relabel_configs:
      - source_labels: [__meta_kubernetes_namespace, __meta_kubernetes_service_name, __meta_kubernetes_endpoint_port_name]
        action: keep
        regex: default;kubernetes;https
    - job_name: 'kubernetes-nodes'
      kubernetes_sd_configs:
      - role: node
      scheme: https
      tls_config:
        ca_file: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
      bearer_token_file: /var/run/secrets/kubernetes.io/serviceaccount/token
      relabel_configs:
      - action: labelmap
        regex: __meta_kubernetes_node_label_(.+)
      - target_label: __address__
        replacement: kubernetes.default.svc:443
      - source_labels: [__meta_kubernetes_node_name]
        target_label: __metrics_path__
        replacement: /api/v1/nodes/${1}/proxy/metrics
    - job_name: 'kubernetes-pods'
      kubernetes_sd_configs:
      - role: pod
      scheme: https
  tls_config:
    ca_file: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
  bearer_token_file: /var/run/secrets/kubernetes.io/serviceaccount/token
  relabel_configs:
  - action: labelmap
    regex: __meta_kubernetes_pod_label_(.+)
  - target_label: __address__
    replacement: kubernetes.default.svc:443
  - source_labels: [__meta_kubernetes_namespace]
    target_label: kubernetes_namespace
  - source_labels: [__meta_kubernetes_pod_name]
    target_label: kubernetes_pod_name
- job_name: 'kubelet'
  static_configs:
  - targets: ['localhost:10250']
- job_name: 'node-exporter'
  static_configs:
  - targets: ['localhost:9100']
  ```
  
  
This manifest file will create a Kubernetes service account, cluster role, and config map for Prometheus to use. It also defines the Prometheus configuration in the `prometheus.yml` file.

To install Prometheus, run the following `kubectl` command:

```
kubectl apply -f prometheus.yaml
```


This will create the necessary resources in the `prometheus` namespace.

## Step 3: Accessing Prometheus

By default, Prometheus will be exposed using a Kubernetes service called `prometheus-server`. To access Prometheus, you can use the following `kubectl` command to port-forward the service to your local machine:

```
kubectl port-forward -n prometheus svc/prometheus-server 9090
```


You can then access Prometheus by navigating to `http://localhost:9090` in your web browser.

## Step 4: Cleanup

To uninstall Prometheus, you can use the following `kubectl` command:

```
kubectl delete -f prometheus.yaml
```

This will delete all the resources created by the manifest file.

## Conclusion

In this guide, we have walked through the steps required to install Prometheus on a Kubernetes cluster. Prometheus is a powerful monitoring tool that can help you keep your Kubernetes clusters healthy and performing optimally.

     

