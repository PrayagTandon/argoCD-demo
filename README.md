# ArgoCD Demo Setup

## Prerequisite

- kubectl
- minikube
- docker

## Steps for setting up argoCD

### kubectl install
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl && sudo mv kubectl /usr/local/bin/
```

### minikube install
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

### helm install
```
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

### argoCD CLI Install
```
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
```

### start minikube and verify cluster
```
minikube start --driver=docker --extra-config=apiserver.service-node-port-range=1-65535
kubectl cluster-info 
```

### ArgoCD Install

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### expose argoCD server
```
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```
#### use minikube tunnel or port forwarding (use separate terminal)
```
minikube tunnel 
```
or
```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

### Get argoCD admin Password
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
### Connect with the ArgoCD CLI
```
argocd login localhost:8080
```

### Register the app in argoCD
```
argocd app create argocd-demo \
  --repo https://github.com/PrayagTandon/argocd-demo.git \
  --path kubefolder \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace default \
  --sync-policy automated
```

### Check app status
```
argocd app get argocd-demo
```
