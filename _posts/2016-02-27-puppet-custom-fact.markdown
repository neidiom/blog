---
layout: post
title:  "Welcome to Jekyll!" 02/27/2016
date:   2016-27-02 12:34:07 +0000
categories: jekyll update
---
We will ad custom puppet fact that will show ruby path.

create file

/etc/puppet/modules/YOU_MODULE_NAME/lib/facter/rubypath.rb

add following ruby code


Facter.add(:rubypath) do
setcode 'which ruby'
end

run on puppet gent

facter --puppet rubypath

Custom facts from modules are only used on puppet runs.
