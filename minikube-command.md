# Minikube Commands

## Start

> $ minikube start

### drivers

drivers used for tell minikube with virtual project will use to work

> $ minikube start --driver=docker
>> create a container in docker to work minikube

## Status

> $ minikube status
>> minikube \
>> type: Control Plane \
>> host: Running \
>> kubelet: Running \
>> apiserver: Running \
>> kubeconfig: Configured

## Delete

Remove the minikube and all settings over it

> $ minikube delete
>> 🔥  Deleting "minikube" in docker ... \
>> 🔥  Deleting container "minikube" ... \
>> 🔥  Removing /home/alejo/.minikube/machines/minikube ... \
>> 💀  Removed all traces of the "minikube" cluster.

## SSH

connecto to minikube in terminal mode

> $ minikube SSH
>> show us terminal commands into minikube
