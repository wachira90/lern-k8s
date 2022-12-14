# namespace

## get 

````
kubectl get namespaces

kubectl create namespace develop
````


## example

````
[root@kube-server mappath]# kubectl create namespace develop
namespace/develop created
[root@kube-server mappath]#
````

## yaml 

````
apiVersion: v1
kind: Namespace
metadata:
    name: develop
````

## example

````
kubectl run ns-pod --image=nginx:1.22.0 --port=80 --generator=run-pod/v1 -n develop
````
