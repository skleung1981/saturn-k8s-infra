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

```
kubectl apply -f postgres.yaml
```

### No-SQL - Mongo

```
kubectl apply -f mongo.yaml
```

### Memory Based - Redis

```
kubectl apply -f redis.yaml
```

## Message Service

### RabbitMQ

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
