# INSTALL KUBERNET ON WINDOWS

## create folder by powershell run as admin

````
New-Item -Path 'c:\' -Name 'minikube' -ItemType Directory -Force
````

## download  minikube.exe

````
Invoke-WebRequest -OutFile 'c:\minikube\minikube.exe' -Uri 'https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' -UseBasicParsing
````

# ADD PATH ENV 

````
C:\minikube\

D:\Program Files (x86)\VMware\VMware Workstation
````

## check hash

````
curl.exe -LO "https://dl.k8s.io/release/v1.25.0/bin/windows/amd64/kubectl.exe"

curl.exe -LO "https://dl.k8s.io/v1.25.0/bin/windows/amd64/kubectl.exe.sha256"
````

## check hash

````
CertUtil -hashfile kubectl.exe SHA256
type kubectl.exe.sha256
````



## download kubectl-convert.exe

````
curl.exe -LO "https://dl.k8s.io/release/v1.25.0/bin/windows/amd64/kubectl-convert.exe"

curl.exe -LO "https://dl.k8s.io/v1.25.0/bin/windows/amd64/kubectl-convert.exe.sha256"
````

## download docker-machine-driver-vmware.exe

````
https://github.com/machine-drivers/docker-machine-driver-vmware/releases

https://github.com/machine-drivers/docker-machine-driver-vmware/releases/download/v0.1.5/docker-machine-driver-vmware_0.1.5_windows_amd64.tar.gz
````

## start command 

````
minikube start --driver vmware
````

## result

````
C:\minikube>minikube start --driver vmware
* minikube v1.28.0 on Microsoft Windows 10 Pro 10.0.19045 Build 19045
* Using the vmware driver based on user configuration
* Downloading VM boot image ...
    > minikube-v1.28.0-amd64.iso....:  65 B / 65 B [---------] 100.00% ? p/s 0s
    > minikube-v1.28.0-amd64.iso:  274.45 MiB / 274.45 MiB  100.00% 14.68 MiB p
* Starting control plane node minikube in cluster minikube
* Downloading Kubernetes v1.25.3 preload ...
    > preloaded-images-k8s-v18-v1...:  385.44 MiB / 385.44 MiB  100.00% 15.88 M
* Creating vmware VM (CPUs=2, Memory=4000MB, Disk=20000MB) ...
* Preparing Kubernetes v1.25.3 on Docker 20.10.20 ...
  - Generating certificates and keys ...
  - Booting up control plane ...
  - Configuring RBAC rules ...
* Verifying Kubernetes components...
  - Using image gcr.io/k8s-minikube/storage-provisioner:v5
* Enabled addons: storage-provisioner, default-storageclass
* Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default

C:\minikube>
````

## check status command

````
kubectl version --client

kubectl cluster-info

kubectl version --client --output=yaml

kubectl version --client --output=json
````


# https://kubernetes.io/docs/tutorials/hello-minikube/
````
kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.39 -- /agnhost netexec --http-port=8080

kubectl get deployments

kubectl get events

kubectl config view
````


````
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
````

# start

````
minikube start --driver=hyperv

minikube start --vm-driver kvm2 --disk-size 20GB
````

### https://kubernetes.io/docs/tutorials/hello-minikube/

````
kubectl delete service hello-node
kubectl delete deployment hello-node


minikube dashboard --url

minikube stop

minikube status

minikube delete
````





## dashboard

````
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.6.1/aio/deploy/recommended.yaml

kubectl proxy

# process infolder dashboard / user 
kubectl apply -f ./

kubectl -n kubernetes-dashboard create token admin-user
````



````
kubectl get pods

kubectl describe pods hello-node

kubectl get pods --field-selector=status.phase=Pending

kubectl uncordon  hello-node

kubectl uncordon hello-node-67949d9db-stknz

kubectl describe node 

kubectl get events

kubectl expose deployment hello-node --type=LoadBalancer --port=8080

kubectl get pod,svc -n kube-system

# DELETE
kubectl delete service hello-node
kubectl delete deployment hello-node

kubectl get pods --namespace=kube-system

kubectl get svc -A
````





