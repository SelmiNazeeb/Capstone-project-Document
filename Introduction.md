Deployment of a three tier application in aws eks cluster.
Application Name : **TaskFlow**
App containing a Frontend, backend and postgresql Database

**💡 About TaskFlow**
TaskFlow is a simple task management application designed to help you organize your daily activities. It leverages a robust three-tier architecture for scalability and reliability:
Actions u can do  : create, store  and delete task
**Frontend**: Built with React and Material-UI, providing an intuitive user interface.

**Backend**: A Node.js/Express API handles task creation, retrieval, updates, and deletion.

**Database**: Stores your tasks persistently, ensuring data integrity

* infrastructure created through
      * region 1 : cloud formation
      * region 2 : Terraform
* Deployment of application in **EKS cluster**
* Database used : **RDS Postgresql**
* Automation through : **Code build, Code deploy, Code pipeline**
* Load balancer : **Network load balancer**
* routing : **Route 53 failure routing**

Git repo with code of application
https://github.com/SelmiNazeeb/FinalProject-Devops.git

## Project Structure
```
FinalProject-Devops/
├── frontend/                   # React frontend application
├── backend/                    # Node.js backend API
├── k8s/                        # Kubernetes manifests
├── terraform_infra/            # terraform infrastructure
└── cft.yaml                    # cloud formation infrastructure
|__ buildspec.yaml              # cloud formation deploy
|__ buildspec-terradeploy.yaml  # terraform deploy      
|__ terraform-buildspec.yaml    # terraform infrastructure build 
|__ database/                   # posgresql database

**files in the repo**
* backend, frontend, database
* cloud formation infrastrure yaml - cft.yaml
* cloud formation deployment buildspec - buildspec.yml
* terraform infrastructure yaml - terraform_infra/*
* terraform infra buildspec - terraform-buildspec.yaml
* terraform deployment buildspec - buildspec-terradeploy.yaml 

