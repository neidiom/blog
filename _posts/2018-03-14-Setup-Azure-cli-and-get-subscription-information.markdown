---
layout: post
title:  "Setup Azure-cli and get subscription information"
date:   2018-03-14 12:55:09 +0000
categories: azure
---
## Install azure-cli

This will install azure-cli on MacOsX

````
brew update && brew install azure-cli && brew install jq
````

## Login to access acconts

{% highlight bash %}
az login
{% highlight bash %}

## List all accounts

````
az account list
````

## List all accounts and filter for attribute "name" which will show you subscription name and emails

````
az account show | grep "name"
````

## Switch to subscription

````
az account set -s "YOUR-CUSTOM-SUBSCRIPTION-NAME"
````

## After you subscription is set, show the current subscription

````
az account show
````

## Show defailted information on subscription

````
az account show -s "YOUR-CUSTOM-SUBSCRIPTION-NAME"
````
