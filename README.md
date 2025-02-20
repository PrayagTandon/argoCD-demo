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