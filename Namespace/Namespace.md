# Namespaces

In Kubernetes, namespaces provides a mechanism for isolating groups of resources within a single cluster
Names of resources need to be unique within a namespace, but not across namespaces. 
Namespace-based scoping is applicable only for namespaced objects (e.g. Deployments, Services, etc) and not for cluster-wide objects (e.g. StorageClass, Nodes, PersistentVolumes, etc).


## Initial namespaces

- default
Kubernetes includes this namespace so that you can start using your new cluster without first creating a namespace.

- kube-node-lease
This namespace holds Lease objects associated with each node. Node leases allow the kubelet to send heartbeats so that the control plane can detect node failure.

- kube-public
This namespace is mostly reserved for cluster usage, in case that some resources should be visible and readable publicly throughout the whole cluster. 

- kube-system
The namespace for objects created by the Kubernetes system.

# View the namespace 
```
kubectl get namespace
```

## Create a Pod in default Namespace
```
kubectl run pod1 --image=nginx 
```
## Create a Namespace 
```
kubectl create namespace <namespacename>
kubectl create namespace alpha
```

## To verify namespace 
```
kubectl get namespace
```
## Create a Namespace with Yaml file 
### create a file `my-namespace.yaml` and paste the below content

```
apiVersion: v1
kind: Namespace
metadata:
  name: alpha
```
```
kubectl apply -f my-namespace.yaml
```

## Create a pod with namespace

```
kubectl run pod1 --image=nginx --namespace alpha
```

## Check all the pods in particular namespace

kubectl get pods --namespace <insert-namespace-name-here>

## Delete a namespace 
```
kubectl delete namespace <namespace-name>
```
## Setting the namespace preference

You can permanently save the namespace for all subsequent kubectl commands in that context.
```
kubectl config set-context --current --namespace=<insert-namespace-name-here>
```
# Validate it
```
kubectl config view --minify | grep namespace:
```
### To see which Kubernetes resources are and aren't in a namespace:

# In a namespace
```
kubectl api-resources --namespaced=true
```
# Not in a namespace
```
kubectl api-resources --namespaced=false
```
## below are not part of namespace, namespace cannot be implemented on these 


clusterroles                                   rbac.authorization.k8s.io/v1           false        ClusterRole

priorityclasses                   pc           scheduling.k8s.io/v1                   false        PriorityClass

csidrivers                                     storage.k8s.io/v1                      false        CSIDriver

csinodes                                       storage.k8s.io/v1                      false        CSINode

storageclasses                    sc           storage.k8s.io/v1                      false        StorageClass

volumeattachments                              storage.k8s.io/v1                      false        VolumeAttachmen