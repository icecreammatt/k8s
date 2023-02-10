# Registry local notes

- `minikube delete` first if one already exists
- `minikube start --insecure-registry "10.0.0.0/24"`

- `minikube addons enable registry`
- `docker run --rm -it --network=host alpine ash -c "apk add socat && socat TCP-LISTEN: 56999,reuseaddr,fork TCP:$(minikube ip): 56999"`
- `docker tag my/image localhost: 56999/myimage`
- `docker push localhost: 56999/myimage`

> After the image is pushed, refer to it by localhost: 56999/{name} in kubectl specs.

## Local Dashboard Access

- `minikube tunnel`
- `kubectl proxy`

### Install dashboard

- `minikube dashboard`
- `minikube addons enable ingress`
- `minikube addons enable metrics-server`

### Non Minikube Dashboard

- `kubectl -n kubernetes-dashboard create token kubernetes-dashboard | pbcopy && open http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login`

### ip-lookup

- `kk apply -f ip-lookup.yaml`
- `kk apply -f ingress.yaml`
- `minikube tunnel`
- `kubectl proxy`

### excalidraw

- `kk apply -f excalidraw.yaml`
- `kk apply -f ingress.yaml`

### hosts

```
127.0.0.1 ip.localhost.com 
127.0.0.1 excalidraw.localhost.com 
127.0.0.1 kubernetes.default.svc.cluster.local
```