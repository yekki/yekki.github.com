---
layout: post
title: "如何查看与清除NVRAM内容"
modified: 2016-01-19 19:22:59 +0800
tags: [NVRAM]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

## 查看NVRAM内容

{% highlight bash %}
nvram -xp
{% endhighlight %}

参数-p 是查看，-x 是以XML格式查看

## 删除NVRAM变量

{% highlight bash %}
nvram -d (variable key name goes here)
{% endhighlight %}

例如：
{% highlight bash %}
nvram -d SystemAudioVolume
{% endhighlight %}

## 清除所有NVRAM变量

{% highlight bash %}
nvram -c
{% endhighlight %}


重置NVRAM变量还有一个比较麻烦的办法是：重启电脑，并同时按下： Command+Option+P+R，听到两声电脑重启的声音后松开按键。

