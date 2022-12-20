# security

https://kubernetes.io/docs/tutorials/security/cluster-level-pss/

````
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.17.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
````

````
kind create cluster --name psa-wo-cluster-pss --image kindest/node:v1.24.0

kubectl cluster-info --context kind-psa-wo-cluster-pss

kubectl get ns

kubectl label --dry-run=server --overwrite ns --all  pod-security.kubernetes.io/enforce=privileged

kubectl label --dry-run=server --overwrite ns --all pod-security.kubernetes.io/enforce=baseline

kubectl label --dry-run=server --overwrite ns --all pod-security.kubernetes.io/enforce=restricted
````

````
mkdir -p /tmp/pss
cat <<EOF > /tmp/pss/cluster-level-pss.yaml 
apiVersion: apiserver.config.k8s.io/v1
kind: AdmissionConfiguration
plugins:
- name: PodSecurity
  configuration:
    apiVersion: pod-security.admission.config.k8s.io/v1
    kind: PodSecurityConfiguration
    defaults:
      enforce: "baseline"
      enforce-version: "latest"
      audit: "restricted"
      audit-version: "latest"
      warn: "restricted"
      warn-version: "latest"
    exemptions:
      usernames: []
      runtimeClasses: []
      namespaces: [kube-system]
EOF
````

````
cat <<EOF > /tmp/pss/cluster-config.yaml 
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  kubeadmConfigPatches:
  - |
    kind: ClusterConfiguration
    apiServer:
        extraArgs:
          admission-control-config-file: /etc/config/cluster-level-pss.yaml
        extraVolumes:
          - name: accf
            hostPath: /etc/config
            mountPath: /etc/config
            readOnly: false
            pathType: "DirectoryOrCreate"
  extraMounts:
  - hostPath: /tmp/pss
    containerPath: /etc/config
    # optional: if set, the mount is read-only.
    # default false
    readOnly: false
    # optional: if set, the mount needs SELinux relabeling.
    # default false
    selinuxRelabel: false
    # optional: set propagation mode (None, HostToContainer or Bidirectional)
    # see https://kubernetes.io/docs/concepts/storage/volumes/#mount-propagation
    # default None
    propagation: None
EOF
````


````
kind create cluster --name psa-with-cluster-pss --image kindest/node:v1.24.0 --config /tmp/pss/cluster-config.yaml

kubectl cluster-info --context kind-psa-with-cluster-pss
````

## Test
````
cat <<EOF > /tmp/pss/nginx-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
    - image: nginx
      name: nginx
      ports:
        - containerPort: 80
EOF
````

````
kubectl apply -f /tmp/pss/nginx-pod.yaml
````

### The output is similar to this:
````
 Warning: would violate PodSecurity "restricted:latest": allowPrivilegeEscalation != false (container "nginx" must set securityContext.allowPrivilegeEscalation=false), unrestricted capabilities (container "nginx" must set securityContext.capabilities.drop=["ALL"]), runAsNonRoot != true (pod or container "nginx" must set securityContext.runAsNonRoot=true), seccompProfile (pod or container "nginx" must set securityContext.seccompProfile.type to "RuntimeDefault" or "Localhost")
 pod/nginx created
````

## Clean up

````
kind delete cluster --name psa-with-cluster-pss

kind delete cluster --name psa-wo-cluster-pss
````
