---
layout: post
title:  "Get list of mysql databases in same line"
date:   2017-12-15 12:34:07 +0000
categories: jekyll update
---
This will skip mysql,performance_schema, information_schema databases.

{% highlight bash %}
mysql> SELECT GROUP_CONCAT(schema_name SEPARATOR ' ') \
FROM information_schema.schemata WHERE schema_name \
NOT IN ('mysql','performance_schema','information_schema');
{% endhighlight %}
