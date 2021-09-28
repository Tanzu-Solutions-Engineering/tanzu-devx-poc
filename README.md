# Tanzu DevX POC

## Overview
Tanzu Developer Experience PoC can be excuted on any CNCF conformant Kubernetes cluster. In this particular example, we are using Azure Kubernetes Service (AKS). We use **Github Actions** to set up PoC environment and deploy all required Tanzu DevX components.

## Prerequisites
Before beginning to run Github Action pipeline, ensure that you have completed all following steps:
- **An Azure AKS cluster**
- **An Azure Storage Account**: Harbor will use Azure Storage Account to store images and charts.
- **Secrets**: In your Github project, go to `Settings --> Secrets --> New repository secret`. Create following secrets:
    - **AZURE_ACCESS**: Azure credential is requried to authenticate with Azure.
    - **AZURE_STORAGE_ACCOUNT_KEY**: Azure Storage Account Key
    - **HARBOR_USERNAME**: The admin username will be used in Harbor instllation and access Harbor later.
    - **HARBOR_PASSWORD**: The admin password will be used in Harbor instllation and access Harbor later.
    - **TANZU_NETWORK_API_TOKEN**: You can find API TOKEN from the your Tanzu Network account profile. 
    - **TANZU_NETWORK_USERNAME**: Your username of Tanzu Network. Will be used download artifacts and retrieve images from Tanzu Network.  
    - **TANZU_NETWORK_PASSWORD**: Your password of Tanzu Network. Will be used download artifacts and retrieve images from Tanzu Network.  
<br>
- **Set Environment Variables in [Github Action pipeline](https://github.com/Tanzu-Solutions-Engineering/tanzu-devx-poc/blob/main/.github/workflows/deploy_tanzu_dev.yml)**:  

    ```bash
    env:
        # Azure Resource Group
        CLUSTER_RESOURCE_GROUP: MorganStanleyPOC
  
        # Azure AKS cluster name
        CLUSTER_NAME: TanzuCluster

        # Customized domain name (i.e. example.com) which will be used to access Harbor(registry.example.com) or Kubeapps(kubeapps.example.com).
        DOMAIN_NAME: ms.hyan.us

        # Azure Storage Account Name
        AZURE_STORAGE_ACCOUNT_NAME: msharbor

        # kpack CLI Linux, you can find it from https://network.pivotal.io/products/build-service/
        KP_CLI_LINUX_RELEASE: https://network.pivotal.io/api/v2/products/build-service/releases/925788/product_files/1000629/download
        
        # Tanzu Build Service (https://network.pivotal.io/products/build-service/)
        TBS_VERSION: 1.2.2
        
        # Tanzu Build Service Dependencies (https://network.pivotal.io/products/tbs-dependencies/)
        TBS_DEPENDENCIES_VERSION: 100.0.150

        # Tanzu Build Service Dependencies, descriptor-xxx.x.xxx.yaml (https://network.pivotal.io/products/tbs-dependencies/)
        TBS_DEPENDENCIES: https://network.pivotal.io/api/v2/products/tbs-dependencies/releases/943744/product_files/1022067/download
        
        # Application Accelerator (https://network.pivotal.io/products/app-accelerator/)
        ACC_VERSION: 0.3.0

        # OOTB samples of Application Accelerator (https://network.pivotal.io/products/app-accelerator/)
        ACC_SAMPLE_FILE: sample-accelerators-0-3.yaml
        ACC_SAMPLE_DOWNLOAD: https://network.pivotal.io/api/v2/products/app-accelerator/releases/965559/product_files/1049989/download

    ```
    
## Steps in Github Actions Pipeline
1. Set up job
2. Checkout the latest code from main
3. Install Kubectl tool
4. Install Helm tool
5. Login to Azure
6. Set the target Azure Kubernetes Service (AKS) cluster
7. Add and Update Helm repos
8. Install Cert-Manager
9. Install Contour ingress controller
10. Add DNS record
11. Create Cluster Issuer for ingress controller using Letsencrypt
12. Install Harbor registry
13. Install Kubeapps
14. Install Carvel CLI tools (kapp, ytt, kbld, imgpkg and vendir)
15. Install Kpack CLI (kp)
16. Install Tanzu Build Service
17. Import Tanzu Build Service Dependencies
18. Install Application Accelerator
19. Install OOTB Applicatio Accelerator Samples
