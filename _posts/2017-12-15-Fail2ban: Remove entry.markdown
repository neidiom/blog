---
layout: post
title:  "Fail2ban: Remove entry"
date:   2017-12-15 12:34:07 +0000
categories: jekyll update
---

If you use fail2ban with route instead of iptables this is how you remove an ip address from banned list.

Get current entries

route -n

This is how you remove an ip address.

fail2ban-client get sasl actionunban
ip route del unreachable 
