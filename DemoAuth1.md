# Demo Authentication 1

Create a namespace

> $ kubectl create namespace demo-auth1
>> namespace/demo-auth1 created

Create folder to Role-Base Access Control

> $ mkdir rbac | cd rbac

Create a new user demo-user1

> $ sudo useradd -s /bin/bash demo-user1
> $ sudo passwd demos-user1 # same name to passwd

Create private key

> $ openssl genrsa -out demos-user1.key 2048 \
> $ openssl req -new -key demos-user1.key -out demos-user1.csr -subj "/CN=user1/O=demos"

Create Signing Request yaml

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

Create on k8s

> $ kubectl create -f signing-request.yaml
>> certificatesigningrequest.certificates.k8s.io/demos-user1-csr created

Check csr in K8s

> $ kubectl get csr
>> NAME              AGE   SIGNERNAME                            REQUESTOR       REQUESTEDDURATION   CONDITION
>> demos-user1.csr   24s   kubernetes.io/kube-apiserver-client   minikube-user   <none>              Pending

Aprove certificated

> $ kubectl certificate approve demos-user1.csr
>> certificatesigningrequest.certificates.k8s.io/demos-user1.csr approved


> Check csr Approved in K8s 

> $ kubectl get csr
>> NAME              AGE   SIGNERNAME                            REQUESTOR       REQUESTEDDURATION   CONDITION
>> demos-user1.csr   24s   kubernetes.io/kube-apiserver-client   minikube-user   <none>              Approved,Issued
