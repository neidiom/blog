---
layout: post
title: "Deploy azure-kafka-spark-adls via ARM template"
date:   2018-04-03 10:34:07 +0100
categories: azure azure-cli
---

* TOC
{:toc}


````
resourceGroup="arm-deploy-test"
location="westeurope"
````

````
az group create -l $location -n $resourceGroup
````

# Extract Service Principal Metadata
````
SP_APPID=$(az ad sp list --display-name $resourceGroup --query "[0].appId" -o tsv)
SP_OBJECTID=$(az ad sp list --display-name $resourceGroup --query "[0].objectId" -o tsv)
AAD_TENANT=$(az account show --query "tenantId" -o tsv)
````


# deploy, specifying all template parameters directly
````
az group deployment create \
    --debug \
    --name ARMTestDeployment \
    --resource-group $resourceGroup \
    --template-file  azure-kafka-spark-adls.json\
    --parameters 'clusterPassword=myvhds96' \
                 'aadTenantId=My5q1P@s5w0rd!' \
                 'servicePrincipalObjectId=mheath' \
                 'servicePrincipalApplicationId=Adm1nP@s5w0rd!' \
                 'servicePrincipalCertificateContents=mypublicip72'
````


or using variables

````
az group deployment create -g $CLUSTER_NAME --template-file azuredeploy.json --debug --parameters\
    clusterPassword="$CLUSTER_PASSWORD" \
    aadTenantId=$AAD_TENANT \
    servicePrincipalObjectId=$SP_OBJECTID \
    servicePrincipalApplicationId=$SP_APPID \
servicePrincipalCertificateContents="$CERT_BASE64"
````

# Links

## Resources

* [link](https://github.com/syedhassaanahmed/azure-kafka-spark-adls)
