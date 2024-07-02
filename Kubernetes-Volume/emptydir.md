#### Overview

An `emptyDir` volume in Kubernetes is a type of storage volume that is created when a Pod is assigned to a Node and exists as long as that Pod is running on that Node. This type of volume is initially empty and can be used to store temperory data that doesn't need to persist beyond the lifecycle of the Pod.

#### Create a YAML file for the Pod definition**

   Create a YAML file named `emptydir-example.yaml` with the following content:

```bash
   apiVersion: v1
   kind: Pod
   metadata:
     name: emptydir-example
   spec:
     containers:
     - name: my-container
       image: busybox
       command: ['sh', '-c', 'echo "Hello, Kubernetes!" > /mnt/data/hello.txt && sleep 3600']
       volumeMounts:
       - mountPath: /mnt/data
         name: temp-storage
     volumes:
     - name: temp-storage
       emptyDir: {}
```

   This YAML defines a Pod named `emptydir-example` that uses the `busybox` image. It runs a command to write a message to a file and then sleeps for an hour. The `emptyDir` volume is mounted at `/mnt/data`.

#### Apply the YAML file to create the Pod**

   Use the `kubectl apply` command to create the Pod:

```bash
   kubectl apply -f emptydir-example.yaml
```
   This command will create the Pod in your Kubernetes cluster.

####  Verify the Pod creation**

   Check the status of the Pod to ensure it has been created and is running:

```bash
   kubectl get pods
```

   You should see an output indicating that the `emptydir-example` Pod is in the `Running` state.

####  Inspect the Pod and the volume**

   To inspect the volume and check the contents of the file created in the `emptyDir` volume, you can use `kubectl exec` to open a shell inside the container:

```bash
   kubectl exec -it emptydir-example -- sh
```

   Once inside the container, navigate to the mounted volume and check the contents of the file:

```bash
   cd /mnt/data
   cat hello.txt
```

   You should see the message "Hello, Kubernetes!" that was written to the file.

####  Clean up resources**

   After verifying the setup and completing your tasks, you can delete the Pod to clean up resources:

```bash
   kubectl delete pod emptydir-example
```
   This command will remove the Pod and its associated `emptyDir` volume.
