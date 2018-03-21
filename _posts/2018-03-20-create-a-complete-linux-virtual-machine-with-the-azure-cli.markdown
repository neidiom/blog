---
layout: post
title: "Create a complete Linux virtual machine with the Azure CLI"
date:   2018-03-20 10:34:07 +0100
categories: azure azure-cli
---

* TOC
{:toc}

# SETUP Shell Environment

## 1.0 Set environment variables

* Set prefix variable

````
AZ_PREFIX=Hadzos
````
* Set username

````
ADMIN_USERNAME=hadzo
````

* Set Resource Group Name

````
RES_GROUP=HadzosResourceGroup
````

* Set location

````
LOCATION=westeurope
````

### 1.1 Test if shell variables were set

* Test if **AZ_PREFIX** variable was set

````
echo $AZ_PREFIX
````

* Test if **ADMIN_USERNAME** variable was set

````
echo $ADMIN_USERNAME
````


* Test if **RES_GROUP** variable was set

```
echo $RES_GROUP
````

* Test if **LOCATION** variable was set

````
echo $LOCATION
````

# Resource Group and Location

## 2.0 Create resource group

* An Azure resource group is a logical container into which Azure resources are deployed and managed.

* A resource group must be created before a virtual machine and supporting virtual network resources.

````
az group create \
--name "${RES_GROUP}" \
--location "${LOCATION}"
````

### 2.1 Test if group was created

We will get list of groups in table format and filter by Resource Group name

````
az group list -o table | grep "${RES_GROUP}"
````

or in `jsonc` format

````
az group list -o jsonc | grep ${RES_GROUP}
````


### 2.2 Get group list and query by location

````
az group list --query "[?location=='"${LOCATION}"']"
````

or manually provide location name

````
az group list --query "[?location=='westeurope']"
````

### 2.3 Get help on `group` options

````
az group -h
````

# Networking

## 3.0 Create a virtual network and subnet

````
az network vnet create \
    --resource-group "${RES_GROUP}" \
    --name "${AZ_PREFIX}"Vnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name "${AZ_PREFIX}"Subnet \
    --subnet-prefix 192.168.1.0/24
````

## 3.1 Get help on **vnet**

````
az network vnet -h
````

## 4.0 Create a public IP address

This public IP address enables you to connect to your VMs from the Internet. Because the default address is dynamic, create a named DNS entry with the --domain-name-label parameter. The following example creates a public IP named "${AZ_PREFIX}"PublicIP with the DNS name of "${AZ_PREFIX}"publicdns. Because the DNS name must be unique, provide your own unique DNS name:


````
az network public-ip create \
    --resource-group "${RES_GROUP}" \
    --name "${AZ_PREFIX}"PublicIP \
    --dns-name "${AZ_PREFIX}"publicdns
````

### 4.1 List newly created public IP Address

````
az network public-ip list \
--resource-group "${RES_GROUP}" | grep ipAddress
````

### 4.2 List newly created FQDN

````
az network public-ip list \
--resource-group "${RES_GROUP}" | grep fqdn
````

## 4.3 Get help on **public-ip**

````
az network public-ip -h
````


## 5.0 Create a network security group

To control the flow of traffic in and out of your VMs, you apply a network security group to a virtual NIC or subnet.

### 5.1 Create **nsg**

````
az network nsg create \
    --resource-group "${RES_GROUP}" \
    --name "${AZ_PREFIX}"NetworkSecurityGroup
````

You define rules that allow or deny specific traffic. To allow inbound connections on port 22 (to enable SSH access), create an inbound rule

### 5.2 Create inbound **nsg rule** for port `22`


````
az network nsg rule create \
    --resource-group "${RES_GROUP}" \
    --nsg-name "${AZ_PREFIX}"NetworkSecurityGroup \
    --name "${AZ_PREFIX}"NetworkSecurityGroupRuleSSH \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 22 \
    --access allow
````

To allow inbound connections on port 80 (for web traffic), add another network security group rule. The following example creates a rule named "${AZ_PREFIX}"NetworkSecurityGroupRuleHTTP:

### 5.3 Create inbound **nsg rule** for port `80`


````
az network nsg rule create \
    --resource-group "${RES_GROUP}" \
    --nsg-name "${AZ_PREFIX}"NetworkSecurityGroup \
    --name "${AZ_PREFIX}"NetworkSecurityGroupRuleWeb \
    --protocol tcp \
    --priority 1001 \
    --destination-port-range 80 \
    --access allow
````

### 5.4 Examine **nsg**


Examine the network security group and rules with az network nsg show:


````
az network nsg show --resource-group "${RES_GROUP}" \
--name "${AZ_PREFIX}"NetworkSecurityGroup
````

### 5.5 Get help on **nsg**

````
az network nsg -h
````


## 6.0 Create a virtual NIC

Virtual network interface cards (NICs) are programmatically available because you can apply rules to their use. Depending on the VM size, you can attach multiple virtual NICs to a VM. In the following az network nic create command, you create a NIC named "${AZ_PREFIX}"Nic and associate it with your network security group. The public IP address "${AZ_PREFIX}"PublicIP is also associated with the virtual NIC.

````
az network nic create \
    --resource-group "${RES_GROUP}" \
    --name "${AZ_PREFIX}"Nic \
    --vnet-name "${AZ_PREFIX}"Vnet \
    --subnet "${AZ_PREFIX}"Subnet \
    --public-ip-address "${AZ_PREFIX}"PublicIP \
    --network-security-group "${AZ_PREFIX}"NetworkSecurityGroup
````

### 6.1 Get help on **nic**

````
az network nic -h
````

# AvailabilitySet

## 7.0 Create an availability set


Availability sets help spread your VMs across fault domains and update domains. Even though you only create one VM right now, it's best practice to use availability sets to make it easier to expand in the future.


````
az vm availability-set create \
    --resource-group "${RES_GROUP}" \
    --name "${AZ_PREFIX}"AvailabilitySet
````

### 7.1 Get help on **availability-set**

````
az vm availability-set -h
````

# VM

## 8.0 Create a VM

You've created the network resources to support Internet-accessible VMs. Now create a VM and secure it with an SSH key. In this example, let's create an Ubuntu VM based on the most recent LTS.

Create the VM by bringing all the resources and information together with the az vm create command.


````
az vm create \
    --resource-group "${RES_GROUP}" \
    --name "${AZ_PREFIX}"VM \
    --location "${LOCATION}" \
    --availability-set "${AZ_PREFIX}"AvailabilitySet \
    --nics "${AZ_PREFIX}"Nic \
    --image UbuntuLTS \
    --admin-username "${ADMIN_USERNAME}" \
    --ssh-key-value ~/.ssh/id_rsa.pub
````

### 8.1 Connect to VM

Be sure to set the correct **FQDN**

````
ssh "${ADMIN_USERNAME}"@mypublicdns.eastus.cloudapp.azure.com
````

### 8.2 Get help on **vm create**

````
az vm create -h
````


# Extra

## 9.0 Export as a template

What if you now want to create an additional development environment with the same parameters, or a production environment that matches it?


````
az group export --name "${RES_GROUP}" > "${RES_GROUP}".json
````

To create an environment from your template

````
az group deployment create \
    --resource-group "${AZ_PREFIX}"NewResourceGroup \
    --template-file "${RES_GROUP}".json
````
# Links

## 10.0 Reference

* [Link to MS Docs article](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/create-cli-complete)
