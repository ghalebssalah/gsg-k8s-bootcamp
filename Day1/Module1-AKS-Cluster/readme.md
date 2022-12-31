# Create AKS Cluster 

## Login to Azure portal

az login 


## create resource group 
az group create -l westeurope -n {CLUSTER_RESOURCE_GROUP}



## connect to your cluster 

az aks get-credentials -n {CLUSTER_NAME} -g {CLUSTER_RESOURCE_GROUP}

## get contexts


kubectl config get-contexts 

## Step-04: Explore Cluster Control Plane and Workload inside 

# List Namespaces
kubectl get namespaces
kubectl get ns

# List Pods from all namespaces
kubectl get pods --all-namespaces

# List all k8s objects from Cluster Control plane
kubectl get all --all-namespaces