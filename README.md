# lern-k8s
lerning k8s

## check all 

````
kubectl get all
````
## test 

nano nginx-1.22.yml

````
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
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
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30002
````      
