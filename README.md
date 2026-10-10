# Saturn K8S Infrastructure 

It is about my new pc 'saturn' that I try to build my local development environment and application on it. It involves three major items

- CI/CD
    - Obviously, it is first thing to setup, a CI/CD environment that I could build application. Please see README.md in the cicd folder for more details
- Backend
    - It is about some of shared services to be used by applications such as RabbitMQ. Please see README.md in the backend folder for more details

- Application
    - It is a place I build my applications. They probably are built in form of docker image and deployed to the K8S. Please see README.md in the app folder for more details

### CI (folder: cicd)
Currently, the following are installed
- Forgejo
- Jenkins
- Nexus
- Dependency-track api

### CD (folder: gitops)
- Argo CD


### Backend (folder: backend)
- LLM
    - ollama
- Postgres
- Mongo
- Valkey
- Rabbitmq

### Frontend (folder: frontend)
- LLM 
    - OpenWebUI
