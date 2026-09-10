
# Create a namespace (saturn-cicd)

# Import a self-signed cert for ingress to k8s (secret)
-- In fact, you already install a nginx ingress before
kubectl create secret tls saturn-ingress-tls --cert=saturn-local.cert.pem --key=saturn-local.key.pem -n saturn-cicd

# Update the nginx ingress so it read the secret
kubectl edit deployment my-ingress-ingress-nginx-controller -n ingress-nginx
- --default-ssl-certificate=saturn-cicd/saturn-ingress-tls


# deploy applications
## Postgres

kubectl create secret generic postgres-user-pass \
--namespace saturn-cicd \
--from-literal=username=pg_admin \
--from-literal=password='supersecret123456'

kubectl create secret generic postgres-user-pass \
--namespace saturn-cicd \
--from-literal=username=postgres_admin \
--from-literal=password='supersecret123456'

semanage fcontext -a -t container_file_t "/opt/saturn-cicd/postgres(/.*)?"
sudo restorecon -Rv /opt/saturn-cicd/postgres

kubectl apply -f postgres.yaml

kubectl logs 

## Forgejo 

kubectl apply -f forgejo.yaml


## Nexus

sudo semanage fcontext -a -t container_file_t "/opt/saturn-cicd/nexus(/.*)?"
sudo restorecon -R -v /opt/saturn-cicd/nexus

kubectl apply -f nexus.yaml

- disable public access
- create a local user - skl
- add cert using existing local self-signed certificate
- create a new repo for docker, rpm, pypi (TBC)


## Jenkins

sudo semanage fcontext -a -t container_file_t "/opt/saturn-cicd/jenkins(/.*)?"
sudo restorecon -R -v /opt/saturn-cicd/jenkins

kubectl apply -f jenkins.yaml

Docker
version:
Plugin
How many file

### Maven 

#### Add tls (self-signed cert) to saturn jvm (java 25)
/usr/lib/jvm/java-25-openjdk
sudo keytool -importcert -alias saturn-nexus -file saturn-local.cert.pem -keystore /usr/lib/jvm/java-25-openjdk/lib/security/cacerts -storepass changeit -noprompt


### Dependency tack
