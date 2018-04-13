---
layout: post
title: "Azure: Creating a Service Principal for Authentication"
date:   2018-04-12 07:34:07 +0100
categories: azure service-principal authentication
---

* TOC
{:toc}

# List current Subscription information

We will need the **SubscriptionId** from the subscription we will add the new ServicePrincipal to.

````
az account list -o table
````
or just get the ID

````
az account list \
--query [].id -o tsv
````

## Set the `SubscriptionId` variable

Please set your **SubscriptionId** that you got from `az account list -o table` dommand.

````
SUBSCRIPTION_ID=0000000-0000-0000-0000-000000000000
````

## Set the `Service Principal` password variable

````
SP_PASSWORD="A_Add65f6b2010F3"
````

## Set the `Service Principal` name variable

````
SP_NAME="TerraFormServicePrincipal"
````

# Create Service Principal

RBAC stands for role based access control

````
az ad sp create-for-rbac \
--name=$SP_NAME \
--password=$SP_PASSWORD \
--role="Contributor" \
--scopes="/subscriptions/${SUBSCRIPTION_ID}"
````

## Create the Service Principal with a level access to a Resource Group only

### Set Resource Group variable

````
ResourceGroup=MyResGroup
````

### Create the Service Principal

````
az ad sp create-for-rbac \
--name=$SP_NAME \
--password=$SP_PASSWORD \
--role="Contributor" \
--scopes="/subscriptions/${SUBSCRIPTION_ID}/resourceGroups/${ResourceGroup}"
````
**Remember the password**

## Authenticate as the Service Principal

### Getting info authentication info from the Service Principal

We need to get fields below to authenticate our app.

* appId
* password
* tenant

## Set the variables required for login


```
APP_ID="YOUR_APP_ID"
TENANT_ID="YOUR_TENANT_ID"
````

## Login as the Service Principal

````
az login \
--service-principal \
-u $APP_ID \
-p $SP_PASSWORD \
--tenant $TENANT_ID
````

## Check the account type

After you login as Service Principal, the type should be **servicePrincipal**.

````
az account list
````
