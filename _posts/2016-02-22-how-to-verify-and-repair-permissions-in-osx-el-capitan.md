---
layout: post
title: "OSX El Capitan如何校验与修复磁盘权限"
modified: 2016-02-22 12:34:06 +0800
tags: [El Capitan,磁盘]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

## 修复校验磁盘权限

{% highlight bash %}
sudo /usr/libexec/repair_packages --verify --standard-pkgs /
{% endhighlight %}

这里修复校验的是/目录下，如果要修复其它磁盘，需要指明Volume参数，如下面例子

## 修复磁盘权限

{% highlight bash %}
sudo /usr/libexec/repair_packages --repair --standard-pkgs --volume /
{% endhighlight %}

