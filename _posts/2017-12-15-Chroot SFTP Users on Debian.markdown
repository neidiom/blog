---
layout: post
title:  "Chroot SFTP Users on Debian"
date:   2017-12-15 12:34:07 +0000
categories: jekyll update
---

Check your openssh-server version as it must be over 4.8

{% highlight bash %}
# dpkg --list | grep openssh-server
{% endhighlight %}

Edit sshd_config file

{% highlight bash %}
# nano /etc/ssh/sshd_config
{% endhighlight %}

search for Subsystem sftp, comment it out and below add

Subsystem sftp internal-sftp

User Setup

Replace hadzo with your user name

{% highlight bash %}
# useradd -m -s /bin/false hadzo
{% endhighlight %}

or

{% highlight bash %}
# useradd --create-home --shell /bin/false hadzo
{% endhighlight %}

set user password

{% highlight bash %}
# passwd hadzo
{% endhighlight %}

set correct permissions

{% highlight bash %}
# chown root:hadzo /home/hadzo/
{% endhighlight %}

Edit sshd_config file

{% highlight bash %}
# nano /etc/ssh/sshd_config
{% endhighlight %}

add at EOF

{% highlight bash %}
Match User hadzo
ChrootDirectory /home/hadzo
ForceCommand internal-sftp
{% endhighlight %}

restart ssh server

{% highlight bash %}
# /etc/init.d/ssh restart
{% endhighlight %}
