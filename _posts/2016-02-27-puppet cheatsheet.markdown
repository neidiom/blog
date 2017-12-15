---
layout: post
title:  "Welcome to Jekyll!"
date:   2017-12-15 12:34:07 +0000
categories: jekyll update
---

02/27/2016



puppet config print
puppet config print runinterval
/usr/bin/puppet config set runinterval 1200
puppet parser validate init.pp

Regenerating a Puppet Agent Certificate

puppet resource service puppet ensure=stopped
rm -rf /var/lib/puppet/ssl/
puppet resource service puppet ensure=running

Remove client from Master

puppet cert --revoke puppet-client.server.com
puppet cert --clean puppet-client.server.com
puppet node clean puppet-client.server.com
