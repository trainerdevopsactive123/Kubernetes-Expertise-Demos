# Create Pod with Node Affinity

_Node affinity is used to ensure that a pod is scheduled on a node that meets specific criteria. It is based on node labels._

### Ensure your Kubernetes cluster is up and running. You should have at least two nodes: one control-plane node and one worker node.

```bash
kubectl get nodes
```

## Define Node Labels

```bash
kubectl label nodes controlplane size=large
kubectl label nodes node01 size=small
```

## Create Pod with Node Affinity

Create a YAML file for a pod that uses node affinity. This pod will only run on nodes labeled with `size=large`.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: node-affinity
spec:
  containers:
  - name: nginx
    image: nginx
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: In    # You can use In, NotIn, Exists, DoesNotExist, Gt and Lt.
            values:
            - large
```

### Save the file as `node-affinity.yaml` and apply it

```bash
kubectl apply -f node-affinity.yaml
```

### Verify Pod with Node Affinity

```bash
kubectl get pods -o wide
```

#### You should see `node-affinity` scheduled on the controlplane node.

# Create Pod with Pod Affinity

### Useful in scenarios like:

- Where you want specific types of pod to run together.
- Where frontend and backend pods run together to reduce latency.
- Generally for pods which run together for high availability.

### Co-locating Frontend and Backend Pods

We need to ensure that frontend pods are scheduled on the same node as backend pods to reduce latency between them.

save the content in **backend.yaml** file 

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: backend-pod
  labels:
    app: backend
spec:
  containers:
  - name: backend
    image: nginx
```

save the content in **frontend.yaml** file 

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: frontend-pod
  labels:
    app: frontend
spec:
  containers:
  - name: frontend
    image: nginx
  affinity:
    podAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchLabels:
            app: backend
        topologyKey: "kubernetes.io/hostname"
```

### Apply the configurations

```bash
kubectl apply -f backend.yaml
kubectl apply -f frontend.yaml
```

### To verify

```bash
kubectl get pods -o wide
```

# Create Pod with Node Anti-Affinity

_Node anti-affinity is used to ensure that a pod is not scheduled on a node with specific labels._

### Ensure your Kubernetes cluster is up and running. You should have at least two nodes: one control-plane node and one worker node.

```bash
kubectl get nodes
```

## Define Node Labels

```bash
kubectl label nodes node01 disallow=scheduling
```

## Create Pod with Node Anti-Affinity

Create a YAML file for a pod that uses node anti-affinity. This pod will not run on nodes labeled with `disallow=scheduling`.

Save the file as `pod-with-node-anti-affinity.yaml`.

**pod-with-node-anti-affinity.yaml**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-with-node-anti-affinity
spec:
  containers:
  - name: nginx
    image: nginx
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: disallow
            operator: NotIn
            values:
            - scheduling
```

### Apply the configuration

```bash
kubectl apply -f pod-with-node-anti-affinity.yaml
```

### Verify the Pod Placement

```bash
kubectl get pods -o wide
```

This should ensure that `pod-with-node-anti-affinity` is not scheduled on the node labeled with `disallow=scheduling`.