---
layout: post
title:  "Investigate sockets"
date:   2017-12-15 12:34:07 +0000
categories: jekyll update
---

List source port 80

{% highlight bash %}
ss -t -a -n -s '( sport = :80 )'
{% endhighlight %}

Display All Established SMTP Connections

{% highlight bash %}
ss -o state established '( dport = :smtp or sport = :smtp )'
{% endhighlight %}

Display All Established HTTP Connections

{% highlight bash %}
ss -o state established '( dport = :http or sport = :http )'
{% endhighlight %}

List All The Tcp Sockets and process info with source ip address

{% highlight bash %}
ss -p -o state all '( sport = :http or sport = :https )' src xx.xx.xx.xx
{% endhighlight %}

{% highlight bash %}
lsof -n -u www-data | grep ESTABLISHED
{% endhighlight %}
