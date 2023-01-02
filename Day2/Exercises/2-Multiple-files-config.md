# You got a request to create multiple file configuration files on disk 

1- first file

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mydb-config-file
data:
  db.config: |
    database-name: mydb
    database-port: 3306
    database-host: mysql-server
```

2- second file 

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: kafka-config-file
data:
  kafka.config: |
    kafka-host: myKafaka-broker
    kafka-port: 9092
```