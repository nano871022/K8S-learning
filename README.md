# K8S-learning
Kubernetes Learning - CNFC

-- Commands

Download Minikube for linux 
> curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64

Install minikube

>  sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64

Start

> $ minikube start --driver=docker
>> Over docker drive \
> $ minikube start --drive=docker --nodes=2 --cpus=2 --memory=8g

Stop

> $ minikube stop

Delete

> $ minikube delete

Status

> $ minikube status \
> minikube \
> type: Control Plane \
> host: Running \
> kubelet: Running \
> apiserver: Running \
> kubeconfig: Configured

Profile list

> $ minikube profile list
>> $ minikube profile [list, minikube, minibox, default]
