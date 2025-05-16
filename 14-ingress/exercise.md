# Exercise with Servicess

Use deployment [blue-app](./app-blue-shared-vol-yaml) and [green-app](./app-green-shared-vol.yaml) yamls

Expose blue-app 
```
$ kubectl expose deploymet blue-app --name=blue-svc --port=80 --target-port:8080 --type=NodePort
```
> service/blue-svc expose

Expose green-app 
```
$ kubectl expose deploymet green-app --name=green-svc --port=80 --target-port:8080 --type=NodePort
```
> service/green-svc expose

enable ingress controller
```
$  minikube addons enable ingress
```
> ```
> 💡  ingress is an addon maintained by Kubernetes. For any concerns contact minikube on GitHub.
> You can view the list of minikube maintainers at: https://github.com/kubernetes/minikube/blob/master/OWNERS
>     ▪ Using image registry.k8s.io/ingress-nginx/controller:v1.10.1
>     ▪ Using image registry.k8s.io/ingress-nginx/kube-webhook-certgen:v1.4.1
>     ▪ Using image registry.k8s.io/ingress-nginx/kube-webhook-certgen:v1.4.1
> 🔎  Verifying ingress addon...
> minikube addons enable ingress
> ```

See all settings

```
$ kubectl get all -o wide -L app -l app=blue-app -l app=green-app
```

NAME                           |  READY  | STATUS  |  RESTARTS |  AGE   |  IP         |  NODE         |  NOMINATED NODE |  READINESS GATES |  APP
-------------------------------|---------|---------|-----------|--------|-------------|---------------|-----------------|------------------|-----------
pod/blue-app-84c86b5c8b-5xpzv  |  2/2    | Running |  0        |  5h15m |  10.244.2.2 |  minikube-m03 |  <none>         |  <none>          |  blue-app
pod/green-app-5d5497d885-gbvlb |  2/2    | Running |  0        |  5h13m |  10.244.2.3 |  minikube-m03 |  <none>         |  <none>          |  green-app

NAME               |  TYPE      |  CLUSTER-IP     |  EXTERNAL-IP |  PORT(S)      |  AGE   |  SELECTOR     |   APP
-------------------|------------|-----------------|--------------|---------------|--------|---------------|---------------
service/blue-svc   |  NodePort  |  10.109.69.117  |  <none>      |  80:32465/TCP |  5h8m  |  app=blue-app |   blue-app
service/green-svc  |  NodePort  |  10.103.153.174 | <none>       | 80:30530/TCP  | 5h8m   | app=green-app |  green-app
service/kubernetes |  ClusterIP |  10.96.0.1      |  <none>      |  443/TCP      |  5h31m |  <none>       |

NAME                      |  READY |  UP-TO-DATE |  AVAILABLE |  AGE   |  CONTAINERS   |  IMAGES       |  SELECTOR     |   APP
--------------------------|--------|-------------|------------|--------|---------------|---------------|---------------|-------------
deployment.apps/blue-app  |  1/1   |  1          |  1         |  5h15m |  nginx,debian |  nginx,debian |  app=blue-app |   blue-app
deployment.apps/green-app |  1/1   |  1          |  1         |  5h13m |  nginx,debian |  nginx,debian |  app=green-app|   green-app

NAME                                 |  DESIRED  | CURRENT  | READY  | AGE    | CONTAINERS    | IMAGES        | SELECTOR                                  |   APP
-------------------------------------|-----------|----------|--------|--------|---------------|---------------|-------------------------------------------|-----------
replicaset.apps/blue-app-84c86b5c8b  |  1        | 1        | 1      | 5h15m  | nginx,debian  | nginx,debian  | app=blue-app,pod-template-hash=84c86b5c8b  |  blue-app
replicaset.apps/green-app-5d5497d885 |  1        | 1        | 1      | 5h13m  | nginx,debian  | nginx,debian  | app=green-app,pod-template-hash=5d5497d885  | green-app

Run [second-ingress](./second-ingress.yaml) yaml
```
$ kubectl create -f second-ingress.yaml
```
> ingress.networking.k8s.io/fan-out-ingress created

Check Ingress created
```
$ kubectl get ingress 
```
NAME            |  CLASS  | HOSTS        | ADDRESS      |  PORTS |  AGE
----------------|---------|--------------|--------------|--------|---------
fan-out-ingress |  nginx  | example.com  | 192.168.49.2 |  80    |  20m

# test it

## same machine run docker

if you workin in same machine with docker direct, you just need to use http://$(minikubeip)/blue or http://$(minikubeip)/green

## wls 

if you use linux over windows wls you need to

```
$ kubectl port-forward -n ingress-nginx service/ingress-nginx-controller-{hashcode}
```
> when you run ```$ minikube addons enable ingress``` it create a namespace called ingress-nginx where it save controller ingress and manage all over this
>> ```
>> $ kubectl get all -n ingress-nginx
>> ```

and you need add in ```c:/windows/system32/driver/etc/hosts``` edit file and  add line ```127.0.0.1 local-domain.dev```

now you can use browser and put http://local-domain.dev/blue

but if does not work try in cmd terminal

```
$ curl -v http://local-domain.dev:8082/blue
$ curl -v http://local-domain.dev:8082/green
```

or use it over linux into wls

```
$ curl -v -H "Host: local-domain.dev" http://localhost:8082/blue
$ curl -v -H "Host: local-domain.dev" http://localhost.dev:8082/green
```
