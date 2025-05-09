# Demo Authentication 1

## Create a namespace

> $ kubectl create namespace demo-auth1
>> namespace/demo-auth1 created

## Create folder to Role-Base Access Control

> $ mkdir rbac | cd rbac

## Create a new user demo-user1

> $ sudo useradd -s /bin/bash demo-user1
> $ sudo passwd demos-user1 # same name to passwd

## Create private key

> $ openssl genrsa -out demos-user1.key 2048 \
> $ openssl req -new -key demos-user1.key -out demos-user1.csr -subj "/CN=user1/O=demos"

## Create Signing Request yaml

> $ vim signingrequest.yaml
>> apiVersion: certificates.k8s.io/v1 \
>> kind: CertificateSigningRequest \
>> metadata: \
>>   name: demos-user1-csr \
>> spec: \
>>   groups: \
>>   - system:authenticated \
>>  request: <assign encoded value from command: cat demos-user1.csr | base64  | tr -d '\n','%'> \
>>   signerName: kubernetes.io/kube-apiserver-client \
>>   usages: \
>>   - digital signature \
>>   - key encipherment \
>>   - client auth

## Create on k8s

> $ kubectl create -f signing-request.yaml
>> certificatesigningrequest.certificates.k8s.io/demos-user1-csr created

## Check csr in K8s

> $ kubectl get csr
>> NAME              AGE   SIGNERNAME                            REQUESTOR       REQUESTEDDURATION   CONDITION
>> demos-user1.csr   24s   kubernetes.io/kube-apiserver-client   minikube-user   <none>              Pending

## Approve certificated

> $ kubectl certificate approve demos-user1.csr
>> certificatesigningrequest.certificates.k8s.io/demos-user1.csr approved


> Check csr Approved in K8s 

> $ kubectl get csr
>> NAME              AGE   SIGNERNAME                            REQUESTOR       REQUESTEDDURATION   CONDITION
>> demos-user1.csr   24s   kubernetes.io/kube-apiserver-client   minikube-user   <none>              Approved,Issued

## Get certificate from K8s

> $ kubectl get csr demos-user1.csr -o jsonpath='{.status.certificate}' | base64 -d > certificate-demos-user1.crt

## Assign Certificate to *user1* credentials

> $ kubectl config set-credentials user1 --client-certificate=demos-user1.crt --client-key=demos-user.key

## Assign user to context (cluster, namespace, user)

> $ kubectl config set-context demos-user1-context --cluster=minikube --namespace=test-namespace --user=user1
>> Context "demos-user1-context" created.

## View all configuration

> $ kubectl config view
>>apiVersion: v1 \
>>clusters:\
>>- cluster:\
>>    certificate-authority: /home/alejo/.minikube/ca.crt\
>>    extensions:\
>>    - extension:\
>>        last-update: Thu, 08 May 2025 09:41:50 -05\
>>        provider: minikube.sigs.k8s.io\
>>        version: v1.33.1\
>>      name: cluster_info\
>>    server: https://0.0.0.0:32771\
>>  name: minikube\
>>contexts:\
>>- context:\
>>    cluster: minikube\
>>    namespace: test-namespace\
>>    user: user1\
>>  name: demos-user1-context\
>>- context:\
>>    cluster: minikube\
>>    extensions:\
>>    - extension:\
>>        last-update: Thu, 08 May 2025 09:41:50 -05\
>>        provider: minikube.sigs.k8s.io\
>>        version: v1.33.1\
>>      name: context_info\
>>    namespace: default\
>>    user: minikube\
>>  name: minikube\
>>current-context: minikube\
>>kind: Config\
>>preferences: {}\
>>users:\
>>- name: minikube\
>>  user:\
>>    client-certificate: /home/alejo/.minikube/profiles/minikube/client.crt\
>>    client-key: /home/alejo/.minikube/profiles/minikube/client.key\
>>- name: user1\
>>  user:\
>>    client-certificate: /mnt/c/Users/jose.parra.PERFICIENT/workspace-k8s/learning-cnfc/rbac/demos-user1-copy.crt\
>>    client-key: /mnt/c/Users/jose.parra.PERFICIENT/workspace-k8s/learning-cnfc/rbac/demos-user1.key

## Create a Deployment over namespace test-namespace

> $ kubectl -n test-namespace create deployment nginx-demo1 --image=nginx:alpine
>> deployment.apps/nginx-demo1 created

## Check Pod on Namespace

>  $ kubectl get pods -n test-namespace
>> NAME                           READY   STATUS    RESTARTS   AGE \
>> nginx-demo1-85d5d9fc76-8p55t   1/1     Running   0          31s

## Check Pod in context

> $ kubectl get pods --context=demos-user1-context
>> Error from server (Forbidden): pods is forbidden: User "user1" cannot list resource "pods" in API group "" in the namespace "test-namespace"

## Create Role for namespace

>### create role.yaml 
>apiVersion: rbac.authorization.k8s.io/v1 \
>kind: Role \
>metadata: \
>  name: pod-reader \
>  namespace: test-namespace \
>rules: \
>- apiGroups: [""] \
>  resources: ["pods"] \
>  verbs: ["get", "watch", "list"] 
>> $ kubectl create -f role.yaml
>>> role.rbac.authorization.k8s.io/pod-reader created

## Check roles on namespaces

> $ kubectl get roles -n test-namespace
>> NAME         CREATED AT \
>> pod-reader   2025-05-08T21:38:52Z

## Create Role Binding

> ### Create file rolebinding.yaml
> apiVersion: rbac.authorization.k8s.io/v1
> kind: RoleBinding
> metadata:
>   name: pod-read-access
>   namespace: test-namespace
> subjects:
> - kind: User
>   name: bob
>   apiGroup: rbac.authorization.k8s.io
> roleRef:
>   kind: Role
>   name: pod-reader
>   apiGroup: rbac.authorization.k8s.io
>> $ kubectl create -f rolebinding.yaml
>>> rolebinding.rbac.authorization.k8s.io/pod-read-access created

## Check Role Bindings

> $ kubectl get rolebindings --namespace test-namespace
>> NAME              ROLE              AGE \
>> pod-read-access   Role/pod-reader   2m51s

## Check again Context for namespace
> $ kubectl get pods --context=demos-user1-context
>> NAME                           READY   STATUS    RESTARTS   AGE \
>> nginx-demo1-85d5d9fc76-8p55t   1/1     Running   0          17m
