# 📌 Project Overview

The **DevOps Exam Platform** is a cloud-native three-tier web application deployed on **Amazon Elastic Kubernetes Service (EKS)**.

The objective of this project was to design and deploy a production-like application environment using modern DevOps practices including:

- Containerization using Docker
- Container image management using Amazon ECR
- Kubernetes orchestration using Amazon EKS
- Secure database connectivity using Amazon RDS
- Kubernetes deployments and services
- Application scalability and high availability

This project demonstrates how a real-world application can be deployed, managed, and operated on Kubernetes in AWS.

# 🏗️ Application Architecture
``````````````````````````````````````````
                         Users
                           |
                           |
                  AWS Load Balancer
                           |
                           |
                 Frontend Kubernetes Service
                           |
                           |
                  Frontend Docker Pods
                           |
                           |
                 Backend Kubernetes Service
                           |
                           |
                  Backend Application Pods
                           |
                           |
                  Amazon RDS MySQL Database

``````````````````````````````````````````````
**## Architecture Components**
---------------------------------------------------------------------------------------------------------------------------------------------------------------
| Layer               | Technology               | Purpose                                                                                                     |
| ------------------- | ------------------------ | ----------------------------------------------------------------------------------------------------------- |
| Frontend            | Docker + Nginx           | Hosts and serves the user interface application as a containerized web application                          |
| Backend             | Docker + Application API | Provides business logic, handles API requests, and communicates with the database                           |
| Container Registry  | Amazon ECR               | Stores and manages Docker container images used for Kubernetes deployment                                   |
| Kubernetes Platform | Amazon EKS               | Provides container orchestration, scaling, high availability, and workload management                       |
| Database            | Amazon RDS MySQL         | Stores application data such as users, questions, exam responses, and results                               |
| Networking          | AWS VPC                  | Provides isolated network infrastructure, subnets, security groups, and communication between AWS resources |
| Deployment Tool     | kubectl + eksctl         | Used to create/manage Kubernetes clusters and deploy application workloads                                  |
----------------------------------------------------------------------------------------------------------------------------------------------------------------

# 🛠️ Technology Stack
## Cloud
- Amazon EC2 ---(Provides Linux server environment for deployment and management)
- Amazon EKS ---(Manages Kubernetes cluster for application orchestration)
- Amazon ECR ---(Stores and manages Docker container images)
- Amazon RDS ---(Provides managed MySQL database service)
- Amazon VPC ---(Provides secure network infrastructure)
- AWS IAM ---(Manages users, roles, and permissions )

## Containers
- Docker  ---(Containerizes application components)
- Docker Images ---(Packages application code and dependencies)
- Docker Registry --- (Stores and distributes container images)

## Kubernetes
- Pods  
  - Runs application containers (Frontend and Backend services)

- Deployments  
  - Manages application replicas, rolling updates, and ensures desired pod state

- Services  
  - Enables communication between application components inside the cluster

- Namespaces  
  - Provides resource isolation and organizes Kubernetes resources

- Secrets  
  - Securely stores sensitive information such as database credentials (DB host, username, password, and database name)
  - Injects database configuration into backend pods using environment variables

- LoadBalancer Service  
  - Exposes the application externally through an AWS Elastic Load Balancer

- Amazon EKS Worker Nodes  
  - Provides compute capacity to run application workloads inside the Kubernetes cluster

## Tools
- kubectl ---(Manages Kubernetes resources)
- eksctl ---(Creates and manages EKS clusters)
- AWS CLI ---(Interacts with AWS services through command line)
- Git ---(Version control and source code management)
--------------------------------------------------------------------

# 🔄 Application Deployment Flow
````
Developer 
    |
    |
GitHub Repository
    |
    |
Docker Build
    |
    |
Docker Image
    |
    |
Amazon ECR
    |
    |
Amazon EKS Cluster
    |
    |
Kubernetes Deployment
    |
    |
Application Running

```
---

# 📂 Project Structure

```
DevOps-Exam-Platform

│
├── frontend
│   ├── Dockerfile
│   ├── nginx.conf
│   └── admin.html
│   ├── exam.html
│   └── index.html
|   └── result.html
 
│
├── backend
|   ├── template
|   |   ├── admin.html
|   |   ├── exam.html
|   |   ├── index.html
|   |   ├── result.html
│   ├── Dockerfile
│   ├── app.py
|   ├── questions.py
│   └── requirements.txt
│

├── backend-deployment.yaml
├── frontend-deployment.yaml
├── services.yaml
└── README.md

