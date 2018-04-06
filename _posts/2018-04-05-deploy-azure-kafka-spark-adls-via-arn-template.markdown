---
layout: post
title: "Deploy azure-kafka-spark-adls via ARM template"
date:   2018-04-03 10:34:07 +0100
categories: azure azure-cli
---

* TOC
{:toc}


````
ResourceGroup="arm-deploy-test"
location="westeurope"
````

````
az group create -l $location -n $ResourceGroup
````

# Create Service Principal with certificate
Certificate is not created if Service Principal already exists

````
PEM_FILE=$(az ad sp create-for-rbac --name $ResourceGroup --create-cert --query "fileWithCertAndPrivateKey" -o tsv)
````

# Export pem to pfx with password

````
openssl pkcs12 -export <$PEM_FILE -out $ResourceGroup.pfx -password pass:$CLUSTER_PASSWORD
````

# Set CERT_BASE64

````
CERT_BASE64=$(base64 $ResourceGroup.pfx)
````


# Extract Service Principal Metadata
````
SP_APPID=$(az ad sp list --display-name $ResourceGroup --query "[0].appId" -o tsv)
SP_OBJECTID=$(az ad sp list --display-name $ResourceGroup --query "[0].objectId" -o tsv)
AAD_TENANT=$(az account show --query "tenantId" -o tsv)
````


# deploy, specifying all template parameters directly
````
az group deployment create \
    --debug \
    --name ARMTestDeployment \
    --resource-group $ResourceGroup \
    --template-file  azure-kafka-spark-adls.json \
    --parameters 'clusterPassword=00000000000000' \
                 'aadTenantId=00000000000000' \
                 'servicePrincipalObjectId=00000000000000' \
                 'servicePrincipalApplicationId=00000000000000' \
                 'servicePrincipalCertificateContents=mypublicip00000'
````


### Using Shell Variables

password must be 6-72 characters long and must contain at least one digit, one upper case letter and one lower case letter

````
CLUSTER_PASSWORD=""
````

````
az group deployment create \
    --resource-group $ResourceGroup \
    --template-file azure-kafka-spark-adls.json \
    --debug \
    --parameters \
    clusterPassword="$CLUSTER_PASSWORD" \
    aadTenantId=$AAD_TENANT \
    servicePrincipalObjectId=$SP_OBJECTID \
    servicePrincipalApplicationId=$SP_APPID \
    servicePrincipalCertificateContents="$CERT_BASE64"
````

# Links

## Resources

* [link](https://github.com/syedhassaanahmed/azure-kafka-spark-adls)
