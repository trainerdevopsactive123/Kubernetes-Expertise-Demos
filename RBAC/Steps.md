### RBAC in Kubernetes

RBAC (Role-Based Access Control) means restricting access to Kubernetes resources when accessed by a user or any service account.

## Types of Resources in Kubernetes
1. Namespaced
2. Non-namespaced

### To determine which resources are namespaced and which are not, check:
```sh
- kubectl api-resources --namespaced=false  
- kubectl api-resources --namespaced=true
```

### For example:
- Role is namespaced.
- ClusterRole is non-namespaced.

### With Role and ClusterRole, we can define a set of permissions, such as:
- Who can read secrets 
- Who can edit pods.

### RoleBinding and ClusterRoleBinding

RoleBinding and ClusterRoleBinding define who gets the set of permissions.

- Bind a Role or ClusterRole to something or someone.

## Example 1: User Steve's Permissions

### Create Namespaces

```sh
kubectl create ns prod
kubectl create ns test
```

### Create Role in `prod` Namespace

```sh
kubectl -n prod create role secret-manager --verb=get --resource=secrets
```

### To get more details:

```sh
kubectl -n prod describe role secret-manager
kubectl -n prod get role secret-manager -o yaml
kubectl -n prod get roles
```

### Create RoleBinding in `prod` Namespace

```sh
kubectl -n prod create rolebinding secret-manager --role=secret-manager --user=steve
```

To get more details:

```sh
kubectl -n prod describe rolebinding secret-manager
kubectl -n prod get rolebinding secret-manager -o yaml
kubectl -n prod get rolebindings
```

### Create Role in `test` Namespace

```sh
kubectl -n test create role secret-manager --verb=get --verb=list --resource=secrets
```

### Create RoleBinding in `test` Namespace

```sh
kubectl -n test create rolebinding secret-manager --role=secret-manager --user=steve
```

### Verify Permissions

```sh
kubectl -n prod auth can-i create pods --as steve 
kubectl -n prod auth can-i create secrets --as steve 
kubectl -n prod auth can-i get secrets --as steve 
kubectl -n prod auth can-i list secrets --as steve 
kubectl -n test auth can-i list secrets --as steve 
```

## Example 2: User Deff and User Tom's Permissions

### Create ClusterRole for User Deff

```sh
kubectl create clusterrole delete --verb=delete --resource=deployment
```

To get more information:

```sh
kubectl describe clusterrole delete
kubectl get clusterrole delete -o yaml
```

### Create ClusterRoleBinding for User Deff

```sh
kubectl create clusterrolebinding delete --clusterrole=delete --user=deff
```

To get more information:

```sh
kubectl describe clusterrolebinding delete
kubectl get clusterrolebinding delete -o yaml
```

### Create Namespace for User Tom

```sh
kubectl create ns alpha
```

### Create ClusterRole for User Tom

```sh
kubectl create clusterrole delete --verb=delete --resource=pods
```

### Create ClusterRoleBinding for User Tom

```sh
kubectl create clusterrolebinding delete1 --clusterrole=delete --user=tom --namespace=alpha
```

### Verify Permissions

```sh
kubectl auth can-i delete deploy --as deff -n alpha
kubectl auth can-i delete deploy --as deff
kubectl auth can-i get pod --as deff
```

## Clean Up

```sh
kubectl delete ns prod
kubectl delete ns test
kubectl delete role secret-manager -n prod
kubectl delete rolebinding secret-manager -n prod
kubectl delete role secret-manager -n test
kubectl delete rolebinding secret-manager -n test
kubectl delete clusterrole delete
kubectl delete clusterrolebinding delete
kubectl delete ns alpha
kubectl delete clusterrole delete1
```
```