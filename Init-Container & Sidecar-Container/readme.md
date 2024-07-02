# Init-Container Steps 

### 1. Creating the Deployment and Service:

### Create the Deployment
```sh
kubectl apply -f Init_container_deployment.yaml
```

### Create the Service
```sh
kubectl apply -f init_service.yaml
```

### Verifying Deployment and Service:
```sh
kubectl get deployments
```

### Check Service Details:

```sh
kubectl get services
```

### Accessing the Application:
Replace <NODE_IP> with the actual IP address of a worker node in your cluster
```sh
kubectl get nodes -o jsonpath="{.items[0].status.addresses[?(@.type=='ExternalIP')].address}"
```

### Using kubectl proxy:

```sh
kubectl proxy
```

### Checking Logs:

Get the pod name
```sh
kubectl get pods
```


### View logs for the init container and main container:
```sh
kubectl logs -p example-pod init-myservice # Logs for init container
kubectl logs example-pod myservice      # Logs for main container
kubectl logs deployment/example-pod
```


# Sidecar-Container Steps 

### Create the Pod:
```sh
kubectl apply -f pod.yaml
```
___Explanation:___
Replace pod.yaml with the actual filename where you've saved the provided YAML configuration.
The kubectl apply command creates resources in your Kubernetes cluster based on the definitions in the YAML file.

### Verify the Pod's Status:

```sh
kubectl get pods
```
___Explanation:___
This command retrieves a list of pods in your cluster. Look for the pod named nginx-pod and ensure its status is Running and has 1/1 ready replicas.

### Check Logs:
```sh
kubectl logs nginx-pod nginx
```
___Explanation:___
This command retrieves and displays logs specifically for the nginx container within the nginx-pod.

### Sidecar Container (busybox):
```sh
kubectl logs nginx-pod sidecar
```
___Explanation:___
This command retrieves and displays logs specifically for the busybox sidecar container within the nginx-pod.