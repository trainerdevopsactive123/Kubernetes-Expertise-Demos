# Steps
## 1. Create a YAML File
Create a YAML file named "startup-example.yaml":
```sh
nano startup-example.yaml
```

## 2. Deploy the Pod
Apply the configuration to deploy the pod:
```sh
kubectl apply -f startup-example.yaml
```

## 3. Verify the Pod Status
Check the status of the pod to ensure it is running:
```sh
kubectl get pods
```

## 4. Observe the Startup Probe in Action
- The container will initially sleep for 30 seconds before creating the `/tmp/healthy` file. The startup probe will keep checking for this file.
- If the file is not found within the given period, the container will be restarted.
- Check the pod status again to see if it has started successfully:
```sh
kubectl get pods
```

The pod should start successfully after the initial delay and create the `/tmp/healthy` file, allowing the startup probe to pass. If the file is not created within the specified time, the container will be restarted.
```s