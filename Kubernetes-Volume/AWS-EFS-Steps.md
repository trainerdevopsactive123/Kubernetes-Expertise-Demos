```markdown
# Steps to Deploy a Kubernetes Pod Using AWS EFS Volume

## Create EFS File System

1. **Login to AWS Account**:
   - Search for EFS.
   - Click on "Create file system".
   - Select the name.
   - Select VPC (ensure the same VPC where EKS is deployed is selected).
   - Click "Create".

2. **Configure Network Settings**:
   - Once the file system is created, click on its name.
   - Go to the "Network" tab.
   - Click on "Manage".
   - Update the security group.
   - Ensure your EFS file system has mount targets in each availability zone used by your EKS cluster.

3. **Update Security Group for EC2**:
   - Go to the EC2 instances (nodes).
   - Navigate to the "Security" section.
   - Click on the security group.
   - Open NFS port 2049 in the inbound rules.

## Install EFS CSI Driver

```bash
kubectl apply -k "github.com/kubernetes-sigs/aws-efs-csi-driver/deploy/kubernetes/overlays/stable/ecr/"
```

**To verify if the CSI driver is installed successfully**:

```bash
kubectl get all --all-namespaces
```

## Create StorageClass for EFS

1. **Create a StorageClass YAML file (efs-sc.yaml)**:

    ```yaml
    apiVersion: storage.k8s.io/v1
    kind: StorageClass
    metadata:
      name: efs-sc
    provisioner: efs.csi.aws.com
    ```

2. **Apply StorageClass**:

    ```bash
    kubectl apply -f efs-sc.yaml
    ```

## Create PersistentVolume and PersistentVolumeClaim

1. **Create a PersistentVolume (PV) YAML file (efs-pv.yaml)**:

    ```yaml
    apiVersion: v1
    kind: PersistentVolume
    metadata:
      name: efs-pv
    spec:
      capacity:
        storage: 5Gi
      volumeMode: Filesystem
      accessModes:
        - ReadWriteMany
      persistentVolumeReclaimPolicy: Retain
      storageClassName: efs-sc
      csi:
        driver: efs.csi.aws.com
        volumeHandle: fs-12345678 # Replace with your EFS File System ID, no DNS name
    ```

2. **Create a PersistentVolumeClaim (PVC) YAML file (efs-pvc.yaml)**:

    ```yaml
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: efs-pvc
    spec:
      accessModes:
        - ReadWriteMany
      storageClassName: efs-sc
      resources:
        requests:
          storage: 5Gi
    ```

3. **Apply PV and PVC**:

    ```bash
    kubectl apply -f efs-pv.yaml
    kubectl apply -f efs-pvc.yaml
    ```

## Deploy a Pod using EFS Volume

1. **Create a pod YAML file (efs-pod.yaml)**:

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: efs-app
    spec:
      containers:
        - name: app
          image: amazonlinux
          command: [ "sh", "-c", "echo Hello Kubernetes! > /data/out.txt && sleep 3600" ]
          volumeMounts:
            - name: efs-volume
              mountPath: /data
      volumes:
        - name: efs-volume
          persistentVolumeClaim:
            claimName: efs-pvc
    ```

2. **Deploy Pod**:

    ```bash
    kubectl apply -f efs-pod.yaml
    ```

## Verify the Pod and EFS Volume

1. **Check Pod Status**:

    ```bash
    kubectl get pods
    ```

2. **Access Pod and Verify**:

    ```bash
    kubectl exec -it efs-app -- /bin/bash
    cat /data/out.txt
    ```

3. **Create Files in the /data/ Directory**:

    ```bash
    touch /data/file1 /data/file2 /data/file3
    ```

4. **Verify by Creating a New Pod**:

    Create a new pod and check if the files are accessible in the EFS volume.
```