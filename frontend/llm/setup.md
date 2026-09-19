# Installation Guide for Saturn Frontend Components

The frontend is about the services for applications such as OpenWebUI

## Pre-requisite

### K8S

- Namespace
    - saturn-frontend
        ```
        kubectl create namespace saturn-frontend
        ```

- Secrets
    - Each application may own one or more than one secret
        
- User permission
    - Try to use single user to own all frontend components
    ```
    mkdir -p /opt/saturn-frontend/
    cd /opt/saturn-frontend/
    mkdir openwebui
    sudo groupadd -g 5001 saturn-frontend
    sudo useradd -u 964 -g saturn-frontend saturn-frontend-user
    sudo chown -R saturn-frontend-user:saturn-frontend saturn-frontend
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
