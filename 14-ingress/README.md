# Ingress

Settings ingress hosts webserver-blue-svc and webserver-green-svc how backend and accross to ingress to call that services

## 2 differents Hosts

File [Ingress Rule Green and Blue](./first-ingress.yaml)

```
$ kubectl apply -f first-ingress.yaml
```

## 1 Host differents endpoints

File [Ingress Rule Green and Blue](./second-ingress.yaml)

```
$ kubectl apply -f second-ingress.yaml
```
