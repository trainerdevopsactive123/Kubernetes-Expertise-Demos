## Setting Resource Requests and Limits in a Kubernetes Pod

### Step 1: Create a YAML File

First, create a YAML file to define your Pod. Let's call it `pod-resources.yaml`.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: resource-limits-pod
spec:
  containers:
  - name: my-container
    image: nginx:latest
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```

### Step 2: Apply the YAML File

Use the `kubectl apply` command to create the Pod with the resource requests and limits.

```sh
kubectl apply -f pod-resources.yaml
```

### Step 3: Verify the Pod Creation

Check if the Pod is created and running with the specified resource requests and limits.

```sh
kubectl get pod resource-limits-pod
```

### Step 4: Describe the Pod

To verify that the resource requests and limits have been applied, use the `kubectl describe pod` command.

```sh
kubectl describe pod resource-limits-pod
```

Look for the `Containers` section in the output, which should display the resource requests and limits you set:

```plaintext
Containers:
  my-container:
    Container ID:  docker://<container_id>
    Image:         nginx:latest
    Image ID:      docker-pullable://nginx@<image_id>
    Port:          <none>
    Host Port:     <none>
    State:         Running
      Started:     <timestamp>
    Ready:         True
    Restart Count: 0
    Requests:
      cpu:      250m
      memory:   64Mi
    Limits:
      cpu:      500m
      memory:   128Mi
```


### Step 5: Describe the Node

Check resource requests and limits from node to verify correct limit has been assigned.

```sh
kubectl describe node <nodename>
```

```plaintext
Non-terminated Pods:          (11 in total)
  Namespace                   Name                                        CPU Requests  CPU Limits  Memory Requests  Memory Limits  Age
  ---------                   ----                                        ------------  ----------  ---------------  -------------  ---
  default                     resource-limits-pod                         250m (3%)     500m (6%)   64Mi (1%)        128Mi (3%)     70s
  kube-system                 calico-kube-controllers-558d465845-vwnt7    0 (0%)        0 (0%)      0 (0%)           0 (0%)         27d
  kube-system                 calico-node-s72z5                           250m (3%)     0 (0%)      0 (0%)           0 (0%)         27d
  kube-system                 coredns-5dd5756b68-58624                    100m (1%)     0 (0%)      70Mi (1%)        170Mi (4%)     27d
  kube-system                 etcd-minikube                               100m (1%)     0 (0%)      100Mi (2%)       0 (0%)         27d
  kube-system                 kube-apiserver-minikube                     250m (3%)     0 (0%)      0 (0%)           0 (0%)         27d
  kube-system                 kube-controller-manager-minikube            200m (2%)     0 (0%)      0 (0%)           0 (0%)         27d
  kube-system                 kube-proxy-ckf2l                            0 (0%)        0 (0%)      0 (0%)           0 (0%)         27d
  kube-system                 kube-scheduler-minikube                     100m (1%)     0 (0%)      0 (0%)           0 (0%)         27d
  kube-system                 metrics-server-7c66d45ddc-8wpwh             100m (1%)     0 (0%)      200Mi (5%)       0 (0%)         25h
  kube-system                 storage-provisioner                         0 (0%)        0 (0%)      0 (0%)           0 (0%)         27d
```