# example project

## ingress.yml

````
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-service
  namespace: develop
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/use-regex: "true"
spec:
  rules:
    - host: app.inno.local
      http:
        paths:
          - path: /api/
            pathType: Prefix
            backend:
              service:
                name: api-service
                port:
                  number: 80
          - path: /?(.*)
            pathType: Prefix
            backend:
              service:
                name: web-service
                port:
                  number: 80
````

## web.yml

````
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-deployment
  namespace: develop
spec:
  replicas: 2
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      name: web-pods
      labels:
        app: web
    spec:
      containers:
        - name: web
          image: nginx:1.22.0
---
apiVersion: v1
kind: Service
metadata:
  name: web-service
  namespace: develop
spec:
  type: ClusterIP
  selector:
    app: web
  ports:
    - port: 80
      targetPort: 80
````

## api.yml

````
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-deployment
  namespace: develop
spec:
  replicas: 2
  selector:
    matchLabels:
      app: api
  template:
    metadata:
      name: api-pods
      labels:
        app: api
    spec:
      containers:
        - name: api
          image: nginx:1.22.0
---
apiVersion: v1
kind: Service
metadata:
  name: api-service
  namespace: develop
spec:
  type: ClusterIP
  selector:
    app: api
  ports:
    - port: 80
      targetPort: 80
````
