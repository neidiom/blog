---
layout: post
title:  "Nginx Catch-All Host As Front End To Apache For ISPConfig 3 On Debian Lenny"
date:   2017-12-15 12:34:07 +0000
categories: jekyll update
---

Author: Nedim Hadzimahmutovic <nedim [at] inservio [dot] ba>

Version: v1.1

Last Change: May 19, 2010

Introduction
Apache has always been the web server of choice for me. It is a real beast when it comes to resources usage specially in a resource limited environment such as a VPS. I started playing with Nginx a lightweight, high performance web server. My area of interest was running Nginx as a reverse proxy and making it work in a Apache/ISPConfig 3 environment.



The Problem
I am an OpenVZ, apache2-mpm-itk, mod_php user. Apache mpm-itk does not support FastCGI. This problem dramatically increases if you use a 64-bit OS since Apache will now use much more memory (32-bit systems have 4-byte pointers whereas 64-bit systems have 8-byte pointers). I started getting KMEMSIZE limit errors and Apache was the reason why. Apache made my VPS unusable so I had to look for an alternative.



The Solution
Nginx was the answer but I am a ISPConfig user which only supports Apache and if I found a way around this there was no way I would manually manage each virtual host. The solution was to setup Nginx catch all host as front-end and proxy to Apache which will be running in the back-end on a different port. This way Nginx will serve the static files and PHP would be left to Apache. You can also leave a whole domain to Nginx if you like, just put a that domain's virtual host before the default vhost.

One step further would be to run a 32-bit chroot environment on top of the 64-bit OS and install 32-bit Apache but this will not be covered in this tutorial.



Configure Apache
Configure Apache to run on port 82 in /etc/apache2/ports.conf and in all of your virtual hosts. To make it easier use sed command:


# sed -ie 's/YOUR-IP:80/YOUR-IP:82/g' /etc/apache2/sites-available/*.vhost

I assume your virtual host is IP based - your vhost could have *:80 instead of IP:80.

The sed command will make backup files of your .vhost files which will have .vhoste extension. You can move the backup vhost files:

mkdir /root/apache2_vhost_backup/
mv /etc/apache2/sites-available/*.vhoste /root/apache2_vhost_backup/

Restart apache and use netstat check if it is running on port 82:


# /etc/init.d/apache2 restart

# netstat -tunap | grep apache2

tcp 0 0 0.0.0.0:82 0.0.0.0:* LISTEN 7630/apache2

Now you have to change the ISPConfig Apache templates. Copy them to your conf-custom directory:


# cd /usr/local/ispconfig/server/


# cp conf/apache_ispconfig.conf.master conf-custom/


# cp conf/vhost.conf.master conf-custom/

Open the two files and change :80 to :82. Just to be sure run grep command and check if the output matches:


# grep :82 -i /usr/local/ispconfig/server/conf-custom/*


/usr/local/ispconfig/server/conf-custom/apache_ispconfig.conf.master:NameVirtualHost {tmpl_var name="ip_address"}:82

/usr/local/ispconfig/server/conf-custom/vhost.conf.master:
:82>

You will see all requests as originating from localhost (127.0.0.1). To see users real IP address you will have to install libapache2-mod-rpaf:


# apt-get install libapache2-mod-rpaf

Add the following to /etc/apache2/apache2.conf:


# nano /etc/apache2/apache2.conf


RPAFsethostname On
RPAFproxy_ips 127.0.0.1 YOU_IP_ADDRESS



Installing And Configure Nginx
Enable the lenny-backports repository, you will find the instructions on http://backports.org/.


#apt-get install nginx

Remove the default vhost:


# rm /etc/nginx/sites-available/default

Open the file:


# nano /etc/nginx/sites-available/default

Add the following content to the file:


server {
listen 80 default;
server_name _;
server_name_in_redirect off;
resolver 127.0.0.1;
#### www. redirect	- all domains starting with www will be redirected to http://domain. ####
if ($host ~* ^(www\.)(.+)) {
set $rawdomain $2;
rewrite ^/(.*)$ http://$rawdomain/$1 permanent;
}
access_log /var/log/ispconfig/httpd/$host/access.log;
location ~* ^.+.(jpg|jpeg|gif|png|ico|css|zip|tgz|gz|rar|bz2|doc|xls|exe|pdf|ppt|txt|tar|mid|midi|wav|bmp|rtf|js|swf|flv|html|htm|mp3)$ {
root /var/www/$host/web;
access_log off;
expires 30d;
}
location / {
root /var/www/$host/web;
index index.html index.htm index.php;
access_log off;
proxy_pass http://$host:82;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header Host $host;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
}

or do a proxy_pass to your server's IP address


server {
listen 80 default;
server_name _;
server_name_in_redirect off;
resolver 127.0.0.1;
#### www. redirect	- all domains starting with www will be redirected to http://domain. ####
if ($host ~* ^(www\.)(.+)) {
set $rawdomain $2;
rewrite ^/(.*)$ http://$rawdomain/$1 permanent;
}
access_log /var/log/ispconfig/httpd/$host/access.log;
location ~* ^.+.(jpg|jpeg|gif|png|ico|css|zip|tgz|gz|rar|bz2|doc|xls|exe|pdf|ppt|txt|tar|mid|midi|wav|bmp|rtf|js|swf|flv|html|htm|mp3)$ {
root /var/www/$host/web;
access_log off;
expires 30d;
}
location / {
root /var/www/$host/web;
index index.html index.htm index.php;
access_log off;
proxy_pass http://IP-ADDRESS:82;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header Host $host;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
}

Do not forget to replace IP-ADDRESS with your web server's ip address.

That's it. Nginx will serve all your static files like images even html files and php stuff will be forwarded to Apache.
