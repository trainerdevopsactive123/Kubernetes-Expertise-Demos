# Get Worker Node Status
```bash
kubectl get nodes
```
# Get Worker Node Status with wide option
```
kubectl get nodes -o wide
```
# Create a pod 

# Imperative Code

Tell kubernetes step by step on how to create a pod 

# Template  
```
kubectl run <desired-pod-name> --image <Container-Image>

```
or 
```
kubectl run my-pod --image=nginx
```

# Declerative Approach 

Describe desire state of the pod, how it look like 
```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx
```
```
kubectl apply -f filename.yaml
```
# List Pods
```
kubectl get pods
```
# Alias name for pods is po
```
kubectl get po
```
# List pods with wide option which also provide Node information on which Pod is running
```
kubectl get pods -o wide
```
# Describe the Pod
```
kubectl describe pod <Pod-Name>
kubectl describe pod my-first-pod 
```
# To get list of pod names
```
kubectl get pods
```
# Delete Pod
```
kubectl delete pod <Pod-Name>
kubectl delete pod my-first-pod
```

