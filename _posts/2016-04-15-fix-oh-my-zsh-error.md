---
layout: post
title: "修复compdef: unknown command or service错误"
modified: 2016-04-15 16:29:23 +0800
tags: [zsh,compdef]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

升级完OSX不知道怎么搞得，打开Terminal时会报以下错误：

{% highlight text %}
compdef: unknown command or service: git
compdef: unknown command or service: grep
compdef: unknown command or service: git
{% endhighlight %}

估计是权限方面的问题，后来试了很多方法，解决方法如下：

执行：

{% highlight bash %}
compaudit | sudo xargs chmod g-w
compaudit | sudo xargs chown root
rm ~/.zcompdump*
compinit
{% endhighlight %}

