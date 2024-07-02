# Comments

```yaml
# Name: Vicky
Class: Devops
City: Delhi
```

### Example

```yaml
apiVersion: v1 # Specifies the version of the Kubernetes API
kind: Pod # Indicates that this YAML file defines a Pod
metadata:
  name: nginx-pod # The name of the Pod
  labels:
    app: nginx # Label to categorize the Pod
spec:
  containers:
    - name: nginx # The name of the container
      image: nginx:latest # The container image to run
      ports:
        - containerPort: 80 # The port on which the container will listen for incoming connections
```
