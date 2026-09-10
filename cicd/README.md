# Local Linux Server (Saturn) running K8S for CI/CD

For reference only about how I build my local development CI/CD on my PC (saturn) using K8S

## Installation

### Self-signed Certificate 

A self-signed certificate is required in different area. Please use the below command to create a self-signed cert.

```
openssl req -x509 -newkey rsa:2048 -nodes -keyout saturn-local.key.pem -out saturn-local.cert.pem -days 3650
cat saturn-local.key.pem saturn-local.cert.pem > saturn-local.full.pem
```

The files are required to the following
- Import it to local JVM

```
cd tls
sudo keytool -importcert -alias saturn-nexus -file saturn-local.cert.pem -keystore /usr/lib/jvm/java-25-openjdk/lib/security/cacerts -storepass changeit -noprompt  
```

- Nexus
    - In the nexus portal, you should import the cert.    
- K8S (Ingress)
    - Please follow the step 'mport the Self-signed cert to K8S'


### K8S setup

<To be filled>

#### Requirements
- No swap
- No selinux (Disable)

#### Create a namespce for Saturn CI/CD

A namespace is recommended so all components are classified by different namespace. 

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

#### Local user/group (Optional)
Make sure a correct permission is applied.
- Username (UID): cicd-postgres (967)
- Group (GID): cicd (5000)

#### Storage
- Path: /opt/saturn-cicd/postgres
- Size (GB): 30

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
<To be filled>

#### Local user/group
- Username (UID): cicd-nexus (200)
- Group (GID): cicd (5000)

#### Storage
- Path: /mnt/saturn-cicd/nexus
- Size (GB): 400

#### Steps

Apply the postgres yaml file
```
kubectl apply -f nexus.yaml
```

In the nexus portal
- disable public access
- add cert using existing local self-signed certificate
- create the repo you need such as docker, maven and pypi
- update dasmon.json that located in /etc/docker. Add/Append saturn:30088 in the insecure-registries

### Jenkins
The jenkins support to build java and python projects

#### Local user/group
- Username (UID): cicd-jenkins (970)
- Group (GID): cicd (5000)

#### Storage
- Path: /mnt/saturn-cicd-sec/jenkins
- Size (GB): 150

#### Steps

Apply the jenkins yaml file
```
kubectl apply -f jenkins.yaml
```

#### Installed Tool
- Create secret file
- Additional Plugins
    - Configure Manager
- Maven (3.9.16)
    - Setting file for repo
    - Set Install automatically    
- Docker (29.7.2)
    - Set Install automatically

### Dependency-Track (API)
<To be filled>

#### Local user/group
- Username (UID): cicd-dependency-track (966)
- Group (GID): cicd (5000)

#### Storage
- Path: /mnt/saturn-cicd-sec/dependency-track
- Size (GB): 20

### Steps

Apply the dependency-track yaml fil
```
kubectl apply -f dependency-track.yaml
```