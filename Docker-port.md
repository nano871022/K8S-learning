## Docker connect over ¨port 2375"

This case when docker was setting with different port to connect linux on windows, and docker can shared information outoff linux to windows was needed to change port, for that it need to create a export env var to use per minikube to know how connect with docker

> $ export DOCKER_HOST=tcp://0.0.0.0:2375 \
> $ minikube delete \
> $ minikube start --driver=docker \
> $ minikube status \
> minikube \
> type: Control Plane \
> host: Running \
> kubelet: Running \
> apiserver: Running \
> kubeconfig: Configured
