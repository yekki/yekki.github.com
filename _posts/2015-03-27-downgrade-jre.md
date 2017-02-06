---
layout: post
title: "如何降级JRE版本"
modified: 2015-03-27 13:44:10 +0800
tags: [Java,JRE,JDK]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
我们公司的某软件的管理界面用到Java Applet，并且还要求JRE版本不能高于1.6，当前最新的JRE版本是1.8.0.40，而OSX是不支持安装低版本JRE的，此问题的解决办法如下：

## 安装JDK6
下载并安装：[Java for OS X 2014-001](http://supportdownload.apple.com/download.info.apple.com/Apple_Support_Area/Apple_Software_Updates/Mac_OS_X/downloads/031-03190.20140529.Pp3r4/JavaForOSX2014-001.dmg)

打开『终端』，执行如下命令：

{% highlight bash %}
sudo mkdir -p /Library/Internet\ Plug-Ins/disabled 
sudo mv /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin /Library/Internet\ Plug-Ins/disabled
sudo ln -sf /System/Library/Java/Support/Deploy.bundle/Contents/Resources/JavaPlugin2_NPAPI.plugin /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin
sudo ln -sf /System/Library/Frameworks/JavaVM.framework/Commands/javaws /usr/bin/javaws
{% endhighlight %}

## 测试
重新启动浏览器，访问：[Verify Java Version](https://www.java.com/en/download/installed.jsp)

## 恢复
如果需要恢复到原来的JRE版本，重新安装下JRE就好了。
