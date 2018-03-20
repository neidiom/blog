---
layout: post
title: "Create a complete Linux virtual machine with the Azure CLI"
date:   2018-03-20 10:34:07 +0100
categories: azure azure-cli
---
## Set environment variables

* Set prefix variable

````
AZPREFIX=Hadzos
````
* Set username

````
ADMIN_USERNAME=hadzo
````

* Set Resource Group Name

````
RESGRP="HadzosResourceGroup
````

* Test if RESGRP variable was set

```
echo $RESGRP
````
* Set location

````
LOCATION=westeurope
````
* Set if LOCATION variable was set

````
echo $LOCATION
````


## Create resource group

* An Azure resource group is a logical container into which Azure resources are deployed and managed.

* A resource group must be created before a virtual machine and supporting virtual network resources.

````
az group create \
--name "${RESGRP}" \
--location "${LOCATION}"
````

## Create a virtual network and subnet

````
az network vnet create \
    --resource-group "${RESGRP}" \
    --name "${AZPREFIX}"Vnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name "${AZPREFIX}"Subnet \
    --subnet-prefix 192.168.1.0/24
````
# Create a public IP address

This public IP address enables you to connect to your VMs from the Internet. Because the default address is dynamic, create a named DNS entry with the --domain-name-label parameter. The following example creates a public IP named "${AZPREFIX}"PublicIP with the DNS name of "${AZPREFIX}"publicdns. Because the DNS name must be unique, provide your own unique DNS name:


````
az network public-ip create \
    --resource-group "${RESGRP}" \
    --name "${AZPREFIX}"PublicIP \
    --dns-name "${AZPREFIX}"publicdns
````

## List newly created public IP Address

````
az network public-ip list \
--resource-group "${RESGRP}" | grep ipAddress
````

## List newly created FQDN

````
az network public-ip list \
--resource-group "${RESGRP}" | grep fqdn
````


# Create a network security group

To control the flow of traffic in and out of your VMs, you apply a network security group to a virtual NIC or subnet.

````
az network nsg create \
    --resource-group "${RESGRP}" \
    --name "${AZPREFIX}"NetworkSecurityGroup
````

You define rules that allow or deny specific traffic. To allow inbound connections on port 22 (to enable SSH access), create an inbound rule


````
az network nsg rule create \
    --resource-group "${RESGRP}" \
    --nsg-name "${AZPREFIX}"NetworkSecurityGroup \
    --name "${AZPREFIX}"NetworkSecurityGroupRuleSSH \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 22 \
    --access allow
````

To allow inbound connections on port 80 (for web traffic), add another network security group rule. The following example creates a rule named "${AZPREFIX}"NetworkSecurityGroupRuleHTTP:


````
az network nsg rule create \
    --resource-group "${RESGRP}" \
    --nsg-name "${AZPREFIX}"NetworkSecurityGroup \
    --name "${AZPREFIX}"NetworkSecurityGroupRuleWeb \
    --protocol tcp \
    --priority 1001 \
    --destination-port-range 80 \
    --access allow
````

Examine the network security group and rules with az network nsg show:


````
az network nsg show --resource-group "${RESGRP}" \
--name "${AZPREFIX}"NetworkSecurityGroup
````


## Create a virtual NIC

Virtual network interface cards (NICs) are programmatically available because you can apply rules to their use. Depending on the VM size, you can attach multiple virtual NICs to a VM. In the following az network nic create command, you create a NIC named "${AZPREFIX}"Nic and associate it with your network security group. The public IP address "${AZPREFIX}"PublicIP is also associated with the virtual NIC.

````
az network nic create \
    --resource-group "${RESGRP}" \
    --name "${AZPREFIX}"Nic \
    --vnet-name "${AZPREFIX}"Vnet \
    --subnet "${AZPREFIX}"Subnet \
    --public-ip-address "${AZPREFIX}"PublicIP \
    --network-security-group "${AZPREFIX}"NetworkSecurityGroup
````

## Create an availability set


Availability sets help spread your VMs across fault domains and update domains. Even though you only create one VM right now, it's best practice to use availability sets to make it easier to expand in the future.


````
az vm availability-set create \
    --resource-group "${RESGRP}" \
    --name "${AZPREFIX}"AvailabilitySet
````

## Create a VM

You've created the network resources to support Internet-accessible VMs. Now create a VM and secure it with an SSH key. In this example, let's create an Ubuntu VM based on the most recent LTS.

Create the VM by bringing all the resources and information together with the az vm create command.


````
az vm create \
    --resource-group "${RESGRP}" \
    --name "${AZPREFIX}"VM \
    --location "${LOCATION}" \
    --availability-set "${AZPREFIX}"AvailabilitySet \
    --nics "${AZPREFIX}"Nic \
    --image UbuntuLTS \
    --admin-username "${ADMIN_USERNAME}" \
    --ssh-key-value ~/.ssh/id_rsa.pub
````

````
ssh hadzo@mypublicdns.eastus.cloudapp.azure.com
````

## Export as a template

What if you now want to create an additional development environment with the same parameters, or a production environment that matches it?


````
az group export --name "${RESGRP}" > "${RESGRP}".json
````

To create an environment from your template

````
az group deployment create \
    --resource-group "${AZPREFIX}"NewResourceGroup \
    --template-file "${RESGRP}".json
````

###### Reference

* https://docs.microsoft.com/en-us/azure/virtual-machines/linux/create-cli-complete
