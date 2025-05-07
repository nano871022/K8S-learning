# POD handly

when you need to create a pod handly 

> $ kubectl run nginx-pod \\ \
> --image=nginx:1.22.1 \\ \
> --port=80 \\ \
> --dry-run=client
> -o yaml > nginx-pod.yaml
>> this command  create a yaml with settings

## Command 

> $ kubectl apply -f {file-yaml}
>> this command run setting in file-yaml

> $ kubectl get pods
>> this command show pods created

> $ kubectl get pods -o wide
>> show information pods with most detail 

> $ kubectl get pod {name-pod} -o yaml
>> this command show full complete description setting of execution pod in yaml format

> $ kubectl describe pod {name-pod}
>> this command show description of execution pod 

> $ kubectl delete pod {name-pod}
>> this command show delete pod named

