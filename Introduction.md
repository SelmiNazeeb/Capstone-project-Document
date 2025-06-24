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
```

## Steps
```
1. create infrastructure (cloud formation and terraform in 2 region)
2. Build and Push Docker Images to ecr registry
3. Applying manifest files
4. Deploy to AWS EKS
5. buy domain from route 53
6. Route 53 for failover routing
 7. cloud watch monitoring
```

##cloud watch steps
 1. Create IAM Policy for CloudWatch Agent
```
 cat <<EOF > cwagent-policy.json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:PutLogEvents",
        "logs:DescribeLogStreams",
        "logs:DescribeLogGroups",
        "logs:CreateLogStream",
        "logs:CreateLogGroup",
        "logs:PutRetentionPolicy",
        "cloudwatch:PutMetricData"
      ],
      "Resource": "*"
    }
  ]
}
EOF

aws iam create-policy \
  --policy-name CloudWatchAgentServerPolicy \
  --policy-document file://cwagent-policy.json
```
2. Create IAM Service Account for CloudWatch Agent
```
eksctl create iamserviceaccount \
  --cluster <your-cluster-name> \
  --namespace amazon-cloudwatch \
  --name cloudwatch-agent \
  --attach-policy-arn arn:aws:iam::<your-account-id>:policy/CloudWatchAgentServerPolicy \
  --approve \
  --region us-east-1
```
 3. Create Namespace
```
kubectl create namespace amazon-cloudwatch
```
4. Apply the CloudWatch Agent ConfigMap
```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: ConfigMap
metadata:
  name: cwagentconfig
  namespace: amazon-cloudwatch
  labels:
    app: cloudwatch-agent
data:
  cwagentconfig.json: |
    {
      "agent": {
        "region": "us-east-1"
      },
      "logs": {
        "metrics_collected": {
          "kubernetes": {
            "cluster_name": "eks-cluster-2",
            "metrics_collection_interval": 60
          }
        },
        "force_flush_interval": 5
      }
    }
EOF
```
5. Deploy the CloudWatch Agent DaemonSet
```
kubectl apply -f https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/master/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-cloudwatch-agent.yaml
kubectl get pods -n amazon-cloudwatch
```
6. Verify in CloudWatch Console
```
Go to CloudWatch → Container Insights → Performance Monitoring
Go to CloudWatch → Logs → Log groups
   - Log groups will appear like /aws/containerinsights/eks-cluster-2/...
```





