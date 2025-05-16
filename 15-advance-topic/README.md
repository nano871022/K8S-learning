# Advance Topics

## Annotations

this annotations is added in metadata is just for information uses, its not inject to containers or to filter it

> ```
> "annotations": {
>   "key1": "value1",
>   "key2": "value2"
> }
> ```

Command to create it too

```
$ kubectl annotate pod mypod key1=value1 key2=value2
```

example uses

> ```
> apiVersion: apps/v1
> kind: Deployment
> metadata:
>   name: webserver
>   annotations:
>     description: Deployment based PoC dates 2nd Mar'2022
> ```

Test this command

```
$ kubectl run saved --image=nginx:alpine --save-image=true
$ kubectk get pod saved -o yaml
```
> this command create a yaml look like this [save config = true](./saved-config-trye.yaml)

## Resource Quota

this limit resources
1. **Compute Resource Quota**: CPU , Memory, etc in namespaces requested
2. **Storage Resource Quota**: limit total sum of storage resource, that can be requested [PersistentVolumeClaims, request.storage, etc]
3. **Object Count Quota**: restrict num of objects [Pods, ConfigMaps, PersistentVolumeClaims, ReplicationCOntrollers, Services, Secret, etc]

Examples, 
* [Coumpute Resource Quota](./computeresourcequote.yaml)
* [Object Count Quota](./objectcountquota.yaml)

## Limite Range

It limit resources allocation to POD and Containers in a namespace
*  Compute resources limits
*  Storage request limit per Persistent Volume Claim
*  Limit Ratio for a resource
*  Request, limits and inject them to containers env.

  Example [Limit Range](./limitrange.yaml)
