# 11 . Deployment a Stand-Alone Application

Create a webserver application with command

> $ kubectl create webserver --image=nginx:alpine --port=80 --replicas=3 --dry-run=client -o yaml > [webserver.yaml](github.com/nano871022/K8S-learning/11-deploy-standalone-app/webserver.yaml)
>> it create a file called webserver.yaml with configuration.

Next create a service in [webserver-svc.yaml](github.com/nano871022/K8S-learning/11-deploy-standalone-app/webserver-svc.yaml) file. \
That file use app=webserver filter to apply expose port to use that pods

Next use a comand to check all pods, deployments, replica set and service 

> $ kubectl get po,ep, -l app=webserver -o wide

## PODS

NAME                            | READY  | STATUS   | RESTARTS     | AGE  | IP           | NODE      | NOMINATED NODE  | READINESS GATES  | APP
--------------------------------|--------|----------|--------------|------|--------------|-----------|-----------------|------------------|------------
pod/webserver-77b9bf9c79-7bst2   | 1/1    | Running  | 2 (11m ago)  | 40m  | 10.244.0.9   | minikube  | <none>          | <none>           | webserver
pod/webserver-77b9bf9c79-fvwpf   | 1/1    | Running  | 2 (11m ago)  | 40m  | 10.244.0.11  | minikube  | <none>          | <none>           | webserver
pod/webserver-77b9bf9c79-w4n5d   | 1/1    | Running  | 2 (11m ago)  | 40m  | 10.244.0.10  | minikube  | <none>          | <none>           | webserver

## END POINTs

NAME                   | ENDPOINTS                                    | AGE  | APP
-----------------------|----------------------------------------------|------|------------
endpoints/web-service  | 10.244.0.10:80,10.244.0.11:80,10.244.0.9:80  | 19m  | webserver

When run next command you can get port expose per expose command

> $ kubectl describe service webserver-svc

```
Name:                     web-service
Namespace:                default
Labels:                   app=webserver
Annotations:              <none>
Selector:                 app=webserver
Type:                     NodePort
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.96.57.164
IPs:                      10.96.57.164
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  32000/TCP
Endpoints:                10.244.0.10:80,10.244.0.11:80,10.244.0.9:80
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```

In this case NodePort use 32000

Check ip minikube

> $ minikube ip
>> 192.168.49.2

Web-Service:nginx -> http://192.168.49.2:32000

if you can see the browser you can run next command

> $ minikube service webserver-svc


| NAMESPACE |    NAME     | TARGET PORT |            URL            |
|-----------|-------------|-------------|---------------------------|
| default   | web-service |          80 | http://192.168.49.2:32000 |


🏃  Starting tunnel for service web-service.

| NAMESPACE |    NAME     | TARGET PORT |          URL           |
|-----------|-------------|-------------|------------------------|
| default   | web-service |             | http://127.0.0.1:41901 |

🎉  Opening service default/web-service in default browser...\
👉  http://127.0.0.1:41901\
❗  Because you are using a Docker driver on linux, the terminal needs to be open to run it.\
✋  Stopping tunnel for service web-service.

In this case use a localhost/127.0.0.1 host and port assign 41901 with this command you can see page in browser

You can get url with next command but it block the command line

> $ minikube service web-service --url

```
http://127.0.0.1:34069
❗  Because you are using a Docker driver on linux, the terminal needs to be open to run it.
```

With next command can create a **port-forward** look like **minikube service web-service** but level **kubectl**

> $ kubectl port-forward service/webserver-svc 8080:80
>> this command block shell
>>> Forwarding from 127.0.0.1:8080 -> 80 \
>>> Forwarding from [::1]:8080 -> 80 \
>>> Handling connection for 8080 \
>>> Handling connection for 8080

# Expose Deployment

Expose a deployment with Loadbalancer

> $ kubectl expose deployment webserver --name=web-lb --type=LoadBalancer --port=8080
>> service/web-lb exposed

 NAME                            | READY  | STATUS   | RESTARTS    |  AGE
 --------------------------------|--------|----------|-------------|--------
pod/webserver-77b9bf9c79-7bst2  | 1/1    | Running  | 3 (25m ago)  | 79m
pod/webserver-77b9bf9c79-fvwpf  | 1/1    | Running  | 3 (25m ago)  | 79m
pod/webserver-77b9bf9c79-w4n5d  | 1/1    | Running  | 3 (25m ago)  | 79m

NAME                 | TYPE          | CLUSTER-IP      | EXTERNAL-IP  | PORT(S)         | AGE
---------------------|---------------|-----------------|--------------|-----------------|----------
service/kubernetes   | ClusterIP     | 10.96.0.1       | <none>       | 443/TCP         | 4h32m
service/web-lb       | LoadBalancer  | 10.106.117.165  | <pending>    | 8080:30687/TCP  | 19s
service/web-service  | NodePort      | 10.96.57.164    | <none>       | 80:32000/TCP    | 57m

NAME                       | READY  | UP-TO-DATE  | AVAILABLE  | AGE
---------------------------|--------|-------------|------------|---------
deployment.apps/webserver  | 3/3    | 3           | 3          | 79m

NAME                                  | DESIRED  | CURRENT  | READY  | AGE
--------------------------------------|----------|----------|--------|--------
replicaset.apps/webserver-77b9bf9c79  | 3        | 3        | 3      | 79m

## Create Tunnel

this connet localhost with cluster ip addres set

> $ minikube tunnel
>> ✅  Tunnel successfully started \
>> 📌  NOTE: Please do not close this terminal as this process must stay alive for the tunnel to be accessible ...

