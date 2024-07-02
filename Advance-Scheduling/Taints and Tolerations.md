### Step-by-Step Instructions to Demonstrate Taints and Tolerations in Kubernetes
### Get the node information 
```bash
kubectl get node
```
### Step 1: Taint any node in our case we use node01 Node
First, let's taint the `node01` node with the key `env` and value `production` with the `NoSchedule` effect:

```bash
kubectl taint nodes node01 env=production:NoSchedule
```

This means that any pods without a matching toleration will not be scheduled on the `node01` node.

### Step 2: Create a Pod without Any Tolerations
Let's create a simple nginx pod without any tolerations:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-no-toleration
spec:
  containers:
  - name: nginx
    image: nginx
```

Apply this pod definition:

```bash
kubectl apply -f nginx-no-toleration.yaml
```

You'll notice that this pod is not scheduled on the `node01` node, as it does not have a matching toleration.

### Step 3: Create a Pod with a Toleration
Now, let's create another nginx pod, but this time with a toleration that matches the taint on the `node01` node:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-with-toleration
spec:
  containers:
  - name: nginx
    image: nginx
  tolerations:
  - key: "env"
    operator: "Equal"
    value: "production"
    effect: "NoSchedule"
```

Apply this pod definition:

```bash
kubectl apply -f nginx-with-toleration.yaml
```

You'll notice that this pod is scheduled on the `node01` node, as it has a matching toleration.

### Step 4: Verify the Pod Placements
You can verify the pod placements using the following commands:

```bash
kubectl get pods -o wide
kubectl describe node node01
```

The output should show that the `nginx-no-toleration` pod is not scheduled on the `node01` node, while the `nginx-with-toleration` pod is scheduled on the `node01` node.

### Step 5: Remove the Taint from the node01 Node
Finally, let's remove the taint from the `node01` node:

```bash
kubectl taint nodes node01 env=production:NoSchedule-
```
This will allow pods without any tolerations to be scheduled on the `node01` node again.

