---
layout: post
title:  "Using Nagios-Naemon monitoring plugins in monit"
date:   2015-01-27 12:34:07 +0000
categories: nagios naemon monito
---
This is the way I use external naemon plugin for smtp authentication in monit.

Create file /etc/monit/naemon_smtp_auth.sh with following content.


{% highlight bash %}
#!/bin/bash
/usr/lib/naemon/plugins/check_smtp -H mail.example.com -A LOGIN -U naemon@example.com -P PASSWORD | grep -q "SMTP OK"
exit $?
{% endhighlight %}

Create file /etc/monit/conf.d/mail/naemon_smtp_auth with following content.


{% highlight bash %}
check program naemon_smtp_auth with path /etc/monit/naemon_smtp_auth.sh
with timeout 500 seconds
start program = "/etc/init.d/saslauthd start"
stop program = "/etc/init.d/saslauthd stop"
if status != 0 then restart
if status != 0 then alert
if 5 restarts within 5 cycles then timeout
{% endhighlight %}

Download naemon plugins at https://www.monitoring-plugins.org/ .
