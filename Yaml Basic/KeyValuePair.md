# Key Value Pair

```yaml
Name: Vicky
Class: Devops
City: Delhi
```

### Space after `:` is mandatory 
- Key = Name, Class, City 
- Values = Vicky, Devops, Delhi 

### Example

```yaml
apiVersion: v1          # This is a key-value pair where 'apiVersion' is the key and 'v1' is the value.
kind: Pod               # Another key-value pair where 'kind' is the key and 'Pod' is the value.
metadata:               # 'metadata' is a key that maps to another dictionary.
  name: nginx-pod       # Inside the 'metadata' dictionary, 'name' is a key with the value 'nginx-pod'.
  labels:               # 'labels' is a key that maps to another dictionary.
    app: nginx          # Inside the 'labels' dictionary, 'app' is a key with the value 'nginx'.
spec:                   # 'spec' is a key that maps to another dictionary.
  containers:           # 'containers' is a key that maps to a list of dictionaries.
  - name: nginx         # This is a dictionary inside the list, where 'name' is a key with the value 'nginx'.
    image: nginx:latest # Another key-value pair inside the 'containers' dictionary.
    ports:              # 'ports' is a key that maps to a list of dictionaries.
    - containerPort: 80 # This is a dictionary inside the list, where 'containerPort' is a key with the value '80'.
```
