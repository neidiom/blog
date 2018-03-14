---
layout: post
title:  "Setup Azure-cli and get subscription information"
date:   2018-03-14 12:55:09 +0000
categories: azure azure-cli
---

## Install azure-cli

This will install azure-cli on MacOsX

{% highlight bash %}
brew update && brew install azure-cli && brew install jq
{% endhighlight %}

## Login to access acconts

{% highlight bash %}
az login
{% endhighlight %}

## List all accounts

{% highlight bash %}
az account list
{% endhighlight %}

## List all accounts and filter for attribute "name" which will show you subscription name and emails

{% highlight bash %}
az account show | grep "name"
{% endhighlight %}

## Switch to subscription

{% highlight bash %}
az account set -s "YOUR-CUSTOM-SUBSCRIPTION-NAME"
{% endhighlight %}

## After you subscription is set, show the current subscription

{% highlight bash %}
az account show
{% endhighlight %}

## Show defailted information on subscription

{% highlight bash %}
az account show -s "YOUR-CUSTOM-SUBSCRIPTION-NAME"
{% endhighlight %}
