```markdown
## First, Create the Namespace

```sh
kubectl create namespace demo-clusterip
```

## Create Pod and Service

```sh
kubectl apply -f apache-deployment.yaml
kubectl apply -f apache-service.yaml
kubectl apply -f mysql-deployment.yaml
kubectl apply -f mysql-service.yaml
```

## Get the Pods and Services

```sh
kubectl get pods -n demo-clusterip
kubectl get pods -n demo-clusterip -l app=apache
kubectl get services -n demo-clusterip
```

## Exec into the Apache Pod to Test MySQL Connectivity

```sh
kubectl exec -it <apache-pod-name> -n demo-clusterip -- /bin/bash
```

## Inside the Apache Pod, Install MySQL Client and Test Connectivity

```sh
apt-get update
apt install mariadb-server -y
mysql -h mysql -u root -ppassword
```

Once connected to MySQL, you can run the following SQL command:

```sql
SHOW DATABASES;
CREATE DATABASE your_database_name;
```
```