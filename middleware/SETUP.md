# Installation Guide for Saturn Middleware

The middleware is about the shared service for applications such as RabbitMQ, Redis, Mongo and Postgres

## Pre-requisite

### K8S

- Namespace
    - saturn-middleware
- Secrets
    - Each application may own one or more than one secret
- User permission
    - Try to use single user to own all middlewares


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