---
layout: post
title:  "Puppet cheatsheet"
date:   2016-02-27 12:34:07 +0000
categories: puppet cheatsheet
---

02/27/2016


{% highlight bash %}
puppet config print
puppet config print runinterval
/usr/bin/puppet config set runinterval 1200
puppet parser validate init.pp
{% endhighlight %}

Regenerating a Puppet Agent Certificate

{% highlight bash %}
puppet resource service puppet ensure=stopped
rm -rf /var/lib/puppet/ssl/
puppet resource service puppet ensure=running
{% endhighlight %}

Remove client from Master

{% highlight bash %}
puppet cert --revoke puppet-client.server.com
puppet cert --clean puppet-client.server.com
puppet node clean puppet-client.server.com
{% endhighlight %}
