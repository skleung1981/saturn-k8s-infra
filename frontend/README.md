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

## OpenWebUI Service
OpenWebUI here is considered as frontend application. 

### OpenWebUI

To install OpenWebUI, do the following steps

Generate a secret key first (Do once only)
```
kubectl create secret generic openwebui-secret \
  --from-literal=OPENWEBUI_SECRET="$(openssl rand -hex 32)" \
  -n saturn-frontend
```

Apply the YAML file
```
kubectl apply -f frontend/ai/llm/openwebui.yaml
```

