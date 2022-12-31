# Create AKS Cluster 

## Login to Azure portal
```
az login 
```

## connect to your cluster 
```
az aks get-credentials -n {CLUSTER_NAME} -g {CLUSTER_RESOURCE_GROUP}
```

## get contexts
```
kubectl config get-contexts 
```

## List Namespaces

```
kubectl get namespaces
kubectl get ns
```

## List Pods from all namespaces

```
kubectl get pods --all-namespaces
```


## List all k8s objects from Cluster Control plane
```
kubectl get all --all-namespaces
```


## List services
```
kubectl get service
kubectl get svc
```


## List deployment
```
kubectl get deployment
kubectl get deploy
```
