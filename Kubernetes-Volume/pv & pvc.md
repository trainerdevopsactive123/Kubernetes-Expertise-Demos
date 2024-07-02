#### Overview

A PersistentVolume (PV) in Kubernetes is a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes. PersistentVolumeClaims (PVCs) are requests for those storage resources. This guide demonstrates how to create and use a PersistentVolume and PersistentVolumeClaim in Kubernetes.

#### Create a PersistentVolume (PV)

   Create a YAML file named `pv-example.yaml` with the following content:

   ```yaml
   apiVersion: v1
   kind: PersistentVolume
   metadata:
     name: pv-example
   spec:
     capacity:
       storage: 1Gi
     accessModes:
       - ReadWriteOnce
     persistentVolumeReclaimPolicy: Retain
     hostPath:
       path: /mnt/data
   ```

   This YAML defines a PersistentVolume named `pv-example` with a capacity of 1Gi, an access mode of `ReadWriteOnce`, and a reclaim policy of `Retain`. The volume is backed by the `/mnt/data` directory on the host node.

   Apply the YAML file to create the PersistentVolume:

   ```sh
   kubectl apply -f pv-example.yaml
   ```

#### Create a PersistentVolumeClaim (PVC)

   Create a YAML file named `pvc-example.yaml` with the following content:

   ```yaml
   apiVersion: v1
   kind: PersistentVolumeClaim
   metadata:
     name: pvc-example
   spec:
     accessModes:
       - ReadWriteOnce
     resources:
       requests:
         storage: 1Gi
   ```

   This YAML defines a PersistentVolumeClaim named `pvc-example` that requests 1Gi of storage with an access mode of `ReadWriteOnce`.

   Apply the YAML file to create the PersistentVolumeClaim:

   ```sh
   kubectl apply -f pvc-example.yaml
   ```

#### Create a Pod that uses the PVC

   Create a YAML file named `pod-example.yaml` with the following content:

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: pvc-example
   spec:
     containers:
     - name: my-container
       image: busybox
       command: ['sh', '-c', 'ls /mnt/data && sleep 3600']
       volumeMounts:
       - mountPath: /mnt/data
         name: pvc-storage
     volumes:
     - name: pvc-storage
       persistentVolumeClaim:
         claimName: pvc-example
   ```

   This YAML defines a Pod named `pvc-example` that uses the `busybox` image. It runs a command to list the contents of the `/mnt/data` directory and then sleeps for an hour. The volume is mounted at `/mnt/data` and is backed by the `pvc-example` PersistentVolumeClaim.

   Apply the YAML file to create the Pod:

   ```sh
   kubectl apply -f pod-example.yaml
   ```

####  Verify the resources

   Check the status of the PersistentVolume:

   ```sh
   kubectl get pv
   ```

   Check the status of the PersistentVolumeClaim:

   ```sh
   kubectl get pvc
   ```

   Check the status of the Pod:

   ```sh
   kubectl get pods
   ```

   You should see the `pv-example` PersistentVolume bound to the `pvc-example` PersistentVolumeClaim, and the `pvc-example` Pod in the `Running` state.

####  Inspect the Pod and the volume**

   To inspect the volume and check the contents of the directory mounted from the PersistentVolume, use `kubectl exec` to open a shell inside the container:

   ```sh
   kubectl exec -it pvc-example -- sh
   ```

   Once inside the container, navigate to the mounted volume and list the contents:

   ```sh
   ls /mnt/data
   ```

   You should see the contents of the `/mnt/data` directory on the host node.

####  Clean up resources**

   After verifying the setup and completing your tasks, delete the Pod, PVC, and PV to clean up resources:

   ```sh
   kubectl delete -f pod-example.yaml
   kubectl delete -f pvc-example.yaml
   kubectl delete -f pv-example.yaml
   ```

#### Conclusion

Using PersistentVolume (PV) and PersistentVolumeClaim (PVC) in Kubernetes allows you to provision and manage storage resources effectively.