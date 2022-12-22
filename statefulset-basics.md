# StatefulSet Basics

https://kubernetes.io/docs/tutorials/stateful-application/basic-stateful-set/

## example stateful.yml

````
apiVersion: v1
kind: Service
metadata:
  name: nginx
  namespace: test
  labels:
    app: nginx
spec:
  ports:
  - port: 80
    name: web
  # clusterIP: None
  selector:
    app: nginx
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
  namespace: test
spec:
  serviceName: "nginx"
  replicas: 4
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.22.0
        # image: registry.k8s.io/nginx-slim:0.8
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
      resources:
        requests:
          storage: 1Gi
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-test
  namespace: test
  labels:
    app: www
    env: test
spec:
  rules:
  - host: "app.inno.test"
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: nginx
            port:
              number: 80
````


## run 

````
kubectl apply -f stateful.yml
````

## write file 

````
kubectl exec -n test web-1 -- sh -c "echo 'hello web-1' > /usr/share/nginx/html/index.html"

kubectl exec -n test web-0 -- sh -c "echo 'hello web-0' > /usr/share/nginx/html/index.html"
````

## result

````
C:\minikube\deploy\test001>curl http://app.inno.test
hello web-1

C:\minikube\deploy\test001>curl http://app.inno.test
hello web-2
````

## result round robin

````
C:\minikube\deploy\test001>curl http://app.inno.test
hello web-1

C:\minikube\deploy\test001>curl http://app.inno.test
hello web-0

C:\minikube\deploy\test001>curl http://app.inno.test
<html>
<head><title>403 Forbidden</title></head>
<body>
<center><h1>403 Forbidden</h1></center>
<hr><center>nginx/1.22.0</center>
</body>
</html>

C:\minikube\deploy\test001>curl http://app.inno.test
<html>
<head><title>403 Forbidden</title></head>
<body>
<center><h1>403 Forbidden</h1></center>
<hr><center>nginx/1.22.0</center>
</body>
</html>

C:\minikube\deploy\test001>
````

## delete pod (but data persistent in per pod )

````
kubectl delete -f stateful.yml
````
