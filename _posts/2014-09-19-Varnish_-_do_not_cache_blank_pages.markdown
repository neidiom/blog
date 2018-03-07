---
layout: post
title:  "Varnish - do not cache blank pages"
date:   2014-09-19 12:34:07 +0000
categories: varnish
---
This is a way not to cache blank pages or WSOD (white screen of death pages). WSOD pages are pages that have Content-Length: 0 header and return a HTTP/1.1 200 OK response which is not and error and Varnish will cache it.

Check http headers with curl.


{% highlight bash %}
$ curl hadzimahmutovic.com -I
HTTP/1.1 200 OK
Content-Length: 0
{% endhighlight %}

Now the Varnish part.


{% highlight bash %}
sub vcl_fetch {

if (beresp.http.Content-Length == "0"){
return(restart);
}
return(deliver);
}
{% endhighlight %}
