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
