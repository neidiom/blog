---
layout: post
title:  "Rsnapshot: Archiving snapshots"
date:   2017-12-15 12:34:07 +0000
categories: jekyll update
---

Archiving snapshots


# cp /usr/share/doc/rsnapshot/examples/utils/rsnaptar /usr/local/bin/

Make sure your backup scripts are owned by root, and not writable by anyone else.

# chown root.root /usr/local/bin/rsnaptar
# chmod o-w /usr/local/bin/rsnaptar

edit script and set directory paths

# nano /usr/local/bin/rsnaptar

set TAR_DIR to path where snapshot will be archived, and SNAPSHOT_DIR to where the daily snapshot is located

TAR_DIR="/home/user/dvd_backup"
SNAPSHOT_DIR="/var/cache/rsnapshot/daily.0"

please note that the hourly cycle needs to complete in order for the daily snapshot to be created.

This script supports gpg encrupting of files, to disable it comment out following fline in /usr/local/bin/rsnaptar


GPG="/usr/bin/gpg"

Scheduling execution via cron


# nano /etc/cron.daily/rsnaptar

add


#!/usr/bin/env bash
/usr/local/bin/rsnaptar nedim@inservio.ba

Notes:
- this howto does not use the GPG option to encrypt files, if this is a necessity than I can update the howto to include file encryption for better security,
- by default script is run standalone, but it could be modified to run as a backup_script via rsnapshot itself,
