# ConfigMaps and Secret

This allow us custom data to shared in each pod depends what it needed

## ConfigMaps

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
