---
layout: post
title: "Azure CLI Cheat Sheet"
date:   2018-03-14 11:25:07 +0100
categories: azure azure-cli cheatsheet
---

* TOC
{:toc}


# Roles

````
az role definition list
````

````
az role definition list -o table
````

````
az role definition list --output json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
````

````
az role definition list --name "Contributor"
````
## Grant permission

````
az role assignment create --role "Owner" --assignee <Service_Principal>
````

# Role assignment

--assignee
Represent a user, group, or service principal. supported format: object id, user sign-in name, or service principal name.

--assignee-object-id:
Assignee's graph object id, such as the 'principal id' from a managed service identity. Use this instead of '--assignee' to bypass graph permission issues.

````
az role assignment create \
--assignee-object-id 1d500143-702b-4ed8-a0cc-c46a1f29de1b \
--role Contributor
````

# List Roles for an Assignee

````
az role assignment list \
--assignee 1d500143-702b-4ed8-a0cc-c46a1f29de1b \
-o table
````
# Delete a Role for an Assignee

````
az role assignment delete \
--assignee 1d500143-702b-4ed8-a0cc-c46a1f29de1b \
--role Reader
````

# Listing

## Get info about VM from a Resource Group

````
az vm list --resource-group $ResourceGroup
````

````
 az vm list --resource-group $ResourceGroup | grep 'id'
````

## List images

````
az vm image list --output table
````

## List resources

````
az resource list -o table
````

## List role assignment

````
az role assignment list -o table
````

### List role assignment and filter for `name` or `principalName`

````
az role assignment list | grep 'name\|principalName'
````

### List role assignment and filter for `name` or `principalName` or `principalId`

````
az role assignment list | grep 'name\|principalName\|principalId'
````

## List access

````
az role assignment list --assignee "ASSIGNEE-PRINCIPAL-NAME"
````
## Generate an ARM template from an existing resource group

````
az group export -n kakoje_acs_rg1
````

## List VM usage in a region

````
az vm list-usage --location westeurope -o table
````

# Locks

````
az lock create --name ReadOnlyLock \
--resource-group HadzosResourceGroup \
--lock-type CanNotDelete
````

# Network

## List VNets

````
az network vnet list -o table
````

## List Public IPs

````
az network public-ip list -o table
````

## List Network Security Group

````
az network nsg list -o table
````

## List NIC

````
az network nic list -o table
````

# List resources in a group

````
az resource list -g MyHadzoGroupName
````

# List resources in a group and filter for name or type

````
 az resource list -g HadzoGroup | grep "name\|type"
 ````

# List VMs

````
az vm list
````

## List groups

* as json

````
az group list
````

* as table

````
az group list --output=table
````

* List locations

````
az account list-locations -o table
````

* Create group in `westeurope` region

````
az group create -n HadzoGroup -l "westeurope"
````

#### Show information for a specific group

````
az group show -n HadzoGroup
````

* show only id of the group

````
az group show -n HadzoGroup --query id -o tsv
````

# Creating

## Creating VM's

````
az vm create \
-n MyVM \
-g HadzoGroup \
--image "UbuntuLTS" \
--size Standard_DS2_v2
````

or

````
az vm create \
--name MyVM \
--resource-group HadzoGroup \
--image "UbuntuLTS" \
--size Standard_DS2_v2 \
--admin-username "hadzo" \
--ssh-key-value ~/.ssh/id_rsa.pub
````


# Deletig

## Deleting Resource Groups

````
az vm delete -n MyVM -g MyResourceGroup
````

````
az group delete -n MyGroup
````

````
az group delete --name MyGroup --yes --no-wait
````
