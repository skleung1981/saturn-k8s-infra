
### Please follow the below steps

```
openssl req -x509 -newkey rsa:2048 -nodes -keyout saturn-local.key.pem -out saturn-local.cert.pem -days 3650
cat saturn-local.key.pem saturn-local.cert.pem > saturn-local.full.pem
```

For import it locally
```
sudo keytool -importcert -alias saturn-nexus -file saturn-local.cert.pem -keystore /usr/lib/jvm/java-25-openjdk/lib/security/cacerts -storepass changeit -noprompt  
```

For import it into K8S
```
kubectl create secret tls saturn-ingress-tls --cert=saturn-local.cert.pem --key=saturn-local.key.pem -n saturn-cicd
```