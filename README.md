# Local Linux Server (Saturn) running K8S for CI/CD

For reference only about how I build my local development.

## Installation

### Self-signed Certificate 

A self-signed certificate is required in different area.. Please read the steps.md in the tls folder
- Import it to local JVM
- Nexus
- K8S (Ingress)



### K8S setup
TBC

#### Requirements
- No swap
- No selinux (Disable)

#### Create a namespce for Saturn CI/CD

```
kubectl create namespace saturn-cicd
```

#### Import the Self-signed cert to K8S 

```
kubectl create secret tls saturn-ingress-tls --cert=saturn-local.cert.pem --key=saturn-local.key.pem -n saturn-cicd

```

#### Install and Update the ingress nginx controller

```
helm install my-ingress ingress-nginx/ingress-nginx -n ingress-nginx -f ingress-values.yaml
```

```
kubectl edit deployment my-ingress-ingress-nginx-controller -n ingress-nginx
```

Add the following line in the args section
```
--default-ssl-certificate=saturn-cicd/saturn-ingress-tls
```

### Postgres
It is used to store data for local ci/cd services such as Forgejo, Dependency Track and etc.

#### Local user/group
- Username (UID): 
- Group (GID): 

#### Storage
- Path:
- Size (GB):

#### Steps

Create a secret in K8S and apply the postgres yaml file
```
kubectl create secret generic postgres-user-pass \
--namespace saturn-cicd \
--from-literal=username=<Your username> \
--from-literal=password='<Your Password>'

kubectl apply -f postgres.yaml
```

### Nexus
#### Local user/group
- Username (UID): 
- Group (GID): 

#### Storage
- Path:
- Size (GB):

#### Steps

```
kubectl apply -f nexus.yaml
```

In the nexus portal
- disable public access
- add cert using existing local self-signed certificate
- create the repo you need such as docker, maven and pypi
- update dasmon.json that located in /etc/docker. Add/Append saturn:30088 in the insecure-registries

### Jenkins
The jenkins support java and python project


#### Local user/group
- Username (UID): 
- Group (GID): 

#### Storage
- Path:
- Size (GB): 150GB

#### Steps

```
kubectl apply -f jenkins.yaml
```

#### Installed Tool
- Create secret file
- Additional Plugins
    - Configure Manager
- Maven (v?)
    - Setting file for repo
- Docker (v?)
    - 

### Dependency-Track
#### Local user/group
- Username (UID): 
- Group (GID): 

#### Storage
- Path:
- Size (GB):

### 