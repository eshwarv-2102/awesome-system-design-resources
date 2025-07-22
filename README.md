 System Design:  File Sharing System

 1. Functional Requirements
    
These are the core user-facing features:
Upload a file (any format)
Download/view a file
Share file via link or user email
View file list & metadata (size, date, type)
Delete/rename files

2. Non-Functional Requirements

| Metric              | Value                              |
| ------------------- | ---------------------------------- |
| **Availability**    | 99.99% uptime                      |
| **Scalability**     | Up to 100 million users            |
| **Latency**         | <200ms for metadata fetch          |
| **Durability**      | 99.999999999% (11 9s, like AWS S3) |
| **Security**        | TLS, OAuth, role-based access      |
| **Cost Efficiency** | Auto-scale infra, use cold storage |



3.  High-Level Architecture

   
   
 Architecture Layers:
Client Layer

Web App / Mobile App

Auth, file picker, preview

API Gateway

Rate limiting, input validation, TLS termination

Load Balancer

NGINX or AWS ALB, distributes to app servers

App Layer (Backend)

Auth microservice

File metadata service

File sharing & permission service

Upload handler

Object Storage

AWS S3 / GCS for storing actual files

Lifecycle rules to move cold files to cheaper storage

Metadata Database

PostgreSQL / DynamoDB stores filename, owner, permissions

Message Queue (Async)

Kafka/SQS → Thumbnail generation, virus scan, etc.

Cache Layer

Redis to cache metadata or file previews

CDN

CloudFront/Akamai for file downloads (fast delivery)

Monitoring & CI/CD

Prometheus + Grafana

Jenkins + Helm + K8s


4.** Capacity Planning**

| Item            | Estimate                      |
| --------------- | ----------------------------- |
| Daily uploads   | 1M files/day × 5MB = 5TB      |
| Monthly storage | \~150TB (with some deletions) |
| Peak RPS        | 10,000 API req/sec            |
| Redis memory    | 10GB for hot metadata         |
| App replicas    | 20–30 pods (Kubernetes)       |
| DB shard count  | 2–3 (PostgreSQL or DynamoDB)  |
| Queue workers   | Auto-scaled: 10–50 workers    |


5. DevOps & Operational Excellence

   | Area                   | Tool/Practice                        |
| ---------------------- | ------------------------------------ |
| **CI/CD**              | GitHub Actions + Docker + Helm + K8s |
| **Monitoring**         | Prometheus + Grafana                 |
| **Logging**            | EFK/ELK Stack                        |
| **Secrets Mgmt**       | HashiCorp Vault / K8s Secrets        |
| **Deployments**        | Canary / Blue-Green                  |
| **Security**           | HTTPS, OAuth2, IAM Policies          |
| **Auto-scaling**       | HPA on CPU/RPS                       |
| **Storage Durability** | S3 with versioning + lifecycle       |
| **Disaster Recovery**  | Backup to Glacier or cold storage    |
