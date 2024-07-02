## Deploy the replicaset 
```
kubectl apply -f replica-1.yaml
```

##  List All ReplicaSets
```
kubectl get replicasets
# or shorthand
kubectl get rs
```

## Describe a ReplicaSet
```
kubectl describe rs <replicaset-name>
```

## Delete a ReplicaSet
```
kubectl delete rs <replicaset-name>
```

## Scale a ReplicaSet Manually
```
kubectl scale rs <replicaset-name> --replicas=<number-of-replicas>
```

## Edit a ReplicaSet
```
kubectl edit rs <replicaset-name>
```

## Rollout History of a ReplicaSet
```
kubectl rollout history rs/<replicaset-name>
```

## Rollback a ReplicaSet
```
kubectl rollout undo rs/<replicaset-name>
```
