---
layout: post
title:  "Preventing attackers discovering database dumps"
date:   2014-03-27 12:34:07 +0000
categories: security nginx
---

Browsing my web server logs I saw


{% highlight bash %}
server.domain.com:80 193.106.172.144 - - [24/Mar/2014:22:25:56 +0100] "GET /dump.sql HTTP/1.0" 404 291 "-" "-"
server.domain.com:80 193.106.172.144 - - [24/Mar/2014:22:25:56 +0100] "GET /Dump.sql HTTP/1.0" 404 291 "-" "-"
server.domain.com:80 193.106.172.144 - - [24/Mar/2014:22:25:56 +0100] "GET /backup.sql HTTP/1.0" 404 293 "-" "-"
server.domain.com:80 193.106.172.144 - - [24/Mar/2014:22:25:57 +0100] "GET /_db_.sql HTTP/1.0" 404 291 "-" "-"
server.domain.com:80 193.106.172.144 - - [24/Mar/2014:22:25:57 +0100] "GET /_DB_.sql HTTP/1.0" 404 291 "-" "-"
server.domain.com:80 193.106.172.144 - - [24/Mar/2014:22:25:57 +0100] "GET /sql.txt HTTP/1.0" 404 290 "-" "-"
server.domain.com:80 193.106.172.144 - - [24/Mar/2014:22:25:57 +0100] "GET /database.sql HTTP/1.0" 404 295 "-" "-"
server.domain.com:80 193.106.172.144 - - [24/Mar/2014:22:25:57 +0100] "GET /localhost.sql HTTP/1.0" 404 296 "-" "-"
server.domain.com:80 193.106.172.144 - - [24/Mar/2014:22:25:57 +0100] "GET /sql.sql HTTP/1.0" 404 290 "-" "-"
server.domain.com:80 193.106.172.144 - - [24/Mar/2014:22:25:57 +0100] "GET /data.sql HTTP/1.0" 404 291 "-" "-"
server.domain.com:80 193.106.172.144 - - [24/Mar/2014:22:25:57 +0100] "GET /1.sql HTTP/1.0" 404 288 "-" "-"
{% endhighlight %}

To disable this kind of attack I added this to my nginx server

{% highlight bash %}
# hide protected files
location ~* .(engine|inc|info|install|module|profile|po|sh|.*sql|theme|tpl(.php)?|xtmpl)$|^(code-style.pl|Entries.*|Repository|Root|Tag|Template)$ {
deny all;
}
{% endhighlight %}
