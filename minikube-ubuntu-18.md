# minikube ubuntu 18.04

````
apt -y install qemu-kvm libvirt-bin virtinst bridge-utils

apt -y install apt-transport-https
 
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | tee -a /etc/apt/sources.list.d/kubernetes.list

apt update

apt -y install kubectl

wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 -O minikube

wget https://storage.googleapis.com/minikube/releases/latest/docker-machine-driver-kvm2

chmod 755 minikube docker-machine-driver-kvm2

mv minikube docker-machine-driver-kvm2 /usr/local/bin/

minikube version

kubectl version -o json

minikube start --vm-driver kvm2

# OR
minikube start

minikube status

minikube service list

minikube docker-env

kubectl cluster-info

kubectl get nodes
````
