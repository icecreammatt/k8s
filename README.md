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
- `http://127.0.0.1:8001/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/#/workloads?namespace=default`

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

#### Access

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

## Authentik

- https://xpufx.com/posts/protecting-your-first-app-with-authentik/
- https://goauthentik.io/docs/installation/kubernetes
- helm repo add authentik https://charts.goauthentik.io
- helm repo update
- helm upgrade --install authentik authentik/authentik -f values.yaml

# LetsEncrypt
- sudo /opt/certbot/bin/pip install certbot-dns-cloudflare
- sudo /opt/certbot/bin/certbot certonly --dns-cloudflare \ 
  --dns-cloudflare-credentials ~/CloudFlare/sub.domain.ini -d sub.domain

> Raspbian - certificate output, copy these to machine with Caddy
```bash
$ ls /etc/letsencrypt/archive/dev.domain.com/
cert1.pem  chain1.pem  fullchain1.pem  privkey1.pem
```

> sub.domain.ini
```
ns_cloudflare_api_token = XXX
```

## Caddy

> TLS config - /etc/caddy/Caddyfile

```
{   
    debug
    log
    local_certs
}

https://video.dev.domain.com {

    tls /Users/matt/certs/dev.domain.com/cert1.pem /Users/matt/certs/dev.domain.com/privkey1.pem

    encode gzip
    
    handle_path /* {
        reverse_proxy 127.0.0.1:5173
    }

    handle_path /hls/* {
        root * ./hls
        file_server browse
    }

    handle_path /dash/* {
        root * ./dash
        file_server browse
    }

    handle {
        root * ./public
        file_server browse
    }

}
```

# Hosts
```
127.0.0.1 ip.localhost.com 
127.0.0.1 mattcarrier.localhost.com
127.0.0.1 excalidraw.localhost.com 
127.0.0.1 kubernetes.default.svc.cluster.local
```