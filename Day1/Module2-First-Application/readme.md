# Very Basic deployment

## Create your first pod

```
kubectl run my-first-pod --image  tutum/hello-world
```

## Access your pod's poage

```
kubectl port-forward pod/my-first-pod 8080:80
```

## Access your pod's shell

```
kubectl exec -ti my-first-pod -- /bin/sh
```

# Deployment using YAML files

## apply you tutum deployment and service

```
kubectl apply -f tutum-deployment.yaml
kubectl apply -f tutum-service.yaml
```

## Access your pod's poage

```
kubectl port-forward svc/tutum 8080:80
```