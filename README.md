# Saturn K8S Infrastructure 

**This repo is originally from my local CI/CD environment using forgejo, this one you're viewing is the mirror version in GitHub.**

It is about my new pc 'saturn' that I try to build my local CI/CD environment and applications on it. It involves four major items

- CI/CD
  - Obviously, it is first thing to setup, a CI/CD environment that I could build application. 
  - Please see README.md in the cicd folder for more details
- Backend
  - Not only some of shared backend services such as RabbitMQ but also individual applications are located here. 
  - Please see README.md in the backend folder for more details
- Frontend
  - It is a place I will build my applications. They probably are built in form of docker image and deployed to the K8S. 
  - Please see README.md in the app folder for more details
- DevOps/GitOps
  - Argo CD is used for the whole GitOps process.

### CI (folder: cicd)
Currently, the following are installed
- Forgejo 
- Jenkins
- Nexus
- Dependency Track (API)
- Postgres (For Forgejo and Dependency Track)

### CD (folder: gitops, namespace: saturn-gitops)
This is used to manage the application under saturn-backend and saturn-frontend. Applications under saturn-cicd is excluded.
- Argo CD

### Backend (folder: backend, namespace: saturn-backend)
- AI
  - LLM
    - Ollama
    - SearXNG 
- Postgres
- Mongo
- Valkey
- Rabbitmq
- Kafka
- Applications 
  - UK Weather Services

### Frontend (folder: frontend, namespace: saturn-frontend)
- AI
  - LLM
    - OpenWebUI
