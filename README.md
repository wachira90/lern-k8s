# lern-k8s
lerning k8s

## check all 

````
kubectl get all
````
## test 

kubectl create ns develop

nano nginx-1.22.yml

````
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: develop
  labels:
    environment: develop
    app: nginx
    company: example1
spec:
  replicas: 2
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
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  namespace: develop
  labels:
    environment: develop
    app: nginx
    company: example1
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30080
````

## run 

````
kubectl apply -f nginx-1.22.yml
````

## mikube get url access

minikube service www-service --url -n develop

````
docker@gp65-leopard:~/kwww$ minikube service www-service --url -n develop
http://192.168.49.2:30002
docker@gp65-leopard:~/kwww$
````

## minikube images

````
minikube image pull wachira90/php:7.4

minikube image pull nginx:1.22.0

minikube image ls --format table
````
