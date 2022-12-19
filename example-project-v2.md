# example project v2

## project-v2.yml

````
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: load-balancer-test
  name: hello-world-test
  namespace: test
spec:
  replicas: 2
  selector:
    matchLabels:
      app: load-balancer-test
  template:
    metadata:
      labels:
        app: load-balancer-test
    spec:
      containers:
      - image: nginx:1.22.0
        name: hello-world
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  labels:
    app: load-balancer-test
  name: service-test
  namespace: test
spec:
  allocateLoadBalancerNodePorts: true
  externalIPs:
  - 172.16.1.22
  externalTrafficPolicy: Cluster
  internalTrafficPolicy: Cluster
  ipFamilies:
  - IPv4
  ipFamilyPolicy: SingleStack
  ports:
  - nodePort: 32157
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: load-balancer-test
  sessionAffinity: None
  type: LoadBalancer
status:
  loadBalancer: {}
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-test
  namespace: test
spec:
  rules:
  - host: "foo.bar.local"
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: service-test
            port:
              number: 80
````

## run

````
[root@kube-server ok]# kubectl apply -f project-v2.yml
deployment.apps/hello-world-test created
service/service-test created
ingress.networking.k8s.io/ingress-test created
[root@kube-server ok]#
````

## result

````
[root@kube-server ok]# curl http://172.16.1.22
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
[root@kube-server ok]#
````
