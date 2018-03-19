---
layout: post
title: "Azure CLI Cheat Sheet"
date:   2018-03-14 11:25:07 +0100
categories: azure azure-cli cheatsheet
---

## List images

````
az vm image list --output table
````

## Lis resources

````
az resource list -o table
````

## List role assignment

````
az role assignment list -o table
````

## List access

````
az role assignment list --assignee "ASSIGNEE-PRINCIPAL-NAME"
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

# Grant permission

````
az role assignment create --role "Owner" --assignee <Service_Principal>
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


# Delete

````
az vm delete -n MyVM -g MyResourceGroup
````

````
az group delete -n MyGroup
````
