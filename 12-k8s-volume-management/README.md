# Volumes Management

create yaml and apply it on k8s [App Blue Sharing Volumen](./app-blue-shared-vol.yaml)

> $ kubeclt apply -f app-blue-shared-vol.yaml
>> this file create a volumen in local host and it is shared between nginx and debian, debian container create a index.html and it show in nginx container because that volume is shared.


Next run command to check in browser 

> $ kubectl expose deployment blue-app --type=NodePort
>> service/blue-app exposed
