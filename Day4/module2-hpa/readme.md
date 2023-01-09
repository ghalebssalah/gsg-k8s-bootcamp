# Horizontal Pod Autoscaler (HPA)




## 1. Create deployment and service

```
kubectl apply -f tutum-deployment.yaml
kubectl apply -f service.yaml
```

```
kubectl get deployment 


kubectl autoscale deployment tutum --cpu-percent=20 --min=1 --max=10
```

or using yaml file

```
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: tutum-hpa
spec:
  maxReplicas: 10 # define max replica count
  minReplicas: 1  # define min replica count
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: tutum
  targetCPUUtilizationPercentage: 20 # target CPU utilization

```

lets see do some load

```
kubectl run -i --tty load-generator --rm --image=busybox:1.28 --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://tutum-service; done"

```

watch it :) 

```
kubectl get pod -w 

kubectl get hpa -w

```