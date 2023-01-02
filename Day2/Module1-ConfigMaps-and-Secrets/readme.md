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

## File

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

