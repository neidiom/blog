---
layout: post
title:  "Fix "Cannot set LC_ALL to default locale""
date:   2017-12-15 12:34:07 +0000
categories: jekyll update
---

locale: Cannot set LC_ALL to default locale: No such file or directory

To fix this error do folowing


apt-get install locales
dpkg-reconfigure locales
echo 'export LC_ALL="en_US.UTF-8"' > /etc/profile.d/locale.sh

log out, log back in and test


# echo $LC_ALL
en_US.UTF-8
