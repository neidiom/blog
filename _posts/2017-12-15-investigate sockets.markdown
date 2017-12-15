---
layout: post
title:  "Investigate sockets"
date:   2017-12-15 12:34:07 +0000
categories: jekyll update
---

List source port 80


ss -t -a -n -s '( sport = :80 )'

Display All Established SMTP Connections


# ss -o state established '( dport = :smtp or sport = :smtp )'

Display All Established HTTP Connections


ss -o state established '( dport = :http or sport = :http )'

List All The Tcp Sockets and process info with source ip address


ss -p -o state all '( sport = :http or sport = :https )' src xx.xx.xx.xx

lsof -n -u www-data | grep ESTABLISHED
