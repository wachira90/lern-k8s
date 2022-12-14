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
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  replicas: 2
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
  name: nginx-client-service
  namespace: develop
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30002
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


