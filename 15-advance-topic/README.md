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
