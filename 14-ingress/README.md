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

> [!WARNING]
> its common update hosts file mapping names of server in k8s to minikube IP
>> 127.0.0.1        localhost \
>> ::1              localhost \
>> 192.168.99.100   blue.example.com green.example.com
>>> linux \
>>> $ sudo vim /etc‌‌/‌hosts
