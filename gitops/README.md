# Installation Guide for Saturn GitOps

## Installation
Argo CD is used for saturn gitops.

```
kubectl create namespace saturn-gitops
kubectl apply -n saturn-gitops -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

```

```
kubectl apply -n saturn-gitops --server-side --force-conflicts \
  -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Grab the password
```
kubectl -n saturn-gitops get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

Temporary port forward to access the UI.
```
kubectl port-forward -n saturn-gitops svc/argocd-server 9000:443
```

Access it via https://localhost:9000
Username: admin
Password: <found it in the previous step>

## Setup Repository

Get the username and password from forgejo to setup-repo.yaml and apply it. Make sure not checkin the file with plain-text password
```
kubectl apply -f gitops/setup-repo.yaml
```
 
## Setup Projects

Create Projects for saturn-frontend and saturn-backend
```
kubectl apply -f gitops/setup-projects.yaml
```

## Setup applications

### SearXNG
```
kubectl apply -f gitops/setup-backend-searxng.yaml
```