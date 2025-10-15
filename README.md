# Azure Kubernetes
Set up a small and free Kubernetes Cluster in Azure with Terraform

You can only define the number of worker. The number of Control Planes is created by Azure automatically redundant.

# Terraform installation
https://developer.hashicorp.com/terraform/install#linux

# Preparations
## Azure CLI
Installation of Azure CLI

    curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash

## az login
To run the terraform apply you first need to login at Azure

    az login

## Get Azure Regions
    az account list-locations -o table

## List Subscriptions
    az account list

# Code
## client access values
To access Azure with Terraform you need a client_id and client_secret

Create a Service Principal e.g. terraform in https://entra.microsoft.com

    az ad sp create-for-rbac --name "terraform-sp" --role="Contributor" --scopes="/subscriptions/<SUBSCRIPTION_ID>"


    {
    "appId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",         # → Client ID
    "displayName": "terraform-sp",
    "password": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",       # → Client Secret
    "tenant": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"          # → Tenant ID
    }

In the Azure portal the SP can be seen in Microsoft Entra ID -> App registrations

## terraform.tfvars
Put in here all secret values in terraform.tfvars

client_id       = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

client_secret   = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

tenant_id       = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

subscription_id = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

This file should not be put in git, because it contains secrets.

Check .gitignore

## Test of SP with az login

    az login --service-principal \
            --username <APP_ID> \
            --password <CLIENT_SECRET> \
            --tenant <TENANT_ID>

## locals.tf
put in here all values, but no secrets

# Terraform commands
## initialze Terraform
    terraform init [-upgrade]

## Terraform Validate
    terraform validate

## create a Terraform execution plan
    terraform plan -var-file="terraform.tfvars"

## execute Terraform
    terraform apply -var-file="terraform.tfvars"

## delete all remote objets
    terraform destroy

# AKS Administration
## Get kubeconfig
Login with az

    az login

Get kubeconfig

    az aks get-credentials --resource-group <RESOURCE_GROUP_NAME> --name <AKS_CLUSTER_NAME>

## Test

    kubectl get nodes

## Created resource groups and resources

The resource group rg-<resource_group_name> has been defined in the Terraform code. Here is only one resource for the Kubernetes service.

The resource group MC_<resource_group_name>_<AKS-Name>_<Region> is created automatically by Azure. You can only see it, but not remove it.





