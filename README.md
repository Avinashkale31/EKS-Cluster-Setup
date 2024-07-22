# EKS-Cluster-Setup
<image src="./Assets/EKS-CLuster.png">

**Prerequisites**
1. AWS CLI: Make sure you have the AWS Command Line Interface (CLI) installed and configured with appropriate IAM permissions.
2. kubectl: The Kubernetes command-line tool, kubectl, should be installed.
3. eksctl: A command-line utility for managing EKS clusters.
   
**Step-by-Step Installation**

**1. Install AWS CLI**

If you haven’t installed the AWS CLI yet, you can do so by following the installation guide.
```bash
# On Ubuntu/Debian
sudo apt update
sudo apt-get install unzip curl -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version

```
Configure the AWS CLI with your credentials:
1. AWS Secret key
2. AWS Secret Password

```bash
aws configure
```

**2. Install kubectl**

```bash
# On Ubuntu/Debian
curl -LO "https://dl.k8s.io/release/v1.23.5/bin/linux/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version --client
```

**3. Install eksctl**

```bash
# On Ubuntu/Debian
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" --output eksctl.tar.gz
tar -xzf eksctl.tar.gz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
```

**4. Create an EKS Cluster**
```bash
eksctl create cluster \
  --name my-cluster \
  --region us-west-2 \
  --nodes 3 \
  --nodes-min 1 \
  --nodes-max 4 \
  --node-type t3.medium \
  --managed
```
This command creates a cluster named my-cluster in the us-west-2 region with a minimum of 1 and a maximum of 4 nodes of type t3.medium.

**5. Update kubectl Configuration**
eksctl automatically updates your kubeconfig file, but you can manually update it if needed:
```bash
aws eks update-kubeconfig --region us-west-2 --name my-cluster
```
**6. Verify the Cluster**
Check that your cluster is up and running:
```bash
eksctl get cluster --name my-cluster --region us-west-2
```
Check the nodes
```bash
kubectl get nodes
```

**7. Deleting the EKS Cluster**
```bash
eksctl delete cluster --name my-cluster --region us-east-2
```
