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
- `kubectl proxy`

### Non Minikube Dashboard

- `kubectl -n kubernetes-dashboard create token kubernetes-dashboard | pbcopy && open http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login`

### ip-lookup

- `kk apply -f ip-lookup.yaml`
- `kk apply -f ingress.yaml`
- `minikube tunnel`

### excalidraw

- `kk apply -f excalidraw.yaml`
- `kk apply -f ingress.yaml`
- `minikube tunnel`

### mattcarrier

- `kk apply -f mattcarrier.yaml`
- `kk apply -f ingress.yaml`
- `minikube tunnel`

### ArgoCD

#### Install

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

#### Accesss

```bash
minikube service argocd-server -n argocd -p test
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath='{.data.password}' | base64 -d
```

#### Uninstall

`kubectl delete -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml`

### DNS

#### PiHole DNS Setup
https://github.com/kubernetes-sigs/external-dns/blob/master/docs/tutorials/pihole.md


### hosts

```
127.0.0.1 ip.localhost.com 
127.0.0.1 mattcarrier.localhost.com
127.0.0.1 excalidraw.localhost.com 
127.0.0.1 kubernetes.default.svc.cluster.local
```