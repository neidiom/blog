---
layout: post
title:  "Add postgrey support to Postfix using postconf"
date:   2017-12-15 12:34:07 +0000
categories: jekyll update
---

Postgrey is a great for first line defense sa my Postfix keeps user information in MySQL and every time a mail servers makes a delivery attempt connection to MySQL is made. Postgrey will let through only valid mail servers.

First of all get current settings from Postfix.


# postconf | grep smtpd_recipient_restrictions
smtpd_recipient_restrictions = permit_mynetworks, permit_sasl_authenticated, reject_unauth_destination, check_recipient_access mysql:/etc/postfix/mysql-virtual_recipient.cf, reject_rbl_client zen.spamhaus.org, reject_rbl_client b.barracudacentral.org, reject_rbl_client cbl.abuseat.org

Add check_policy_service inet:127.0.0.1:10023 to smtpd_recipient_restrictions using postconf.


postconf -e 'smtpd_recipient_restrictions = permit_mynetworks, permit_sasl_authenticated, reject_unauth_destination, check_policy_service inet:127.0.0.1:10023, check_recipient_access mysql:/etc/postfix/mysql-virtual_recipient.cf, reject_rbl_client zen.spamhaus.org, reject_rbl_client b.barracudacentral.org, reject_rbl_client cbl.abuseat.org'

That's it.
