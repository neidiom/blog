---
layout: post
title:  "Security tip: change permissions of files to 644 and directories to 755"
date:   2014-01-09 12:34:07 +0000
categories: security permissions
---

For files use:


{% highlight bash %}
user@server:~/web$ find ~ -type f -exec chmod 644 {} \;
{% endhighlight %}

For directories use:

{% highlight bash %}
user@server:~/web$ find ~ -type d -exec chmod 755 {} \;
{% endhighlight %}
