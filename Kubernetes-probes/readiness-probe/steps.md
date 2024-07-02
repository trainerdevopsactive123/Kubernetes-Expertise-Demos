# Steps to Use Readiness Probe

## 1. Create a YAML File
Create a YAML file named "readiness-example.yaml":
```sh
nano readiness-example.yaml
```

## 2. Deploy the Pod
Apply the configuration to deploy the pod:
```sh
kubectl apply -f readiness-example.yaml
```

## 3. Verify the Pod Status
Check the status of the pod to ensure it is running:
```sh
kubectl get pods
```

## 4. Simulate a Readiness Probe Failure
Simulate a failure by deleting the default `index.html` file that `nginx` serves:
```sh
kubectl exec -it readiness-demo -- /bin/sh
rm /usr/share/nginx/html/index.html
exit
```

## 5. Check Pod Status After Failure
Check the pod status again to see if it is no longer marked as ready:
```sh
kubectl get pods
```

The pod should still be running, but it will not be marked as ready. The application will not receive traffic until it becomes ready again.
```