---
layout: post
title:  "Migrate ISPConfig 3 mail user passwords to Zimbra"
date:   2017-12-15 12:34:07 +0000
categories: jekyll update
---

Login to mysql server as root user with


# mysql -u root -p

Export all mail users, all domains

mysql> SELECT email,password FROM dbispconfig.mail_user INTO OUTFILE '/tmp/sql.txt' FIELDS TERMINATED BY ' userPassword \'{crypt}' ESCAPED BY '\\' ENCLOSED BY '' LINES STARTING BY 'zmprov ma ' TERMINATED BY '\'\r\n';

Export mail users for specific domain

mysql> SELECT email,password FROM dbispconfig.mail_user WHERE email LIKE '%@domain.com' INTO OUTFILE '/tmp/sql.txt' FIELDS TERMINATED BY ' userPassword \'{crypt}' ESCAPED BY '\\' ENCLOSED BY '' LINES STARTING BY 'zmprov ma ' TERMINATED BY '\'\r\n';

Replace @domain.com with your domain.

This will create commands ready for zimbra in following format


$ zmprov ma user@domain.com userPassword '{crypt}$1$rV85sAyx$NGKPhhzAVJ/n7tKnxI4g4.'

The script will be saved to /tmp/sql.txt
