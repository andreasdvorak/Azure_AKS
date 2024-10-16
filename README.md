# Preparations
## Azure CLI
The following did not work for me.

lsb_release -cs shows virginia but there is no virginia on the web site

https://packages.microsoft.com/repos/azure-cli/dists/

https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-linux?pivots=apt


This did work

    curl -L https://aka.ms/InstallAzureCli | bash

## az extension
I don´t know why but without the aks-preview extention I get an error setting up the AKS cluster

    az extension add -n aks-preview
    az extension list
    az version

## az login
To run the terraform apply you first need to login at Azure

    az login

# Code
## terraform.tfvars
put in here all secret values

This file should not be put in git, because it contains secrets.

Check .gitignore

## client access values
To access Azure with Terraform you need a client_id client_secret

Create a user e.g. terraform in https://entra.microsoft.com

In the overview you get the client id

To to certifcates & secrets and create a secret client key. Copy the value.

## locals.tf
put in here all values, but no secrets

# Terraform commands
## initialze Terraform
    terraform init -upgrade

## create a Terraform execution plan
    terraform plan -var-file="terraform.tfvars"

## execute Terraform
    terraform apply -var-file="terraform.tfvars"

## delete all remote objets
    terraform destroy