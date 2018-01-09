---
layout: post
title:  "Check for blank White Screen of Death pages with monit"
date:   2017-12-15 12:34:07 +0000
categories: monit update
---

{% highlight bash %}
check host hadzimahmutovic.com_WSOD with address hadzimahmutovic.com
if failed
port 80
protocol http
request "/"
checksum d41d8cd98f00b204e9800998ecf8427e
then exec "/bin/bash -c 'echo all is ok - page is not blank>/dev/null'"
ELSE IF SUCCEEDED
then alert
{% endhighlight %}
