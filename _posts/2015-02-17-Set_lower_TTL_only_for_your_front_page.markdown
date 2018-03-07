---
layout: post
title:  "Varnish: Set lower TTL only for your front page"
date:   2015-02-17 12:34:07 +0000
categories: varnish
---
To lower the load on my backend I increased the TTL value for all content except for front page. This is really simple as shown below.


{% highlight bash %}
sub vcl_fetch {
# TTL for front page
if ((req.url == "/")){
set beresp.ttl = 30s;
} else {
set beresp.ttl = 240s;
}
}
{% endhighlight %}
