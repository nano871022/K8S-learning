# Ingress

Settings ingress hosts webserver-blue-svc and webserver-green-svc how backend and accross to ingress to call that services

## 2 differents Hosts

File [Ingress Rule Green and Blue](./first-ingress.yaml)

```
$ kubectl create -f first-ingress.yaml
```

## 1 Host differents endpoints

File [Ingress Rule Green and Blue](./second-ingress.yaml)

```
$ kubectl create -f second-ingress.yaml
```
>> ingress.networking.k8s.io/fan-out-ingress created

Check Ingress created
```
$ kubectl get ingress 
```
NAME            |  CLASS  | HOSTS        | ADDRESS      |  PORTS |  AGE
----------------|---------|--------------|--------------|--------|---------
fan-out-ingress |  nginx  | example.com  | 192.168.49.2 |  80    |  20m

Check describe

```
$ kubectl describe ingress fan-out-ingress
```
> ```
> Name:             fan-out-ingress
> Labels:           <none>
> Namespace:        default
> Address:          192.168.49.2
> Ingress Class:    nginx
> Default backend:  <default>
> Rules:
>   Host         Path  Backends
>   ----         ----  --------
>   example.com
>                /blue    blue-svc:80 (10.244.2.2:8080)
>                /green   green-svc:80 (10.244.2.3:8080)
> Annotations:   nginx.ingress.kubernetes.io/service-upstream: true
> Events:
>   Type    Reason  Age                From                      Message
>   ----    ------  ----               ----                      -------
>   Normal  Sync    22m (x2 over 22m)  nginx-ingress-controller  Scheduled for sync
>```

> [!WARNING]
> its common update hosts file mapping names of server in k8s to minikube IP
>> 127.0.0.1        localhost \
>> ::1              localhost \
>> 192.168.99.100   blue.example.com green.example.com
>>> linux \
>>> $ sudo vim /etc‌‌/‌hosts

