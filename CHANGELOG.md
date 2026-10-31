
# CHANGE LOG

v3.2.0 - Folder structure change, postgres upgrade, ci/cd components upgrade
- Change Backend Postgres Image to pgvector/pgvector:pg18 (Support Vector)
- Re-structure folder under gitops
  - Platform applications (e.g. postgres, rabbitmq, kafka, mongo) belongs to gitops/platforms
  - Project application belongs to gitops/apps
- CI/CD
  - Jenkins version from 2.580-jdk25 to 2.588-jdk25
  - Nexus version from 3.89.0 to 3.96.5

v3.1.0 - Install Apache Kafka for saturn-backend

v3.0.0 - Use Argo CD for saturn-frontend and saturn-backend

v2.0.0 - Frontend and Backend Applications for saturn-backend/saturn-frontend and DevOps
- Frontend
  - OpenWebUI
- Backend 
  - Persistence
    - Mongo
    - Postgres
    - Valkey
  - Message
    - RabbitMQ
  - AI
    - LLM 
      - Ollama
      - SearXNG
- GitOps
  - Setup steps for Argo CD

v1.0.0 - First full setup for saturn ci/cd locally
- Postgres
    - Major Persistence Storage
    - Store data for Forgejo and Dependency-Track
- Forgejo
    - Major Source Control System
- Jenkins
    - Major CI/CD workflow for Java and Python
    - Support Docker
- Nexus 
    - Support Java, Python and Docker
- Dependency Track (API)


