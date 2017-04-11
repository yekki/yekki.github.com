---
layout: post
title: "如何更新所有通过brew cask安装的软件"
modified: 2017-04-11 15:48:05 +0800
tags: [brew cask,脚本]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

我写了个脚本干这事儿，代码如下：

{% highlight bash %}
#!/bin/bash

while read -r line
do
	brew cask reinstall $line
done <<< `brew cask outdated --greedy`
{% endhighlight %}


我试了下效果还不错！