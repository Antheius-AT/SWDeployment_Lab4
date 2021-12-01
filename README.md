# SWDeployment_Lab4

# Install Azure CLI if not already installed by running the following command in an elevated powershell terminal:

"$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'; rm .\AzureCLI.msi"

# Install Kubernetes command-line client (Kubectl) by running the following command (requires Azure CLI tool installed locally)
az aks install-cli

# Create a resource group
az group create --name SWDeployment --location germanywestcentral

# Whatever
az provider register --namespace Microsoft.OperationsManagement
az provider register --namespace Microsoft.OperationalInsights

# Create AKS Cluster
az aks create --resource-group SWDeployment --name SWDeploymentAKSCluster --node-count 1 --generate-ssh-keys

# Create kustomization.yaml file
File contains the following:
secretGenerator:
- name: mysql-pass
  literals:
  - password=YOUR_PASSWORD

Where "YOUR_PASSWORD" is to be changed to whatever password you plan to use

# Connect to Cluster
az aks get-credentials --resource-group SWDeployment --name SWDeploymentAKSCluster

# Verify that cluster is running with one active node
kubectl get nodes

# Download mysql and wordpress yaml files as specified here: 
https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/

# Add downloaded yaml files as resources to kustomization.yaml file
resources:
  - mysql-deployment.yaml
  - wordpress-deployment.yaml


# Apply (Careful that indentation in kustomization.yaml file is correct, otherwise it throws errors!!)
kubectl apply -k ./

# Validate that secret generation worked
kubectl get secrets

# Verify, that persistent volume was created
kubectl get pvc

# Verify that pod is Running
kubectl get pods

# Verify that service is running
kubectl get services wordpress

# URL
20.79.227.245

# Screenshots

![Wordpress Site](/Screenshots/Cluster.png?raw=true "The cluster")
![Wordpress Site](/Screenshots/Website.png?raw=true "The website")

