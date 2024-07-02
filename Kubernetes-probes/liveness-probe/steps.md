# Steps to Use HTTP GET Liveness Probe

### 1. Create a YAML File
Create a YAML file named "httpGet-example.yaml":

```sh
nano httpGet-example.yaml
```


### 2. Deploy the Pod
Apply the configuration to deploy the pod:
```sh
kubectl apply -f httpGet-example.yaml
```

###  3. Verify the Pod Status
Check the status of the pod to ensure it is running:
```sh
kubectl get pods
```

###  4. Simulate a Failure
Simulate a failure by deleting the default `index.html` file that `nginx` serves:
```sh
kubectl exec -it liveness-demo -- /bin/sh
rm /usr/share/nginx/html/index.html
exit
```

###  5. Check Pod Status After Failure
After a short while, check the pod status again to see if it has restarted:
```sh
kubectl get pods
```

# Steps to Use Exec Liveness Probe

### 1. Create a YAML File
Create a YAML file named "exec-example.yaml":
```sh
nano exec-example.yaml
```

###  2. Deploy the Pod
Apply the configuration to deploy the pod:
```sh
kubectl apply -f exec-example.yaml
```

###  3. Verify the Pod Status
Check the status of the pod to ensure it is running:
```sh
kubectl get pods
```

###  4. Observe the Liveness Probe in Action
After 30 seconds, the `/tmp/healthy` file will be deleted, causing the liveness probe to fail. This will trigger a restart of the container. Check the pod status again to see if it has restarted:
```sh
kubectl get pods
```
```