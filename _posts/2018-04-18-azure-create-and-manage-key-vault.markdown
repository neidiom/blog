---
layout: post
title: "Azure: Create and Manage Key Vault"
date:   2018-04-18 07:34:07 +0100
categories: azure key vault
---

* TOC
{:toc}

# Login

````
az login \
-u username@domain.com \
-p password
````

or login interactively

````
az login
````

# Set Variables


## Set RG name

````
ResourceGroup="DenimResourceGroup"
````

## Set location

````
Location="westeurope"
````
## Set KeyVault variable

````
KeyVault="DenimKeyVault"
````


## Set KeyName

````
KeyName="DenimKey"
````

# Create Resources and Keys

## Create a new resource group

````
az group create \
-n $ResourceGroup \
-l $Location
````

## Register the Key Vault Resource Provider

````
az provider register \
-n Microsoft.KeyVault
````

## Check status of provider, if Registered

````
az provider show\
 -n Microsoft.KeyVault \
 -o table
````

## Create a Key Vault

````
az keyvault create \
--name $KeyVault \
--resource-group $ResourceGroup \
--location $Location
````

## List KeyVaults

````
az keyvault list -o table
````

## Show info on KeyVault


````
az keyvault show -n $KeyVault
````

## Get value of vaultUri

````
az keyvault show \
-n $KeyVault | grep vaultUri
````

## Add a key or secret to the key vault

````
az keyvault key create \
--vault-name $KeyVault \
--vault-name $KeyVault \
--protection software
````

OR import a .pem key


````
az keyvault key import \
--vault-name $KeyVault \
--vault-name $KeyVault \
--pem-file './softkey.pem' \
--pem-password 'PaSSWORD' \
--protection software
````

## Add a secret to the Vault

````
az keyvault secret set \
--vault-name $KeyVault \
--name 'SQLPassword' \
--value 'Pa$$w0rd'
````


## To view your key, type:

````
az keyvault key list \
--vault-name $KeyVault
````

## To view your secret, type:

````
az keyvault secret list \
--vault-name 'ContosoKeyVault'
````
