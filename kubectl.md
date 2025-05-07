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
