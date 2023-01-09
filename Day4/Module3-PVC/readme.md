# PVC

# create a disk pvc

```
kubectl create -f pvc-disk.yaml

```
check the status 

```
kubectl get pvc -w
```


## usefull command 

which pod is using my pvc
```
kubectl get po -o json --all-namespaces | jq -j '.items[] | "\(.metadata.namespace), \(.metadata.name), \(.spec.volumes[].persistentVolumeClaim.claimName)\n"' | grep -v null

```


