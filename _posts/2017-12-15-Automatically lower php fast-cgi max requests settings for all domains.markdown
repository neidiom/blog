---
layout: post
title:  "Automatically lower php fast-cgi max requests settings for all domains"
date:   2017-12-15 12:34:07 +0000
categories: jekyll update
---

# grep -rl PHP_FCGI_MAX_REQUESTS /var/www/php-fcgi-scripts -R | xargs sed -i 's/PHP_FCGI_MAX_REQUESTS=5000/PHP_FCGI_MAX_REQUESTS=500/g'
