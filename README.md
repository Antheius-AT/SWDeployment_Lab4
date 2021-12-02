# SWDeployment_Lab4
## Setup of required tools

### Step 1) Install Azure CLI if not already installed
Achieve this by running the following command in an elevated powershell:

**$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'; rm .\AzureCLI.msi**

### Step 2) Install Kubernetes command-line client (Kubectl)
This can be achieved with the following command:

**az aks install-cli**

Running this command requires Azure CLI to be installed

### Step 3) Create a resource group
To create a resource group, run the following command, using your own name and location:

**az group create --name SWDeployment --location germanywestcentral**

I am choosing to use germany as location. 

## Setting up the cluster

### Step 4) Create AKS Cluster
Run the following command:

**az aks create --resource-group SWDeployment --name SWDeploymentAKSCluster --node-count 1 --generate-ssh-keys**

This creates the cluster with one node

### Step 5) Create kustomization.yaml file
The file can be found in the scripts folder. A screenshot of the file can be found in the screenshots section
You are required to create this file by yourself, the required contents however can be found in tutorials.

## Setting up the connection to the cluster

### Step 6) Connect to Cluster
Achieved by running the following command:

**az aks get-credentials --resource-group SWDeployment --name SWDeploymentAKSCluster**

### Step 7) Verify that cluster is running with one active node
Run the following command:

**kubectl get nodes**

In this case, the command should return one node and the status should indicate that the node is active and running

### Step 8) Download mysql and wordpress yaml files
Files can be found here:

https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/

### Step 9) Add downloaded yaml files as resources to kustomization.yaml file
This is done by appending the following statement to the kustomization.yaml file:

resources:
  - mysql-deployment.yaml
  - wordpress-deployment.yaml

Ensure that these file are located in the same folder as the kustomization.yaml file.

### Step 10) Apply and publish the cluster
Can be done with the following command:

**kubectl apply -k ./**

Ensure that shell is running in the folder which the necessary script files are located in.

## Validation

### Validate that secret generation worked
Run the following command:

**kubectl get secrets**

This command should return the generated secret

### Verify, that persistent volume was created
This is achieved with the following:

**kubectl get pvc**

### Verify that pod is Running
This can be done by issuing:

**kubectl get pods**

### Verify that service is running
To check whether the service is online, execute command:

**kubectl get services wordpress**

This, among other data, returns a URL under which the website is reachable.
In my particular case, the URL is: 

**20.79.227.245**

## Notes
Currently the site is not up and running, to save money, as the screenshots demonstrate and prove that I set it up properly.
Should you require me to run the cluster for you to visit the site and double check, I will gladly start the cluster again.

## Screenshots

### Kustomization.yaml file
![Wordpress Site](/Screenshots/kustomization.yaml.png?raw=true "The cluster")

### Cluster
![Wordpress Site](/Screenshots/Cluster.png?raw=true "The cluster")

### Website
![Wordpress Site](/Screenshots/Website.png?raw=true "The website")

