---
layout: post
title:  "Deploying-with-Capistrano-3"
date:   2017-06-06 12:34:07 +0000
categories: capistano deploy
---

{% highlight bash %}
gem install capistrano
mkdir ~/project
cd ~/project
cap install
{% endhighlight %}

edit

{% highlight bash %}
config/deploy.rb
{% endhighlight %}


Add stage info to file

For production - config/deploy/production.rb
For development - config/deploy/develop.rb


{% highlight bash %}
server 'hostname', user: 'username', roles: %w{web app}, port: 22
{% endhighlight %}

To deploy, must be in root of project

{% highlight bash %}
cd ~/project

cap develop deploy
{% endhighlight %}

Notes

- key based ssh logins need to be setup
- deploy key needs to be setup for git
