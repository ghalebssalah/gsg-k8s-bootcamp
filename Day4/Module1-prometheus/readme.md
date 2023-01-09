# Prometheus installation

## installation
```
k create namespace monitoring

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

helm install -n monitoring prometheus prometheus-community/kube-prometheus-stack 


```


## Access Grafana

```
k -n monitoring port-forward svc/prometheus-grafana 8080:80
```

username admin
password prom-operator
