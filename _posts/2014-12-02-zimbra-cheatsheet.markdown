---
layout: post
title:  "Zimbra cheatsheet"
date:   2014-12-02 12:34:07 +0000
categories: zimbra cheatsheet
---

Relay/Route emails to external smtp server
zmprov ma user@domain.com zimbraMailTransport smtp:mail.example.com:25

Relay/Route all mail destined for a particular domain

zmprov
md example.com zimbraMailCatchAllAddress @example.com
md example.com zimbraMailCatchAllForwardingAddress @example.com
md example.com zimbraMailTransport smtp:other-mta.domain.com

Import user's encrypted password

zmprov ma user@domain.com userPassword '{crypt}$1$rV85sAyx$NGKPhhzAVJ/n7tKnxI4g4.'

Place user account into maintenance mode
This will prevent delivery of new mail messages until we finish with the backup procedure.

zmprov ma user@example.com zimbraAccountStatus maintenance

Get list of domains

zmprov -l gad

Get list of all e-mail accounts for a domain

zmprov -l gaa domain.tld

Get mailbox id

zmprov gmi user@domain.tld
zmprov getMailboxInfo user@domain.tld

Repair meta data for a mailbox
Do a verbose check for a mailbox id. This will generate a report


zmblobchk -m 3 -v start

Delete any items that have a missing blob and delete items without exporting.


zmblobchk -m 3 --missing-blob-delete-item --no-export start

Reindex a user mailbox

su - zimbra
zmprov rim username@domain.tld start
zmprov rim username@domain.tld status

Recalculate the mailbox quota usage and unread messages count

zmprov rmc user@domain.tld

Fix Permissions

/opt/zimbra/libexec/zmfixperms --verbose --extended

Enable http and redirect to https

su - zimbra
zmtlsctl both
zmtlsctl redirect
zmcontrol restart

Check the MySQL database for corruption

/opt/zimbra/libexec/zmdbintegrityreport -m -v

User mailbox backup and restore
backup user mailbox

/opt/zimbra/bin/zmmailbox -z -m user@domain.tld getRestURL "//?fmt=zip" > user@domain.tld.zip

restore user mailobx

/opt/zimbra/bin/zmmailbox -z -m user@domain.tld postRestURL "//?fmt=zip&resolve=reset" user@domain.tld.zip

(Change resolve=skip to resolve=modify or resolve=reset, as required, where:

- resolve=reset will delete all mails and import only ones in backup
- resolve=skip will skip and not replace and therefore make duplicate emails
- resolve=modify will make duplicate

- skip
Restore deleted items
Ignore existing items completely
- modify
Restore deleted items
Update existing items to match backup (unread flags etc.)
- reset
Delete all contents of the account
Restore the backup into the now empty account

defailt explanations

“skip” ignores duplicates of old items, it’s also the default conflict-resolution.
“modify” changes old items.
“reset” will delete the old subfolder (or entire mailbox if /).
“replace” will delete and re-enter them.

“resolve=skip” ignores duplicates of old items, it’s also the default conflict-resolution.
“resolve=modify” changes old items.
“resolve=reset” will delete the old subfolder (or entire mailbox if /).
“resolve=replace” will delete and re-enter them.

Manage MySQL in Zimbra
Activate MySQL backup
setting mysql_backup_retention to 3 will retain three rotating versions of mysqldump, allowing for a restore of all mysql date for the last three days:


zmlocalconfig -e mysql_backup_retention=3

location of mysql my.conf


/opt/zimbra/conf/my.cnf

login to mysql server

source ~/bin/zmshutil ; zmsetvars
mysql -u root --password=$mysql_root_password

Run mysqlcheck

/opt/zimbra/mysql/bin/mysqlcheck --defaults-file=/opt/zimbra/conf/my.cnf -S /opt/zimbra/db/mysql.sock -A -C -s -u root --password=$mysql_root_password
