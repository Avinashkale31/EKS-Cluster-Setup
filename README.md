# EKS-Cluster-Setup
<image src="./Assets/EKS-CLuster.png">
Creating an AWS EKS (Elastic Kubernetes Service) cluster involves several steps, including setting up the necessary AWS IAM roles, creating the EKS cluster, configuring kubectl, and deploying applications. Below is a detailed guide to help you create an AWS EKS cluster. You can use this guide to create a document to upload to GitHub. Note that you'll need to replace placeholders with your specific values.

Creating an AWS EKS Cluster
Prerequisites
AWS CLI: Install and configure the AWS Command Line Interface (CLI).
kubectl: Install kubectl, the command-line tool for Kubernetes.
eksctl: Install eksctl, a simple command-line utility for creating and managing EKS clusters.

Step 1: Configure AWS CLI
Make sure you have configured your AWS CLI with your credentials:

aws configure

Step 2: Create IAM Role
Create an IAM role with the required policies for EKS. You can do this through the AWS Management Console or using the AWS CLI.

Step 3: Create an EKS Cluster
Use eksctl to create the EKS cluster. This command creates a cluster with one node group:

eksctl create cluster --name my-cluster --region us-west-2 --nodegroup-name my-nodes --node-type t3.medium --nodes 3

Output:

css
Copy code
[ℹ]  eksctl version 0.41.0
[ℹ]  using region us-west-2
[ℹ]  setting availability zones to [us-west-2a us-west-2b us-west-2c]
...
Step 4: Configure kubectl
Once the cluster is created, configure kubectl to use the new cluster:

bash
Copy code
aws eks --region us-west-2 update-kubeconfig --name my-cluster
Output:

ruby
Copy code
Added new context arn:aws:eks:us-west-2:123456789012:cluster/my-cluster to /Users/username/.kube/config
Step 5: Verify the Cluster
Verify that your cluster is up and running:

bash
Copy code
kubectl get svc
Output:

scss
Copy code
NAME             TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes       ClusterIP   10.100.0.1   <none>        443/TCP   1m
Step 6: Deploy a Sample Application
Create a deployment and a service for a sample application:

yaml
Copy code
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
yaml
Copy code
# service.yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
Apply the deployment and service:

bash
Copy code
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
Output:

bash
Copy code
deployment.apps/nginx-deployment created
service/nginx-service created
Step 7: Access the Application
Get the external IP of the service to access the application:

bash
Copy code
kubectl get svc nginx-service
Output:

scss
Copy code
NAME            TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)        AGE
nginx-service   LoadBalancer   10.100.200.1   aabbccddeeffgghh  80:32000/TCP   2m
Open a web browser and navigate to the EXTERNAL-IP to see the nginx welcome page.

Step 8: Clean Up
Delete the cluster when you are done to avoid incurring costs:

bash
Copy code
eksctl delete cluster --name my-cluster
Output:

python
Copy code
[ℹ]  deleting EKS cluster "my-cluster"
...
