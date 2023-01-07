# Cron Job

Create a kubernetes cronjob runs every 5 minutes and runs whalesay job

Reference: https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/cron-job-v1/


the job needs to run

```
apiVersion: batch/v1
kind: Job
metadata:
  name: whalesay
spec:
  template:
    spec:
      containers:
      - name: whalesay
        image: docker/whalesay
        command: ["cowsay",  "This is a Kubernetes Job!"]

```