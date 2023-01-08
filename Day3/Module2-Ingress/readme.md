# Ingresses

Features of Ingress:
    1. Host-Based Routing
    2. Context-Based Routing or Path-Based Routing
    3. SSL Termination 


Componentes:
    1. Controller 
    2. Resource


Reference: https://kubernetes.io/docs/reference/kubernetes-api/service-resources/ingress-v1/

# Procedure


## 1. Create a static public IP 

1. first get your aks node resource group

```
 az aks show --resource-group {AKS_RESOURCE_GROUP} --name {AKS_NAME} --query nodeResourceGroup -o tsv
```

for example:  az aks show --resource-group gsalah-rg --name gsalah-aks --query nodeResourceGroup -o tsv

you will get a resource group name like : MC_gsalah-rg_gsalah-aks_westus3



2. now create a static publich ip

```
az network public-ip create --resource-group  {AKS-NODE-RESOURCE-GROUP} \
                            --name YOUR_PUBLIC_IP_NAME \
                            --sku Standard \
                            --allocation-method static \
                            --query publicIp.ipAddress \
                            -o tsv

```
for example: 

```
az network public-ip create --resource-group  MC_gsalah-rg_gsalah-aks_westus3\
                            --name gsgBootCampPublicAddreess \
                            --sku Standard \
                            --allocation-method static \
                            --query publicIp.ipAddress \
                            -o tsv
```

you will get an ip address: 20.171.129.232

**save the ip**



## 2. Install Ingress

1. Create a namespace
```
kubectl create namespace ingress-nginx
```

2. Add the official nginx repo 
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
```

3. Install Nginx

replace YOUR_IP_ADDRESS with the one you created earlier 

```
helm install ingress-nginx ingress-nginx/ingress-nginx \
    --namespace ingress-nginx \
    --set controller.nodeSelector."kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."kubernetes\.io/os"=linux \
    --set controller.service.externalTrafficPolicy=Local \
    --set controller.service.loadBalancerIP=YOUR_IP_ADDRESS 

```

for example: 

```
helm install ingress-nginx ingress-nginx/ingress-nginx \
    --namespace ingress-nginx \
    --set controller.nodeSelector."kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."kubernetes\.io/os"=linux \
    --set controller.service.externalTrafficPolicy=Local \
    --set controller.service.loadBalancerIP=20.171.129.232

```

#  Context-based Ingress

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: gsg-first-ingress
  annotations: 
    kubernetes.io/ingress.class: nginx  
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
    - http:
        paths:
          - path: /hello
            pathType: Prefix
            backend:
              service:
                name: tutum-service
                port:
                  number: 80
          - path: /snake
            pathType: Prefix
            backend:
              service:
                name: snake-service
                port:
                  number: 8080

```

```
kubectl apply -f ingress.yaml
``` 


#  Host-based Ingress

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: gsg-first-ingress
  annotations: 
    kubernetes.io/ingress.class: nginx  
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
    - host: hello.gsg-example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: tutum-service
                port:
                  number: 80
    - host: snake.gsg-example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: snake-service
                port:
                  number: 8080
```

to perform a test without a real dns domain, add the domains to your hosts file 


```
sudo vi /etc/hosts

#add the following ( update to your ingress ip)

20.171.129.232  snake.gsg-example.com
20.171.129.232  hello.gsg-example.com
```

open your browser 

http://snake.gsg-example.com
http://hello.gsg-example.com

