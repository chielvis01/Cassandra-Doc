# StatefulSets

## Using StatefulSets

We need statefulSets for the following

- Unique network identifiers.
- Persistent storage.
- Deployments.
- Scaling.
- Rolling Updates.


## Cassandra Statefulsets Object Specs

1. `Pod Selector`
2. `Ordinal Index`
3. `Network ID`
4. `Pod Identity` 
5. `Pod Name Label` 
6. `Pod Management Policies`
7. `OrderedReady Pod Management`
8. `Parallel Pod Management`
9. `Update Strategies`
10. `On Delete`
11. `Rolling Updates`
12. `Forced Rollback`


```

apiVersion: v1
kind: Service
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  ports:
  - port: 80
    name: web
  clusterIP: None
  selector:
    app: nginx
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  selector:
    matchLabels:
      app: nginx # has to match .spec.template.metadata.labels
  serviceName: "nginx"
  replicas: 3 # by default is 1
  template:
    metadata:
      labels:
        app: nginx # has to match .spec.selector.matchLabels
    spec:
      terminationGracePeriodSeconds: 10
      containers:
      - name: nginx
        image: k8s.gcr.io/nginx-slim:0.8
        ports:
        - containerPort: 80
          name: web
        volumeMounts:
        - name: www
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: "my-storage-class"
      resources:
        requests:
          storage: 1Gi
          
```
