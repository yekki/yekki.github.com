---
layout: post
title: "在macOS Sierra中如何开启安装未认证应用特性"
modified: 2016-09-27 12:07:45 +0800
tags: [Sierra,安全]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
在macOS Sierra以前，要想安装第三方未认证应用必须开启：设置->安全性与隐私->任何来源。但是macOS Sierra默认去掉了此选项，样子是这样的：


![anywhere_before](/upload/images/anywhere_before.png)

要是想重新开启这个功能，步骤如下：

{% highlight bash %}
sudo spctl --master-disable
{% endhighlight %}

重新打开，样子是这个样子的：

![anywhere_after](/upload/images/anywhere_after.png)


关闭这个功能恢复初厂设置：
{% highlight bash %}
sudo spctl --master-enable
{% endhighlight %}

现在又可以愉快地安装盗版应用了！：）


