# **Kubectl** commands

## Download kubectl

> $ curl -LO https://dl.k8s.io/release/v1.28.3/bin/linux/amd64/kubectl

## Install kubectl 

> $ sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

### Check installed

> $ minikube version \
> Client Version: v1.30.2 \
> Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3 \
> Server Version: v1.30.0

### Check configuration 

> $ kubectl config view \
>> it show setting of context, cluster and user, depend all profile created on it.   \
> $ cat ~/.kube/config \
>> it show same information of previous command

### Ckeck cluster info

> $ kubectl cluster-info \
> Kubernetes control plane is running at https://0.0.0.0:32771
> CoreDNS is running at https://0.0.0.0:32771/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
> To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.

### Check nodes

> $ kubectl get nodes \
>> NAME       STATUS   ROLES           AGE   VERSION \
>> minikube   Ready    control-plane   15h   v1.30.0

### Check namespaces

> $ kubectl get namespaces \
>> NAME              STATUS   AGE \
>> default           Active   15h  \
>> kube-node-lease   Active   15h  \
>> kube-public       Active   15h \
>> kube-system       Active   15h \

## Poxy

> $ kubectl proxy & \
>> it create endpoint http://localhost:8001/... \
>>> /healthz \
>>> /metrics \
>>> /api/ \
>>> /api/v1 \
>>> /apis/ \

## Create 

> $ kubectl create token default 
>> it create token to use auth against server, and it is format jwt \

> $ kubectl create clusterrole api-access-root --verb=get --non-resource-url=/* 
>> it create a cluseter role to acces api-access-root  \
>> clusterrole.rbac.authorization.k8s.io/api-access-root created

> $ kubectl create clusterrolebinding api-access-root --clusterrole api-access-root --serviceaccount=default:default 
>> it create cluser role binding acces api-access-root \
>> clusterrolebinding.rbac.authorization.k8s.io/api-access-root created

With this commands kubect allow us to request to ip and port 

> $ kubectl config view -o jsonpath='{.clusters[?(@.name == "minikube")].cluster.server}'
>> this command get url of server on cluster to make request

> $ curl ${URL}/healthz --header "Authorization: Bearer $TOKEN" --insecure
>> this command show us OK , it same way that use proxy <- proxy export internat port to external port, with token we can use direct port with this commands
