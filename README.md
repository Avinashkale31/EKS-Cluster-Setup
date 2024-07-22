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
# On macOS using Homebrew
brew install awscli

# On Ubuntu/Debian
sudo apt-get update
sudo apt-get install awscli
```
Configure the AWS CLI with your credentials:
1. AWS Secret key
2. AWS Secret Password

```bash
aws configure
```

**2. Install kubectl**

```bash
# On macOS using Homebrew
brew install kubectl

# On Ubuntu/Debian
curl -LO "https://dl.k8s.io/release/v1.23.5/bin/linux/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```

**3. Install eksctl**

```bash
# On macOS using Homebrew
brew install eksctl

# On Ubuntu/Debian
curl --silent --location "https://github.com/weaveworks/eksctl/releases/download/0.110.0/eksctl_Linux_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
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
eksctl get cluster --name my-cluster --region us-east-2
```
Check the nodes
```bash
kubectl get nodes
```
