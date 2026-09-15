# Application Deployment Screenshots

This document provides evidence of the successful deployment and working flow of the **DevOps Exam Platform** application.
The deployment flow includes container image management, Kubernetes deployment, external access, database connectivity, and application functionality.

---

---

## 01. AWS ECR Repository

Docker images for the frontend and backend applications are built and pushed to **Amazon Elastic Container Registry (ECR)**.
ECR is used as a private container image repository for storing application images before deployment to Kubernetes.

<img width="1365" height="634" alt="Amazon ECR Images" src="https://github.com/user-attachments/assets/ae255f2e-2f34-43a8-afd4-2b01f35956d3" />

---

## 02. AWS EKS Cluster

The application is deployed on an **Amazon Elastic Kubernetes Service (EKS)** cluster.

The worker nodes are running successfully and are ready to host application workloads.

<img width="666" height="97" alt="EKS Cluster Nodes" src="https://github.com/user-attachments/assets/1e494be1-a75c-420f-8c7f-da79ea6ec435" />

---

## 03. Kubernetes Deployment

Application components are deployed using Kubernetes resources including:

- Deployments
- Pods
- Services
- ConfigMaps/Secrets

Frontend, backend, and database components are running successfully inside the cluster.

<img width="1101" height="391" alt="Kubernetes Resources" src="https://github.com/user-attachments/assets/bc0e8aea-7c4f-4565-b1d1-f9065b508813" />

---

## 04. AWS Load Balancer

The application is exposed externally using a Kubernetes Service integrated with an AWS Load Balancer.

The Load Balancer provides external access to the frontend application.

<img width="1108" height="88" alt="AWS_Loadbalancer" src="https://github.com/user-attachments/assets/25d5ca59-baa7-4e42-9584-b57b7eff3f91" />

---

## 05. Database Connectivity

The backend application successfully establishes connectivity with the MySQL database.

Database credentials are securely managed using Kubernetes Secrets.

<img width="1338" height="648" alt="Database Connectivity_1" src="https://github.com/user-attachments/assets/cd2d7a52-bd72-496d-81eb-edcf671bf2ec" />

<img width="1361" height="652" alt="Database Connectivity_2" src="https://github.com/user-attachments/assets/3cdcfefd-85e5-439a-ac44-f59854920e1d" />


---

## 06. Database Records

User exam responses and results are successfully stored in the database.

Database verification was performed by checking the stored records.

<img width="547" height="586" alt="Database_1" src="https://github.com/user-attachments/assets/3e51194c-0b16-4f75-93f2-07469ebdbc4b" />


Example:

```sql
SELECT * FROM results;

---
SELECT * FROM results;

---

## 07. Application UI

The DevOps Exam Platform application is accessible through the Load Balancer endpoint.

<img width="1197" height="677" alt="Application_page" src="https://github.com/user-attachments/assets/2851136f-272d-4a28-8e2d-22b85d3d1705" />


---

## 08. Exam Results

After completing the exam, results are generated successfully and displayed to the user.

This confirms the complete application flow:

User → Frontend → Backend API → Database → Result

<img width="1160" height="669" alt="Result_page" src="https://github.com/user-attachments/assets/7490584f-de92-4321-be7b-3b7d273fb556" />
