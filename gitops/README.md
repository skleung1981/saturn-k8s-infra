# Installation Guide for Saturn GitOps

## Installation
Argo CD is used for DevOps workflow. Unlike standard installation, A specific namespace `saturn-gitops` is used.

### Create Namespace (saturn-gitops)
```
kubectl create namespace saturn-gitops
kubectl apply -n saturn-gitops -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
### Install Argo CD
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

**Correct the namespace**
Due to not use the default namespace, additional steps is required to make sure that it is able to access the cluster.

```
for crb in argocd-application-controller argocd-applicationset-controller argocd-server; do
  kubectl patch clusterrolebinding $crb --type json \
    -p '[{"op":"replace","path":"/subjects/0/namespace","value":"saturn-gitops"}]'
done 
```

## Setup Repository

Get the username and password from forgejo to setup-repo.yaml and apply it. Make sure not checkin the file with plain-text password
```
kubectl apply -f gitops/setup-repo.yaml
```
 
## Setup Projects

Create Projects for saturn-frontend and saturn-backend
```
kubectl apply -f gitops/apps/setup-projects.yaml
```

## Setup applications

### SearXNG
```
kubectl apply -f gitops/apps/setup-backend-searxng.yaml
```

### OpenWebUI

```
kubectl apply -f gitops/apps/setup-frontend-openwebui.yaml
```

### RabbitMQ

```
kubectl apply -f gitops/apps/setup-backend-rabbitmq.yaml
```

### Mongo

```
kubectl apply -f gitops/apps/setup-backend-mongo.yaml
```

### Valkey

```
kubectl apply -f gitops/apps/setup-backend-valkey.yaml
```

### Postgres

```
kubectl apply -f gitops/apps/setup-backend-postgres.yaml
```

### Ollama

```
kubectl apply -f gitops/apps/setup-backend-ollama.yaml
```

In fact, if root-app.yaml is applied as well, it is not necessary to apply it manual because Argo CD will do it for you if repo is changed.
```
kubectl apply -f gitops/root-app.yaml
```

