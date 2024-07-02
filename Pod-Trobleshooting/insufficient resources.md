### Step-by-Step Instructions to Generate and Fix "Insufficient Resources" Error in Kubernetes

#### Node Information

- **Node Name**: minikube
- **Status**: Ready
- **Roles**: control-plane
- **Kubernetes Version**: v1.28.3

#### Generating the "Insufficient Resources" Error

1. **Setup Kubernetes Cluster**:
   Ensure your Minikube cluster is running if not start with below command.

   ```bash
   minikube start
   ```

2. **Check Available Resources**:
   Before creating a pod, check the available resources on your Minikube node.

   ```bash
   kubectl describe node minikube
   ```

   Look for the `Allocatable` section to see the available CPU and memory resources.

3. **Create a Pod YAML File with High Resource Requests**:
   Create a YAML file for a pod that requests more resources than available on your Minikube node.

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: high-resource-pod
   spec:
     containers:
     - name: high-resource-container
       image: nginx
       resources:
         requests:
           memory: "10Gi"
           cpu: "8"
   ```

   Save this file as `high-resource-pod.yaml`.

4. **Apply the YAML File**:
   Create the pod using the YAML file.

   ```bash
   kubectl apply -f high-resource-pod.yaml
   ```

5. **Check Pod Status**:
   Verify the status of the pod. It should show an error indicating insufficient resources.

   ```bash
   kubectl get pods
   ```

   The pod status may show as `Pending`. To get more details:

   ```bash
   kubectl describe pod high-resource-pod
   ```

   You should see an error message similar to "0/1 nodes are available: Insufficient memory."

#### Fixing the "Insufficient Resources" Error

1. **Modify Resource Requests**:
   Edit the pod YAML file to request fewer resources. For example:

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: high-resource-pod
   spec:
     containers:
     - name: high-resource-container
       image: nginx
       resources:
         requests:
           memory: "512Mi"
           cpu: "0.5"
   ```

   Save the changes to the `high-resource-pod.yaml` file.

2. **Delete the Existing Pod**:
   Delete the existing pod that is in the `Pending` state.

   ```bash
   kubectl delete pod high-resource-pod
   ```

3. **Reapply the Modified YAML File**:
   Create the pod again with the modified resource requests.

   ```bash
   kubectl apply -f high-resource-pod.yaml
   ```

4. **Verify Pod Status**:
   Check the status of the pod to ensure it is running.

   ```bash
   kubectl get pods
   ```

   The pod should now be in the `Running` state.
