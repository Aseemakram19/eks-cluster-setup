How to create an EKS cluster using AWS Console | Create node group | Configure Kubernetes cluster


# Step - 1 : Create EKS Management Host in AWS #


#To install an EKS (Elastic Kubernetes Service) cluster with 2 nodes in AWS, follow these steps:


Prerequisites
1. AWS Account: Make sure you have an AWS account and configure one ec2 instance
2. kubectl: Install kubectl. 
3. AWS CLI: Install and configure the AWS CLI.
4. eksctl: Install eksctl.

Step-1

1) Launch new Ubuntu VM using AWS Ec2 ( t2.micro )	


2) Connect to machine and install kubectl using below commands  
```
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version --short --client
```
3) Install AWS CLI latest version using below commands 
```
sudo apt install unzip
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
```

4) Install eksctl using below commands
```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
```
# Step - 2 : Create IAM role & attach to EKS Management Host #

1) Create New Role using IAM service ( Select Usecase - ec2 ) 	
2) Add below permissions for the role <br/>
	- IAM - fullaccess <br/>
	- VPC - fullaccess <br/>
	- EC2 - fullaccess  <br/>
	- CloudFomration - fullaccess  <br/>
	- Administrator - acces <br/>
		
3) Enter Role Name (eksroleec2) 
4) Attach created role to EKS Management Host (Select EC2 => Click on Security => Modify IAM Role => attach IAM role we have created) 

# Step - 3 : Create EKS Cluster using eksctl # 
**Syntax:** 

eksctl create cluster --name cluster-name  \
--region region-name \
--node-type instance-type \
--nodes-min 2 \
--nodes-max 2 \ 
--zones <AZ-1>,<AZ-2>

## N. Virgina: <br/>
`
eksctl create cluster --name cloudaseem-cluster4 --region us-east-1 --node-type t2.medium  --zones us-east-1a,us-east-1b
`	
## Mumbai: <br/>
`
eksctl create cluster --name cloudaseem-cluster4 --region ap-south-1 --node-type t2.medium  --zones ap-south-1a,ap-south-1b
`

## Note: Cluster creation will take 5 to 10 mins of time (we have to wait). After cluster created we can check nodes using below command.

A. Verify the Cluster
After the cluster is created, verify that kubectl is configured to use the new cluster:

kubectl get svc

NAME             TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
kubernetes       ClusterIP   10.100.0.1    <none>        443/TCP   1m


B. Check the Nodes
Check that the nodes are up and running:
`
 kubectl get nodes  
`

NAME                            STATUS   ROLES    AGE     VERSION
ip-192-168-xx-xx.ec2.internal   Ready    <none>   5m      v1.17.12-eks-7684af
ip-192-168-xx-xx.ec2.internal   Ready    <none>   5m      v1.17.12-eks-7684af


You have successfully created an EKS cluster with 2 nodes in AWS using eksctl. You can now deploy applications to your Kubernetes cluster.

## Note: We should be able to see EKS cluster nodes here.**

# We are done with our Setup #
	
## Step - 4 : After your practise, delete Cluster and other resources we have used in AWS Cloud to avoid billing ##

```
eksctl delete cluster --name cloudaseem-cluster4 --region ap-south-1
```
