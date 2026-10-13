# Installation Guide for Saturn Backend Components

The backend is about the shared services such as RabbitMQ, Valkey, Mongo and Postgres.

## Pre-requisite

### K8S

- Namespace
    - saturn-backend
        ```
        kubectl create namespace saturn-backend
        ```

- Secrets
    - Each application may own one or more than one secret
        
- User permission
    - Try to use single user to own all backend components
    ```
    mkdir -p /opt/saturn-backend/
    cd /opt/saturn-backend/
    mkdir postgres mongo valkey rabbitmq kafka ollama
    sudo groupadd -g 5002 saturn-backend
    sudo useradd -u 965 -g saturn-backend saturn-backend-user
    sudo chown -R saturn-backend-user:saturn-backend saturn-backend
    ```

## Persistence Storage

### SQL - Postgresql 

Create a secret and apply the postgres yaml file (Do once only)
```
kubectl create secret generic postgres-user-pass \
--namespace saturn-backend \
--from-literal=username=<Your username> \
--from-literal=password='<Your Password>'

kubectl apply -f backend/persistence/postgres.yaml
```

### No-SQL - Mongo

Create a secret and apply the mongo yaml file (Do once only)
```
kubectl create secret generic mongo-user-pass \
--namespace saturn-backend \
--from-literal=username=<Your username> \
--from-literal=password='<Your Password>'

kubectl apply -f backend/persistence/mongo.yaml
```

### Memory Based - Valkey

Create a secret and apply the valkey yaml file (Do once only)
```
kubectl create secret generic valkey-user-pass \
--namespace saturn-backend \
--from-literal=username=<Your username> \
--from-literal=password='<Your Password>'

kubectl apply -f backend/persistence/valkey.yaml
```

## Message Service

### RabbitMQ

Create a secret and apply the rabbitmq yaml file (Do once only)
```
kubectl create secret generic rabbitmq-user-pass \
--namespace saturn-backend \
--from-literal=username=<Your username> \
--from-literal=password='<Your Password>'

kubectl apply -f backend/message/rabbitmq.yaml
```

### Kafka
**TBC**

## LLM Service

### Ollama

To install Ollama

```
kubectl apply -f backend/ai/llm/ollama.yaml
```

After the pod is running, run the following command to load model (e.g. gpt-oss:20b)

```
kubectl -n saturn-backend exec -it deploy/ollama -- ollama pull gpt-oss:20b
```

## Search Service
### Searxng

It is required when access internet by OpenWebUI is required.

To install Searxng, please do the following steps

Generate a secret key first (Do once only)
```
kubectl create secret generic searxng-secret \
  --from-literal=SEARXNG_SECRET="$(openssl rand -hex 32)" \
  -n saturn-backend
```

Apply the YAML file
```
kubectl apply -f backend/ai/search/searxng.yaml
```