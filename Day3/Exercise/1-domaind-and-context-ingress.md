
# Create an ingress to perorm following routings


apps.gsg-example.com/ -> runs the docker image nginxdemos/hello
apps.gsg-example.com/tutum -> runs the app tutum/hello-world application
apps.gsg-example.com/hello -> runs the app paulbouwer/hello-kubernetes
snake.gsg-example.com -> runs snake application


deployment details:


* nginxdemos/hello

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: default-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: default
  template:
    metadata:
      labels:
        app: default
    spec:
      containers:
        - name: app1
          image: bhargavshah86/kube-test:v0.1
          ports:
            - containerPort: 80
```

* tutum/hello-world

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: tutum
spec:
  replicas: 1
  selector:
      matchLabels:
        app: tutum
  template:
    metadata:
        labels:
          app: tutum
    spec:
      containers:
        - name: hello-world
          image: tutum/hello-world
          ports:
            - containerPort: 80

```

* paulbouwer/hello-kubernetes 


```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello
spec:
  replicas: 1
  selector:
      matchLabels:
        app: hello
  template:
    metadata:
        labels:
          app: hello
    spec:
      containers:
        - name: hello
          image: paulbouwer/hello-kubernetes
          ports:
            - containerPort: 8080

```


* snake

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: snake-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: snake
  template:
    metadata:
      labels:
        app: snake
    spec:
      containers:
        - name: app1
          image: bhargavshah86/kube-test:v0.1
          ports:
            - containerPort: 80

```