```markdown
## Create Pod and Service

```sh
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

## NodePort Range

The NodePort range is between 30000 and 32767.

## Login to Minikube and Check Connectivity

```sh
minikube ssh
curl http://localhost:30007
```

## Clean Up

To clean up the resources, run the following commands:

```sh
kubectl delete -f deployment.yaml
kubectl delete -f service.yaml
```
```