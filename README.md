# SWDeployment_Lab4
## Setup of required tools

### Step 1) Install Azure CLI if not already installed by running the following command in an elevated powershell terminal:

"$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'; rm .\AzureCLI.msi"

### Step 2) Install Kubernetes command-line client (Kubectl) by running the following command (requires Azure CLI tool installed locally)
az aks install-cli

### Step 3) Create a resource group
To create a resource group, run the following command, using your own name and location:
az group create --name SWDeployment --location germanywestcentral

## Setting up the cluster

### Step 4) Create AKS Cluster
az aks create --resource-group SWDeployment --name SWDeploymentAKSCluster --node-count 1 --generate-ssh-keys

### Step 5) Create kustomization.yaml file
The file can be found in the scripts folder. A screenshot of the file can be found in the screenshots section

## Setting up the connection to the cluster

### Step 6) Connect to Cluster
az aks get-credentials --resource-group SWDeployment --name SWDeploymentAKSCluster

### Step 7) Verify that cluster is running with one active node
kubectl get nodes
This should return one node which status should be active

### Step 8) Download mysql and wordpress yaml files as specified here: 
https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/

### Step 9) Add downloaded yaml files as resources to kustomization.yaml file
resources:
  - mysql-deployment.yaml
  - wordpress-deployment.yaml


### Step 10) Apply and publish the cluster
Can be done with the following command:
kubectl apply -k ./
Ensure that shell is running in the folder which the necessary script files are located in.

## Validation

### Validate that secret generation worked
kubectl get secrets

### Verify, that persistent volume was created
kubectl get pvc

### Verify that pod is Running
kubectl get pods

### Verify that service is running
kubectl get services wordpress

### URL
20.79.227.245

## Screenshots

### Kustomization.yaml file
![Wordpress Site](/Screenshots/kustomization.yaml.png?raw=true "The cluster")

### Cluster
![Wordpress Site](/Screenshots/Cluster.png?raw=true "The cluster")

### Website
![Wordpress Site](/Screenshots/Website.png?raw=true "The website")

