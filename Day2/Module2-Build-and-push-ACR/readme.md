# Building and uploading images to ACR
## Create RG and ACR

global vars

```
ACR_NAME=gsalahacr
RG_NAME=gsalah-rg

```

create a resource group ( if it doesn't exist)

```
az group create --name ${RG_NAME} --location westeurope

```

create your ACR registry 

```
az acr create --resource-group ${RG_NAME} --name ${ACR_NAME} --sku Basic
```


## login to ACR
```
az acr login --name ${ACR_NAME}
```

you should receive the followin message:

```
Login Succeeded
```

## (option 1) build and upload using docker

```
docker build -t myfirst-image . 
docker tag myfirst-image ${ACR_NAME}.azurecr.io/gsg-bootcamp-service:v1
docker push ${ACR_NAME}.azurecr.io/gsg-bootcamp-service:v1
```

## (option 2) build and upload using azure cli

```
az acr build --registry ${ACR_NAME} --image gsg-bootcamp-service:v1 .
```


## list repositories in your ACR

```
az acr repository list --name ${ACR_NAME}.azurecr.io --output table
```


## list tags in your repository

```
az acr repository show-tags --name ${ACR_NAME}.azurecr.io --repository gsg-bootcamp-service --output table
```


# (option 1, the easy way) Connect to ACR using integration with AKS

## Attach your ACR to the cluster

```
az aks update -n AKS_CLUSTER_NAME -g RESOURCE_GROUP --attach-acr ACR_NAME

```

## Detatch 

```
az aks update -n AKS_CLUSTER_NAME -g RESOURCE_GROUP --detach-acr ACR_NAME

```

# (option 2, the generic way ) Connect to ACR using SP and Pull Secrets

reference: https://learn.microsoft.com/en-us/azure/container-registry/container-registry-auth-kubernetes

```
#!/bin/bash

ACR_NAME=gsalahacr
SERVICE_PRINCIPAL_NAME=gsalahacr-sp

# Obtain the full registry ID
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query "id" --output tsv)
# echo $ACR_REGISTRY_ID

# Create the service principal with rights scoped to the registry.
# Default permissions are for docker pull access. Modify the '--role'
# argument value as desired:
# acrpull:     pull only
# acrpush:     push and pull
# owner:       push, pull, and assign roles
PASSWORD=$(az ad sp create-for-rbac --name $SERVICE_PRINCIPAL_NAME --scopes $ACR_REGISTRY_ID --role acrpull --query "password" --output tsv)
USER_NAME=$(az ad sp list --display-name $SERVICE_PRINCIPAL_NAME --query "[].appId" --output tsv)

# Output the service principal's credentials; use these in your services and
# applications to authenticate to the container registry.
echo "Service principal ID: $USER_NAME"
echo "Service principal password: $PASSWORD"


```

# Create Pull Image Secret

```
kubectl create secret docker-registry myacr-pull-secret \
    --docker-server=$ACR_NAME.azurecr.io \
    --docker-username=$USER_NAME \
    --docker-password=$PASSWORD
```