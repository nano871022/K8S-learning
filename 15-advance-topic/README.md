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

## Auto Scaling

It help us to scale pods automatically depends of metrics

### Horizontal POD AutoScaler (HPA)

Auto scaler based on CPU utilization to ReplicaSet, Deployment or Recplication Controller

Ex:.
```
$ kubectl autoscale deploy my-app --min=2 --max=10 --cpu-percent=80
```
> this setting say to my-app than min pods replicates are 2 and max 10 and it increase it when cpu reach 80% of used

### Vertical POD AutoScaler (VPA)

Resource requirements of CPU and Memory ina POD. based on historical utlization data, availablility and real time events , its installed as a Custome Resource

### Cluster AutoScaler

Re-size Kubernetes Cluster, when there are insufficient resources available for new pods.

## Job And CronJobs

### Job

its used for execute a pod until it finish the process and it will be terminate automatically

* **Parallelism**: Pods run in parallel
* **Completions**: Num expected completions
* **Active Dead Line Seconds**: Duration Job
* **Back Off Limit**: Num retries before Job is marked as failed
* **TTL Seconds After Finised**: Delay cleanup of the finished Jobs.

  ### CronJobs

  Job at schedule times/date, Job Object is create per each execution cycle

  * **Start Dead Line Seconds**: dedline to start a job if scheduled time was missed.
  * ** Concurrency Policy**: Allow or forbid concurrent jobs or replace old job with new ones

  ## Stateful Sets

  App require an unique identify, such as name, network id, stric order. Stateful Set Controller has very strict service and storage volume

  ## Custom Resource

  Dinamically resources appear and disappear

  ### Customer Resource Definition (CRDs)

  easy way to add resources.

  ### API Aggregation

  fine-grained control for custom resources.

  ## Security Context and POD Security Admission

  object access permission, provileaged running, capabilities, secirity labels, etc\
  limited to PODs and Contrainers, its setting in spec secction

  > ```
  > spec:
  > securityContext:
  >   runAsUser: 1000
  >   runAsGroup: 3000
  >   fsGroup: 2000
  > ```

  ## Network Policies

  Network Policy API resources specifies podSelectors, ingress, egress policy types, and rules based on source and desination **ipBlocks** and **ports**\
  simplistic **default allow** and **default deny** policy, recomendated define **default deny** policy to block all traffic and then define set od rules for specific traffic allowed.

  Ex. [Network Policy](./networkpolicy.yaml)

  in this example block all ips and open port to specific project

  ## Monitoring, Logging and Troubleshooting

  * **Metric Server**
  * **Prometheus** app by CNCF

  ### Commands

  ```
  $ kubectl logs {pod-name}
  ```
  ```
  $ kubectl exec {pod-name} -- ls -la
  ```
  ```
  $ kubectl exec {pod-name} -it -- /bin/sh
  ```
  ```
  $ kubectl get events
  ```
  ```
  $ kubectl events
  ```

  ## [Helm](https://helm.sh/)

  app to work with object [Deploymentl, servces, PersisteVolumes,...] its a charht repository to help with this, now, it use kubectl and kubeconfig to securely communicate with k8s API server.

 ## Service Mesh

 App tool to discovery, mutual TLS certificates for encruptun, multi-cloud, and traffic telemetric, implementation that relies on a proxu component part of the Data Plane, that manage through a cControl Plane.
