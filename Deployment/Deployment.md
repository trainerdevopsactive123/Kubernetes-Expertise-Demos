## Apply the deployment 
```
kubectl apply -f Deployment-1.yaml
```

## verify the pod 
```
kubectl get pod 
```

## List all the deployment 
```
kubectl get deployments nginx-deployment
# or shorthand
kubectl get deploy 
```

## Describe a Deployment
```
kubectl describe deploy <deployment-name>
```

## Edit a deployment 
```
kubectl edit deployment <deployment-name>
kubectl edit deployment nginx-deployment 
```

## Check deployment history 
```
kubectl rollout status deployment <deployment-name>
kubectl rollout status deployment nginx-deployment 
```

## check deployment changes 
```
kubectl rollout history deployment <deployment-name>
kubectl rollout history deployment nginx-deployment 
```

## Undo a deployment 
```
kubectl rollout undo deployment <deployment-name>
kubectl rollout undo deployment nginx-deployment 
```
## Update the image in deployment 
```
kubectl set image deployment/nginx-deployment nginx=nginx:1.16.1
```


## Move to particular revision 
```
kubectl rollout undo deployment nginx-deployment --to-revision=2
```

## Scale up the deployment 
```
kubectl scale deployment nginx-deployment --replica=10
```

## Delete a Deployment
```
kubectl delete deploy <deployment-name>
```
