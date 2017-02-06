---
layout: post
title: "如何开启原生SSD Trim功能"
modified: 2016-05-07 11:06:47 +0800
tags: [SSD,TRIM]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
{% highlight bash %}
sudo trimforce enable
{% endhighlight %}

因为系统原生工具，此方法无需开启rootless=0，更不会改变已有驱动的签名，也就是说不需要kext-dev-mode=1，白果也可用此方法开启Trim。