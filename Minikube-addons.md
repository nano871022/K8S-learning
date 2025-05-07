# Addons

## List addons

> $ minikube addons list \
>> it show a table with all addons enable or disble you have setting on it

## Enable dashboard

> $ minikube addons enable metrics-server \
> $ minikube addons enable dashboard

## Run dashboard

> $ minikube dashboard \
>> 🤔  Verifying dashboard health ... \
>> 🚀  Launching proxy ... \
>> 🤔  Verifying proxy health ... \
>> 🎉  Opening http://127.0.0.1:43297/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/ in your default browser... \
>> 👉  http://127.0.0.1:43297/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/ \
