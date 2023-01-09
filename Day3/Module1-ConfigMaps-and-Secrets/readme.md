# ConfigMaps

There are two types of configurations: 1- environment variables, 2- configuration files

## Environment variables 
example: 
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: myapp-config-map
data:
  environment: dev
  log_level: INFO

```
Applications access environment variables from os.env methods 

You need to reference vars in deployment 

```yaml
          env:
            - name: environment
              valueFrom:
                configMapKeyRef:
                  name: myapp-config-map
                  key: environment
```

## File
example
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: myapp-config-map-file
data:
  app.config: |
    database-name: mydb
    database-port: 3306
    database-host: mysql-server
```
Applications access configurations by reading file on disk


you need to reference the configuration file on disk

```yaml

      containers:
        - name: myapp
          image: gsalahacr.azurecr.io/gsg-bootcamp-service:v1
          ports:
            - containerPort: 80
          volumeMounts:
            - name: config-volume
              mountPath: /etc/config
      volumes:
        - name: config-volume
          configMap:
            name: myapp-config-map-file

```



# Reloading

* Reference: https://github.com/stakater/Reloader

installation 
```
kubectl apply -k https://github.com/stakater/Reloader/deployments/kubernetes
```


You need to following annotations to your deployment

```
annotations:
    reloader.stakater.com/search: "true"
    
```