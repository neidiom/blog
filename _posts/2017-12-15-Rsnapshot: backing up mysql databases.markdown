---
layout: post
title:  "Rsnapshot: backing up mysql databases"
date:   2017-12-15 12:34:07 +0000
categories: jekyll update
---
Install mysql-client if not installed, this will provide the mysqldump utility

{% highlight bash %}
apt-get install mysql-client
{% endhighlight %}

{% highlight bash %}
cp /usr/share/doc/rsnapshot/examples/utils/backup_mysql.sh /usr/local/bin/
{% endhighlight %}

Make sure your backup scripts are owned by root, and not writable by anyone else.

{% highlight bash %}
chown root.root /usr/local/bin/backup_mysql.sh
chmod o-w /usr/local/bin/backup_mysql.sh
{% endhighlight %}

This scripts is designed only to back up all databases to a single file.

Edit script

{% highlight bash %}
nano /usr/local/bin/backup_mysql.sh
{% endhighlight %}

replace


{% highlight bash %}
/usr/bin/mysqldump --all-databases > mysqldump_all_databases.sql
{% endhighlight %}

with

{% highlight bash %}
/usr/bin/mysqldump --defaults-file=/etc/mysql/debian.cnf --all-databases > mysqldump_all_databases.sql
{% endhighlight %}

If you need to dump a single database use line below


{% highlight bash %}
/usr/bin/mysqldump --defaults-file=/etc/mysql/debian.cnf DATABASE > DATABASE.SQL
{% endhighlight %}


Add script to rsnapshot

Edit rsnapshot config

{% highlight bash %}
nano /etc/rsnapshot.conf
{% endhighlight %}


Under BACKUP POINTS / SCRIPTS add following

{% highlight bash %}
backup_script /usr/local/bin/backup_mysql.sh localhost/mysqldump
{% endhighlight %}


Test config

{% highlight bash %}
rsnapshot configtest
{% endhighlight %}
