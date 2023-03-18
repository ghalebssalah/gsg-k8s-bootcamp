# PVC

# create a disk pvc

```
kubectl create -f pvc-disk.yaml

```
check the status 

```
kubectl get pvc -w
kubectl get pv
```


## usefull command 

which pod is using my pvc
```
kubectl get pod -o json --all-namespaces | jq -j '.items[] | "\(.metadata.namespace), \(.metadata.name), \(.spec.volumes[].persistentVolumeClaim.claimName)\n"' | grep -v null

```


