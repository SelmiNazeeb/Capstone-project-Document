Deployment of a three tier application in aws eks cluster.
Application Name : **TaskFlow**
App containing a Frontend, backend and postgresql Database

**💡 About TaskFlow**
TaskFlow is a simple task management application designed to help you organize your daily activities. It leverages a robust three-tier architecture for scalability and reliability:
Actions u can do  : create, store  and delete task
**Frontend**: Built with React and Material-UI, providing an intuitive user interface.

**Backend**: A Node.js/Express API handles task creation, retrieval, updates, and deletion.

**Database**: Stores your tasks persistently, ensuring data integrity

> infrastructure created through
      *region 1 : cloud formation
      *region 2 : Terraform
> Deployment of application in EKS cluster
> Database used : RDS Postgresql
> Automation through : Code build, Code deploy, Code pipeline
> Load balancer : Network load balancer
> routing : Route 53 failure routing

