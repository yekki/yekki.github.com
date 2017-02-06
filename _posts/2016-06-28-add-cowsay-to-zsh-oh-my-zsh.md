---
layout: post
title: "为Oh My ZSH添加卡通欢迎界面"
modified: 2016-06-28 19:56:30 +0800
tags: [ZSH,cowsay]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

## 安装依赖包

{% highlight bash %}
brew install fortune
brew install cowsay
{% endhighlight %}

## 添加插件

编辑~/.zshrc，找到plugins这行，加入chucknorris插件，例如：

{% highlight bash %}
plugins=(chucknorris xx xx)
{% endhighlight %}

在此文件最后一行加入：

{% highlight bash %}
chuck_cow
{% endhighlight %}

保存退出

## 看看效果

重新打开终端，牛来了！

![cowsay](/upload/images/cowsay.jpg)