---
layout: post
title:  "Set lower TTL only for your front page"
date:   2017-12-15 12:34:07 +0000
categories: varnish update
---
To lower the load on my backend I increased the TTL value for all content except for front page. This is really simple as shown below.


sub vcl_fetch {
# TTL for front page
if ((req.url == "/")){
set beresp.ttl = 30s;
} else {
set beresp.ttl = 240s;
}
}
