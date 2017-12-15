---
layout: post
title:  "Testing Saint mode in Varnish"
date:   2017-12-15 12:34:07 +0000
categories: jekyll update
---

Sometimes your website will return a 500 error code with a blank page. In that case use curl to check the http status code.


# curl -I http://domain.com/
HTTP/1.1 500 Internal Server Error

Since we do not want to cache error pages, specially not on the homepage, we will use Saint mode in Varnish.


sub vcl_recv {
set req.grace = 30m;
}
sub vcl_fetch {
if (beresp.status >= 500) {
set beresp.saintmode = 10s;
return(restart);
}
set beresp.grace = 30m;
}

Saint mode will enable you to discard certain page from backend and serve it from cache if the page returns an error.

Testing Saint mode
Now to test Saint mode.

1. Create a 500.php file with content and put it in your webroot.


echo ("Test");
?>

2. Acess the file via url http://domain.com/500.php. This will cache the page.

Simulating 500 Internal Server Error
3. Edit the 500.php file and Simulate 500 error.


http_response_code(500);
?>

4. Acess again the file via url http://domain.com/500.php. Varnish will keep serving you old content and will keep ignoring the 500 error for next 30 minutes since we defined the time period with set beresp.grace = 30m.

Set lower TTL for test page
Since the test page is named 500.php set low TTL only for that page.

This is optional.


sub vcl_fetch {
# TTL for test
if ((req.url ~ "/500.php")){
set beresp.ttl = 10s;
}
}

Important notes
- Saint mode can work only if one backend.
- If only one backend is used, backend probes must be disabled.
- If saint mode does not work for you try, in backend definition, set saintmode threshold to a high number like this .saintmode_threshold = 999999999;.