```
# 🚀 Deployment Implementation
------------------------------------------------------------------------------------
## 1. Clone Repository

```bash
git clone https://github.com/Rishav-1743/DevOps-Exam-Platform.git
cd DevOps-Exam-Platform
```
--------------------------------------------------------------------------------------
# 2. Containerization Using Docker
## Build Frontend Image

```bash
cd frontend
docker build -t devops-exam-frontend .
```

## Build Backend Image

```bash
cd backend
docker build -t devops-exam-backend .
```

Verify images:

```bash
docker images
```

------------------------------------------------------------------------------------
# 3. Push Images to Amazon ECR

Authenticate Docker with ECR:
```bash
aws ecr get-login-password \
--region us-east-1 | \
docker login \
--username AWS \
--password-stdin <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com
```

Tag Images:

```bash
docker tag devops-exam-frontend:latest \
<ACCOUNT_ID>.dkr.ecr.ap-south-1.amazonaws.com/devops-exam-frontend:latest
```

Push:
```bash
docker push \
<ACCOUNT_ID>.dkr.ecr.ap-south-1.amazonaws.com/devops-exam-frontend:latest
```

The same process was followed for the backend image.
-------------------------------------------------------------------------------------

# 4. Provision Kubernetes Cluster
Created an **Amazon Elastic Kubernetes Service (EKS)** cluster using the **eksctl** command-line tool. This automatically provisioned the Kubernetes control plane, worker nodes, VPC networking, IAM roles, and other required AWS resources.
### Create the EKS Cluster

```bash
eksctl create cluster \
  --name exam-platform \
  --region ap-south-1 \
  --nodegroup-name worker-nodes \
  --node-type t3.medium \
  --nodes 2 \
  --nodes-min 2 \
  --nodes-max 4 \
  --managed
Create cluster:

### Verify Cluster Creation

```bash
eksctl get cluster
kubectl get nodes

```bash
kubectl get nodes
--------------------------------------------------------------------------

# 5. Configure kubectl
Connect kubectl with EKS:

```bash
aws eks update-kubeconfig \
--name exam-platform \
--region us-east-1
```
-----------------------------------------------------------------------

# 6. Create Kubernetes Namespace

Created isolated namespace:

```bash
kubectl create namespace exam-platform
```

Benefits:

- Application isolation
- Better resource management
- Easier troubleshooting

-----------------------------------------------------------------------------------------

# 7. Database Integration

Database was deployed using:
## Amazon RDS MySQL

Configuration:
```
Database Engine:
MySQL

Database Name:
exam-platform-db

Authentication:
Username/Password
```
Database connectivity was configured securely using Kubernetes Secrets.


Create secret:


```bash
kubectl create secret generic db-secret \
--from-literal=username=admin \
--from-literal=password=password
```
--------------------------------------------------------------------------------------

# 8. Deploy Backend Application

Backend deployment includes:

- Docker image from ECR
- Replica management
- Container ports
- Environment variables

Deployment:
```bash
kubectl apply -f backend-deployment.yaml
```
Verify:
```bash
kubectl get pods
```
-------------------------------------------------------------------

# 9. Deploy Backend Service
Backend was exposed internally using ClusterIP service.

Purpose:
Frontend Pod
       |
       |
Backend Service
       |
       |
Backend Pod

```
Apply:
```bash
kubectl apply -f backend-service.yaml
```
--------------------------------------------------------------------

# 10. Deploy Frontend Application
Frontend deployment:

```bash
kubectl apply-f frontend-deployment.yaml
```

Frontend pods are responsible for serving the user interface.
-----------------------------------------------------------------

# 11. Expose Application Using LoadBalancer
Frontend service:

```yaml
type: LoadBalancer
```

AWS automatically provisions:
AWS Elastic Load Balancer
        |
        |
Frontend Service
        |
        |
Frontend Pods

```
Check:

```bash
kubectl get svc
```
Application becomes accessible through AWS Load Balancer endpoint.

---------------------------------------------------------------------------

# 🔐 Security Implementation

## IAM
Implemented AWS IAM based access control.

## Kubernetes Secrets
Sensitive database credentials are stored securely.

## Security Groups
Configured controlled communication between:

- EKS Worker Nodes
- RDS Database

---

# 📈 Kubernetes Features Implemented

## Deployment Management
Used Kubernetes Deployments for:

- Application availability
- Replica management
- Rolling updates
----------------------------------------------------

## Service Discovery
Implemented Kubernetes Services:

```
Frontend
   |
Backend Service
   |
Backend Pods

```
--------------------------------------------------------------------------

## High Availability
Multiple replicas were configured:

```
Frontend

Replica 1
Replica 2


Backend

Replica 1
Replica 2

```
-----------------------------------------------------------------------------

# 🧪 Troubleshooting Performed
## Check Pods
```bash
kubectl get pods
```

## View Logs
```bash
kubectl logs <pod-name>
```

## Describe Resources
```bash
kubectl describe pod <pod-name>
```

## Check Services
```bash
kubectl get svc
```

---

# 🎯 Challenges Faced and Solutions
## Challenge 1: Frontend unable to communicate with Backend

Problem:

```
host not found upstream backend
```
Solution:

- Configured Kubernetes Service DNS
- Updated backend service name
- Verified internal communication

---

## Challenge 2: Database Connectivity Issue


Problem:
Backend unable to connect with MySQL.

Solution:

- Verified RDS Security Group
- Checked database credentials
- Validated network connectivity

---

## Challenge 3: Container Troubleshooting
Used:

```bash
docker logs

kubectl logs

kubectl describe

```
to identify application failures.

-------------------------------------------------------------------------
# 💡 Key Learnings
Through this project, I gained hands-on experience with:

✅ AWS EKS Cluster Management  
✅ Docker Containerization  
✅ Amazon ECR Image Management  
✅ Kubernetes Application Deployment  
✅ Kubernetes Networking  
✅ Cloud Database Integration  
✅ Application Troubleshooting  
✅ Production Deployment Practices  
---
- Docker
- CI/CD
- Terraform
- Linux
