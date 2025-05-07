# POD handly

when you need to create a pod handly 

> $ kubectl run nginx-pod \\ \
> --image=nginx:1.22.1 \\ \
> --port=80 \\ \
> --dry-run=client
> -o yaml > nginx-pod.yaml
>> this comand run a specific pod and create a yaml with settings

## Command 

> $ kubectl apply -f <<file-yaml>>

> $ kubectl get pods

> $ kubectl get pod <<name-pod>> -o yaml

> $ kubectl describe pod <<name-pod>>

> $ kubectl delete pod <<name-pod>>
