---
layout: post
title: "如何在MacOS系统中打开Ubuntu图形应用"
modified: 2016-07-11 18:58:12 +0800
tags: [XWindows,Ubuntu,ssh]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
我用两台电脑做实验，一台是MacOS系统，一台是Ubuntu系统，为了避免在不同的机器上切换，我尝试借助XWindows在MacOS系统中直接打开Ubuntu原生应用。步骤如下：

## 在MacOS上安装XQuartz

下载地址：[https://www.xquartz.org/](https://www.xquartz.org/)

安装过程略，反正就是不停地下一步，没什么花头。

注：我试验中使用的是XQuartz-2.7.9.dmg

## 配置ssh

打开~/.ssh/config文件，添加如下内容

{% highlight text %}
Host gniu-docker
  ForwardX11Trusted yes
  ForwardX11 yes
{% endhighlight %}

注：这里的gniu-docker是安装了Ubuntu系统机器的主机名。

## 实验

{% highlight bash %}
ssh -X gniu-docker
firefox
{% endhighlight %}

这时，Ubuntu中的firefox应用就在MacOS中打开了。在第一次打开时，会报如下错误：

{% highlight text %}
libGL error: No matching fbConfigs or visuals found
libGL error: failed to load driver: swrast
{% endhighlight %}

但是第二次打开时就不报错了，具体原因不研究了，反正能正常工作了。




