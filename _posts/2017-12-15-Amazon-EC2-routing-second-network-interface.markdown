---
layout: post
title:  "Amazon EC2 routing second network interface"
date:   2017-12-15 12:34:07 +0000
categories: amazon ec2
---

I had to run a second network interface only for https and this is howto do it:

1. Edit /etc/network/interfaces as root and change the eth1 configuration to look like this:


auto eth1
iface eth1 inet dhcp
post-up ip route add default via `route -n |grep UG |awk '{print $2}'` dev eth1 table 10001
post-up ip rule add from 172.31.4.0/24 lookup 10001

2. Run the following command to prevent DHCP from adding the default gateway for eth1:


sudo su -
cat > /etc/dhcp/dhclient-enter-hooks.d/restrict-default-route case in
eth0)
;;
*)
unset new_routers
;;
esac
EOF
CONF

3. Restart the network configuration by executing the following command: sudo service networking restart

Now you should be able to connect using the second interface public IP.
