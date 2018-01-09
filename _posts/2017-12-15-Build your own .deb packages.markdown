---
layout: post
title:  "Build your own .deb packages"
date:   2017-12-15 12:34:07 +0000
categories: jekyll update
---
I wanted to upgrade mydns on etch to mydns-ng and the package was missing.

So I decided to build my own using checkinstall which was missing for Debian etch, so I googled it and fond a link:

{% highlight bash %}
# wget http://debian.nix.hu/debian/dists/etch/checkinstall/binary-i386/checkins...
{% endhighlight %}

installed it
{% highlight bash %}
# dpkg -i checkinstall_1.6.1-1_i386.deb
{% endhighlight %}

downloaded mydns-ng

{% highlight bash %}
# wget http://switch.dl.sourceforge.net/sourceforge/mydns-ng/mydns-1.2.8.26.tar.gz
{% endhighlight %}

{% highlight bash %}
# tar -xzvf mydns-1.2.8.26.tar.gz
{% endhighlight %}

{% highlight bash %}
# cd mydns-1.2.8
{% endhighlight %}

the BUILD options for mydns:

{% highlight bash %}
# ./configure --prefix=/usr --mandir=\$${prefix}/share/man --enable-alias --infodir=\$${prefix}/share/info --disable-static-build --enable-debug
{% endhighlight %}

and then:

{% highlight bash %}
# checkinstall -exclude=/usr/bin,/usr/lib -D make install
{% endhighlight %}

and now the deb package is ready - mydns_1.2.8-1_i386.deb
