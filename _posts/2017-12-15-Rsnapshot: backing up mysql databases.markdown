---
layout: post
title:  "Rsnapshot: backing up mysql databases"
date:   2017-12-15 12:34:07 +0000
categories: jekyll update
---
Install mysql-client if not installed, this will provide the mysqldump utility

# apt-get install mysql-client


# cp /usr/share/doc/rsnapshot/examples/utils/backup_mysql.sh /usr/local/bin/

Make sure your backup scripts are owned by root, and not writable by anyone else.

# chown root.root /usr/local/bin/backup_mysql.sh
# chmod o-w /usr/local/bin/backup_mysql.sh

This scripts is designed only to back up all databases to a single file.

Edit script

# nano /usr/local/bin/backup_mysql.sh

replace


/usr/bin/mysqldump --all-databases > mysqldump_all_databases.sql

with


/usr/bin/mysqldump --defaults-file=/etc/mysql/debian.cnf --all-databases > mysqldump_all_databases.sql

If you need to dump a single database use line below


/usr/bin/mysqldump --defaults-file=/etc/mysql/debian.cnf DATABASE > DATABASE.SQL

Add script to rsnapshot

Edit rsnapshot config

# nano /etc/rsnapshot.conf

Under BACKUP POINTS / SCRIPTS add following

backup_script /usr/local/bin/backup_mysql.sh localhost/mysqldump

Test config

# rsnapshot configtest
