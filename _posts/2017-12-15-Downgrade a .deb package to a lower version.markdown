---
layout: post
title:  "Downgrade a .deb package to a lower version"
date:   2017-12-15 12:34:07 +0000
categories: jekyll update
---
I recently upgraded my PHP from 5.2.9 to 5.2.10 PHP using the dotdeb repository. After the upgrade I started getting child died with signal error messages, so I decided to downgrade to PHP 5.2.9 and here is how you do it:

{% highlight bash %}
# apt-cache showpkg php5
{% endhighlight %}

search for line

Provides: 5.2.9-0.dotdeb.2 - 5.2.6.dfsg.1-1+lenny3

then downgrade the package like this:

{% highlight bash %}
# apt-get install php5=5.2.9-0.dotdeb.2
{% endhighlight %}

and the rest of php5 packages:

{% highlight bash %}
# apt-get install libapache2-mod-php5=5.2.9-0.dotdeb.2 php5-cgi=5.2.9-0.dotdeb.2 php5-common=5.2.9-0.dotdeb.2 php5-mysql=5.2.9-0.dotdeb.2 php5-apc=5.2.9-0.dotdeb.2 php5-cli=5.2.9-0.dotdeb.2 php5-curl=5.2.9-0.dotdeb.2 php5-imap=5.2.9-0.dotdeb.2 php5-mcrypt=5.2.9-0.dotdeb.2 php5-gd=5.2.9-0.dotdeb.2 php5-suhosin=5.2.9-0.dotdeb.2
{% endhighlight %}
