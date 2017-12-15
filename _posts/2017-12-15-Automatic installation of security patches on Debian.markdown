---
layout: post
title:  "Automatic installation of security patches on Debian"
date:   2017-12-15 12:34:07 +0000
categories: jekyll update
---

If you plan to automatically upgrade your system you should only use the stable distribution with the security debian repository. For more info on Debian security read http://www.debian.org/security/

To easily get the latest security updates use repository below


deb http://security.debian.org/ squeeze/updates main contrib non-free

Add the security repository to cron-apt

Open con-apt config file

# nano /etc/cron-apt/config

search for line and uncomment the line starting with OPTIONS

# You can for example add an additional sources.list file here.
OPTIONS="-o quiet=1 -o Dir::Etc::SourceList=/etc/apt/security.sources.list"

add the security deb http://security.debian.org/ squeeze/updates main contrib non-free repository to security.sources.list


# nano /etc/apt/security.sources.list

Configuribng cron-apt

The best approach to automatically upgrading is to use a dedicated program like cron-apt

Install the app


# apt-get install cron-apt

Specify when to run, by default it runs every night at 4 o'clock.


# nano /etc/cron.d/cron-apt

change the MAILON option /etc/cron-apt/config which will email the results of the nightly run to specified email address


# nano /etc/cron-apt/config

set to always to get notified on every change

MAILON="always"

set your email address

MAILTO="admin@debian.org"

Configure the upgrade action

edit action config file

# nano /etc/cron-apt/action.d/3-download

replace the contents with


autoclean -y
upgrade --assume-yes -o APT::Get::Show-Upgraded=true

if you only want to download packages and not install them then put below in action file


autoclean -y
upgrade --download-only --assume-yes -o APT::Get::Show-Upgraded=true
