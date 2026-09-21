# Installation Guide for Saturn Backend Components

The backend is about the shared service for applications such as RabbitMQ, Redis, Mongo and Postgres

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
    mkdir postgres mongo redis rabbitmq kafka ollama
    sudo groupadd -g 5002 saturn-backend
    sudo useradd -u 965 -g saturn-backend saturn-backend-user
    sudo chown -R saturn-backend-user:saturn-backend saturn-backend
    ```

## Persistence Storage

### SQL - Postgresql 

Create a secret  and apply the postgres yaml file
```
kubectl create secret generic postgres-user-pass \
--namespace saturn-backend \
--from-literal=username=<Your username> \
--from-literal=password='<Your Password>'

kubectl apply -f backend/persistence/postgres.yaml
```

### No-SQL - Mongo

```
kubectl create secret generic mongo-user-pass \
--namespace saturn-backend \
--from-literal=username=<Your username> \
--from-literal=password='<Your Password>'

kubectl apply -f backend/persistence/mongo.yaml
```

### Memory Based - Valkey

```
kubectl create secret generic valkey-user-pass \
--namespace saturn-backend \
--from-literal=username=<Your username> \
--from-literal=password='<Your Password>'


kubectl apply -f backend/persistence/valkey.yaml
```

## Message Service

### RabbitMQ

```
kubectl create secret generic rabbitmq-user-pass \
--namespace saturn-backend \
--from-literal=username=<Your username> \
--from-literal=password='<Your Password>'

kubectl apply -f backend/message/rabbitmq.yaml
```

### Kafka


## LLM Service

### Ollama

To install Ollama

```
kubectl apply -f backend/llm/ollama.yaml
```

After the pod is running, run the following command to load model

```
kubectl -n saturn-backend exec -it deploy/ollama -- ollama pull gpt-oss:20b
```

```
kubectl -n saturn-backend exec -it deploy/ollama -- ollama pull gpt-oss:20b

or

kubectl -n saturn-backend exec -it deploy/ollama -- ollama pull gemma4:e4b
```
