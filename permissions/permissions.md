**Cloud formation**
Infrastructure
•	Cloud formation role - AdministratorAccess
•	Pipeline role - AmazonEKSClusterPolicy, AWSCloudFormationFullAccess
•	Permission to user to access EKS cluster
    - Select EKS cluster -> Access -> Add access entry -> give user arn -> next -> add “AmazonEKSClusterAdminPolicy “ -> save  

Application deployment
•	Code build role - AmazonEC2ContainerRegistryPowerUser
•	pipeline role – Access entry in EKS cluster

**Terraform**
Infrastructure 
•	Code build role - terraform/terraform-build-policy, AmazonS3FullAccess, AmazonVPCFullAccess
•	Permission to user to access EKS cluster
    - Select EKS cluster -> Access -> Add access entry -> give user arn -> next -> add “AmazonEKSClusterAdminPolicy “ -> save  

Application deployment
•	Code build role - AmazonEC2ContainerRegistryPowerUser
•	pipeline role – Access entry in EKS cluster




