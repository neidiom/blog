---
layout: post
title: "Azure CLI Cheat Sheet"
date:   2018-03-14 11:25:07 +0100
categories: azure azure-cli cheatsheet
---

# List images

````
az vm image list --output table
````

# Lis resources

````
az resource list -o table
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

* Creating VM's

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
