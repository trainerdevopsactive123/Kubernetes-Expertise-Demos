#### Node Information
- **Node Name**: minikube
- **Status**: Ready
- **Roles**: control-plane
- **Kubernetes Version**: v1.28.3

### Generating the "PodNotReady" Error

1. **Setup Kubernetes Cluster**:
   Ensure your Minikube cluster is running.

   ```bash
   minikube start
   ```

2. **Create a Pod YAML File with Readiness Probe Failure**:
   Create a YAML file for a pod that will deliberately fail the readiness probe, causing the `PodNotReady` status.

   Example `pod-not-ready.yaml`:

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: pod-not-ready
   spec:
     containers:
     - name: nginx
       image: nginx
       readinessProbe:
         httpGet:
           path: /ready
           port: 80
         initialDelaySeconds: 5
         periodSeconds: 5
   ```

   Save this file as `pod-not-ready.yaml`.

   In this example, the readiness probe is configured to check the `/ready` endpoint on port 80. Since the default Nginx image does not serve anything on `/ready`, the probe will fail.

3. **Apply the YAML File**:
   Create the pod using the YAML file.

   ```bash
   kubectl apply -f pod-not-ready.yaml
   ```

4. **Check Pod Status**:
   Verify the status of the pod. It should show the `PodNotReady` status.

   ```bash
   kubectl get pods
   ```

   The pod status may show as `Running` with the message `0/1` ready. To get more details:

   ```bash
   kubectl describe pod pod-not-ready
   ```

   You should see that the readiness probe is failing.

### Fixing the "PodNotReady" Issue

1. **Identify the Issue**:
   Use the `kubectl describe` command to understand why the readiness probe is failing. In this example, the issue is that the `/ready` endpoint does not exist.

   ```bash
   kubectl describe pod pod-not-ready
   ```

2. **Modify the Readiness Probe Configuration**:
   Edit the pod YAML file to correct the readiness probe. For example, use a path that exists or create a custom Nginx configuration to serve the `/ready` endpoint.

   Updated `pod-not-ready.yaml` with a correct path:

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: pod-not-ready
   spec:
     containers:
     - name: nginx
       image: nginx
       readinessProbe:
         httpGet:
           path: /
           port: 80
         initialDelaySeconds: 5
         periodSeconds: 5
   ```

   Alternatively, create a custom Nginx configuration that serves the `/ready` endpoint.

3. **Delete the Existing Pod**:
   Delete the existing pod that is in the `PodNotReady` state.

   ```bash
   kubectl delete pod pod-not-ready
   ```

4. **Reapply the Modified YAML File**:
   Create the pod again with the corrected readiness probe configuration.

   ```bash
   kubectl apply -f pod-not-ready.yaml
   ```

5. **Verify Pod Status**:
   Check the status of the pod to ensure it is now ready.

   ```bash
   kubectl get pods
   ```

   The pod should now show as `1/1` ready.