### 1. Create a Deployment
Create a deployment here we'll use a Nginx server.

Create a file named `nginx-deployment.yaml` and add the below content:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

Apply the deployment by running command:

```bash
kubectl apply -f nginx-deployment.yaml
```

### 2. Create a LoadBalancer Service
Create a LoadBalancer service

Create a file named `nginx-service.yaml` and add the below content:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 80
  selector:
    app: nginx
```

Apply the service using the following command:

```bash
kubectl apply -f nginx-service.yaml
```

### 3. Verify the Deployment and Service
Check the status of your deployment and service:

```bash
kubectl get deployments
kubectl get pods
kubectl get services
```

You should see the Nginx pods running and a service with an external IP.

### 4. Access the Application
Once the LoadBalancer service is created, it may take a few minutes for AWS to provision an external IP. Check the external IP of the service using:

```bash
kubectl get services
```

Access the application by navigating to the external IP in your web browser.


## Clean Up

To clean up the resources, run the following commands:

```sh
kubectl delete -f nginx-service.yaml
kubectl delete -f nginx-deployment.yaml
```
```