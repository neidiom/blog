---
layout: post
title:  "Bandwidth throttling when creating archives with tar"
date:   2018-03-07 12:34:07 +0000
categories: pv cstream bandwidth throttling tar
---


If you need to create a large archive and want to spare your disk big IO operations then use the `pv` or `cstream` utility.


## PV

Install the `pv` utility.

{% highlight bash %}
apt-get install pv
{% endhighlight %}

Create the archive with tar and pipe it to `pv` which will do the bandwidth throttling.

{% highlight bash %}
tar -cpzf - web/ | pv -L 1m > ./web.tar.gz
{% endhighlight %}

## CSTREAM

Alternative way is to use cstream utility.

{% highlight bash %}
apt-get install cstream
{% endhighlight %}

Run command with bandwidth limit of 777k

{% highlight bash %}
tar cj file.txt | cstream -t 777k | tar xj -C /tmp/
{% endhighlight %}
