---
layout: post
title: "如何在OSX上安装Oracle Database客户端"
modified: 2016-06-01 12:55:29 +0800
tags: [数据库,Oracle]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

## 下载

[Instant Client Downloads 
for Mac OS X (Intel x86)](http://www.oracle.com/technetwork/topics/intel-macsoft-096467.html)

实验时下载的是（我主要想使用SQL*Plus）：

- Instant Client Package - Basic
- Instant Client Package - SQL*Plus

## 安装

将以上下载的包解压缩到某个目录，比如：我是解压到 ~/Workbench/instantclient_12_1

执行如下命令创建链接：


{% highlight bash %}
cd ~/instantclient_12_1
ln -s libclntsh.dylib.12.1 libclntsh.dylib
ln -s libocci.dylib.12.1 libocci.dylib
{% endhighlight %}

## 测试

{% highlight bash %}
export PATH=~/instantclient_12_1:$PATH
{% endhighlight %}


我使用Docker运行数据库引擎，所以，要先查看端口信息：

{% highlight bash %}
docker port orcl 1521

0.0.0.0:32770
{% endhighlight %}

可以看到，容器中1521端口被映射到本地的32770端口，因此，连接串应该是：


sqlplus system/manager@localhost:32770/orcl

## 其它

执行sqlplus时报如下错误：

{% highlight bash %}
ERROR:
ORA-21561: OID generation failed
{% endhighlight %}

解决方案如下：

执行hostname查看机器名，然后ping一下看是否能ping通，如果不行，手工在/etc/hosts中加入hostname对应项。





