# ServiceType: Load Balancer   

Per default ClusterIp and NodePort use a default loadbalancer for replica set service set, we can use a loadbalancer external, with Cloud Provider

Service can be mapped to ExternalIP address, the traffic can work over cluster with ExternalIP routed to one of service endpoints. it need a external Cloud Provider and LoadBalancer set in Cloud Provider infraestructure.

> Note: Please note that ExternalIPs are not managed by Kubernetes. The cluster administrator has to configure the routing which maps the ExternalIP address to one of the nodes.

# ServiceType: ExternalName

it does not define any endpoints, it return a CNAME record of an externally configurate service it using netowrking naming {namepod}.{domain} ex: my-database.example.com

# MultiPort Services

it is helpfully when the POD has multiple port listening of a POD with multipler container user port each one of them

> apiVersion: v1 \
> kind: Service \
> metadata: \
>  name: my-service \
> spec: \
>  selector: \
>   app: my-app \
>  type: NodePort \
>  ports: 
>  - name: http \
>    protocol: TCP \
>    port: 8080 \
>    targetPort: 80 \
>    nodePort: 31080 
>  - name: https:
>    protocol: TCP \
>    port: 8443 \
>    targetPort: 443 \
>    nodePort: 31443

# Port Forwarding

easily forward from local port to application port.

> $ kubectl port-forward deploy/fontend 8080:5000
>> cominicate to especific pod

> $ kubectl port-forward frontendt-77asd787-asdasd 8080:5000
>> communicate a especifi container into pod

> $ kubectl port-forward service/frontend-svc 8080:80
>> communicate to loadbalancer per default manage that service
