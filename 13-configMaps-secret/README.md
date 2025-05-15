# ConfigMaps and Secret

This allow us custom data to shared in each pod depends what it needed

## ConfigMaps

### explicit declaration

> $ kubectl create configmap my-config --from-literal=key1=value1 --from-literal=key2=value2
>> configmap/my-config created

Check this information

> $ kubectl get configmaps my-config -o yaml

```
apiVersion: v1
data:
  key1: value1
  key2: value2
kind: ConfigMap
metadata:
  creationTimestamp: 2024-03-02T07:21:55Z
  name: my-config
  namespace: default
  resourceVersion: "241345"
  selfLink: /api/v1/namespaces/default/configmaps/my-config
  uid: d35f0a3d-45d1-11e7-9e62-080027a46057
```

### Definition in yaml

file [customer1-configmap.yaml](./customer1-configmap.yaml)

run it 

> $ kubectl apply -f customer1-configmap.yaml
>> configmap/customer1 created

### Properties File

file [properties](./permission-reste.properties)

``` 
$ kubectl create configmap permission-config --from-file={path}/permission-reste.properties
```
> configmap/permission-config created

### Apply config map into container in enviroments variables

yaml configuration
```
 containers:
 - name: my-app
   image: nginx:alpine
   envFrom:
   - configMapRef:
      name: full-config-map 
```


yaml configuration
```
 containers:
 - name: my-app
   image: nginx:alpine
   env:
   - name: SPECIFIC_ENV_VAR1
     valueFrom:
      configMapKeyRef:
       name: config-map1
       key: SPECIIFIC_DATA
   - name: SPECIFIC_ENV_VAR2
     valueFrom:
      configMapKeyRef:
       name: config-map2
       key: SPECIIFIC_INFO
```

### Apply config map into container with volume

yaml configuration
```
 containers:
 - name: my-app
   image: nginx:alpine
   volumeMounts:
   - name: config-volume
     mountPath: /etc/config
 volumes:
 - name: config-volume
   configMap:
    name: vol-config-map  
```

## Secrets

this encript infromation in base64 and saved encripted usially in etcd

### Create a Secret

```
$ kubectl create secret generic my-password --from-literal=password=mysqlpassword
```
> secret/my-password created

```
$ kubectl describe secret my-passwd
```
```
Name:         my-password
Namespace:    default
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
passwd:  11 bytes
```
> in this case we can not see the passwd

```
$ kubectl get secret my-passwd -o yaml
```
```
apiVersion: v1
items:
- apiVersion: v1
  data:
    passwd: bXlzcWxwYXNzd2Q=
  kind: Secret
  metadata:
    creationTimestamp: "2025-05-15T00:27:27Z"
    name: my-password
    namespace: default
    resourceVersion: "40704"
    uid: 3a424fd6-f0b1-41ce-9a26-3468669a2fae
  type: Opaque
kind: List
metadata:
  resourceVersion: ""
```
> in this case we can see passwd but encripted in base64

### Create secret with yaml

use [create-password yaml](./create-password.yaml)
> secret/my-password created
