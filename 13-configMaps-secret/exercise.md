# Excersise ConfigMap

create file [index](./index.html) with fronpage to attach to nginx container

create file [web](./web-green-wth-cm.yaml) yaml configuration of container
- replicas: 1
- volume: web-config
- nginx web

```
 $ kubectl create configmap green-web-cm --from-file=./index.html
```
> configmap/green-web-cm created

Expose service

```
$ kubctl expose deployment green-web --type=NodePort
```
> service/green-web created

Check with port forward in browser

```
$ kubectl port-forward service/green-web 8080:80
```
> check [web](http://localhost:8080)
