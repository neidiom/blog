---
layout: post
title: "Setup Azure-cli and get subscription information"
date:   2018-03-14 12:34:07 +0000
categories: azure azure-cli
---
These are my notes on how to install `azure-cli` and do some basic setup.

## Install azure-cli

This will install azure-cli on MacOsX

````
brew update && brew install azure-cli && brew install jq
````

## Login and authenticate


* Login to access acconts

````
az login
````

* List all accounts

````
az account list
````

* List all accounts and filter for attribute "name" which will show you subscription name and emails

````
az account show | grep "name"
````

## Setup default subscription

* Switch to subscription

````
az account set -s "YOUR-CUSTOM-SUBSCRIPTION-NAME"
````

* After you subscription is set, show the current subscription

````
az account show
````

* Show detailed information on subscription

````
az account show -s "YOUR-CUSTOM-SUBSCRIPTION-NAME"
````
