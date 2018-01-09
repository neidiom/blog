---
layout: post
title:  "Welcome to Jekyll!"
date:   2016-02-27 12:34:07 +0000
categories: jekyll update
---
We will ad custom puppet fact that will show ruby path.

create file

{% highlight bash %}
/etc/puppet/modules/YOU_MODULE_NAME/lib/facter/rubypath.rb
{% endhighlight %}


add following ruby code

{% highlight bash %}
Facter.add(:rubypath) do
setcode 'which ruby'
end
{% endhighlight %}


run on puppet gent

{% highlight bash %}
facter --puppet rubypath
{% endhighlight %}

Custom facts from modules are only used on puppet runs.
