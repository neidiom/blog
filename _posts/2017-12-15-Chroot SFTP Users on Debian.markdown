---
layout: post
title:  "Chroot SFTP Users on Debian"
date:   2017-12-15 12:34:07 +0000
categories: jekyll update
---

Check your openssh-server version as it must be over 4.8


# dpkg --list | grep openssh-server

Edit sshd_config file

# nano /etc/ssh/sshd_config

search for Subsystem sftp, comment it out and below add

Subsystem sftp internal-sftp

User Setup

Replace hadzo with your user name

# useradd -m -s /bin/false hadzo

or

# useradd --create-home --shell /bin/false hadzo

set user password

# passwd hadzo

set correct permissions

# chown root:hadzo /home/hadzo/

Edit sshd_config file

# nano /etc/ssh/sshd_config

add at EOF


Match User hadzo
ChrootDirectory /home/hadzo
ForceCommand internal-sftp

restart ssh server


# /etc/init.d/ssh restart
