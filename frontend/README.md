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

### OpenWebUI

To install OpenWebUI

```
kubectl apply -f frontend/ai/llm/openwebui.yaml
```

