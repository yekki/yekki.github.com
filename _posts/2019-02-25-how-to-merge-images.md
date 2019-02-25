---
layout: post
title: "how to merge images"
modified: 2019-02-25 15:44:51 +0800
tags: [图片]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

我想把几张图片拼成一个大图片，实现方式如下：

安装 imagemagick

{% highlight bash %}
brew install imagemagick
{% endhighlight %}

拼接图片，例如：将1.png, 2.png, 3.png并成me.png，命令如下：

{% highlight bash %}
convert -append 1.png 2.png 3.png me.png
{% endhighlight %}

注：-append是将图片坚着拼，如果想横着拼用+append

