---
layout: post
title: "Deploy Docker containers with Azure CLI"
date:   2018-04-03 10:34:07 +0100
categories: azure azure-cli docker
---

* TOC
{:toc}

## Login

Log in to your account

````
az login
````

# Creating

## Create a resource group

```
az group create \
-n kakoje_acs_rg1 \
-l "westus"
````


## Create an ACS (Azure Container Service) cluster

```
az acs create \
--name kakoje-acs-cluster \
--resource-group kakoje_acs_rg1 \
--dns-prefix kakoje-app-9898
````

or use shorter syntax

````
az acs create \
-n kakoje-acs-cluster \
-g kakoje_acs_rg1 \
-d applink789
````

# Managing

## Manage ACS clusters


List clusters under a subscription

````
az acs list \
--output table
````


List clusters in a resource group

````
az acs list \
-g kakoje_acs_rg1 \
--output table
````


Display details of a container service cluster

````
az acs show \
-g kakoje_acs_rg1 \
-n kakoje-acs-cluster \
--output table
````
## Display details of a container service cluster

````
az acs show \
-g kakoje_acs_rg1 \
-n kakoje-acs-cluster \
--output jsonc
````

## Get username and **FQDN**

Now we get the username and **FQDN** in order to connect

````
az acs show \
-g kakoje_acs_rg1 \
-n kakoje-acs-cluster \
--output jsonc | grep 'user\|fqdn'
````

## Scale the cluster

````
az acs scale \
-g kakoje_acs_rg1 \
-n acs-cluster \
--new-agent-count 4
````

# Deleting

## Delete a container service cluster

````
az acs delete \
-g kakoje_acs_rg1 \
-n acs-cluster
````
## Delete Resource Group

````
az group delete \
-n kakoje_acs_rg1
````
# Links

## Resources

* [link](https://docs.microsoft.com/en-us/azure/container-service/dcos-swarm/container-service-create-acs-cluster-cli)
