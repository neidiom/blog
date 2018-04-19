---
layout: post
title: "Azure: Create a custom image of an Azure VM using the CLI and Create a VM from the image"
date:   2018-04-19 07:34:07 +0100
categories: azure image deprovision generalize
---

* TOC
{:toc}


# Deprovision the VM


Deprovisioning generalizes the VM by removing machine-specific information.
During deprovisioning the host name is reset to `localhost.localdomain`.

Specifies setting are deleted, like

* SSH host keys,
* nameserver configurations,
* root password
* cached DHCP leases


## SSH to the VM.

Connect to your VM using SSH and run the command to deprovision the VM.


````
ssh azureuser@FQDN
````

````
sudo waagent -deprovision+user -force
````

You will get these messages

````
WARNING! The waagent service will be stopped.
WARNING! Cached DHCP leases will be deleted.
WARNING! root password will be disabled. You will not be able to login as root.
WARNING! /etc/resolvconf/resolv.conf.d/tail and /etc/resolvconf/resolv.conf.d/original will be deleted.
WARNING! ned account and entire home directory will be deleted.
````

Exit form the VM

````
exit
````


## Deallocate the VM


````
az vm deallocate \\
--resource-group myResourceGroup \\
--name myVM
````


## Mark the VM as generalized


````
az vm generalize \
--resource-group myResourceGroup \
--name myVM
````


# Create the image


````
az image create \
--resource-group myResourceGroup \
--name myImage \
--source myVM
````


# Create VMs from the image



````
az vm create \
--resource-group myResourceGroup \
--name myVMfromImage \
--image myImage \
--admin-username azureuser \
--generate-ssh-keys
````

# Image management


## List

````
az image list \
--resource-group myResourceGroup \
-o table
````


## Delete


````
az image delete \
--name myOldImage \
--resource-group myResourceGroup
````
