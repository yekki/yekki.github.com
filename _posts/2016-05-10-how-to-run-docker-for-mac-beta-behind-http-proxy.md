---
layout: post
title: "如何在公司内网通过代理使用Docker for Mac Beta"
modified: 2016-05-10 16:43:53 +0800
tags: [Docker,代理,Beta,OSX]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

公司网络需要设置代理服务器才能访问外网，我拿到了Docker for Mac Beta的邀请激活Token，但是在公司内网无法激活，并且，就算是激活了，想从Docker Hub上拖Image也是个麻烦事儿，经过研究，解决方案如下：

## 如何激活Docker for Mac Beta

{% highlight bash %}
export HTTP_PROXY=http(s)://proxy_host:proxy_port
export HTTPS_PROXY=http(s)://proxy_host:proxy_port

./Applications/Docker.app/Contents/MacOS/Docker
{% endhighlight %}

弹出激活窗口，输入Token提交激活。关闭窗口，在LaunchPad上点击Docker图标正常启动即可！


## 如何为Docker Engine设置代理

设置这个主要是为了docker pull，我没有查到相关资料，以下方法纯是自己琢磨的。

打开终端，输入以下命令：

{% highlight bash %}
screen ~/Library/Containers/com.docker.docker/Data/com.docker.driver.amd64-linux/tty
{% endhighlight %}

敲一下回车，登录，用户名root，没有密码，直接回车。编辑/etc/init.d/docker文件，如下添加代理：

{% highlight bash %}
start-stop-daemon --start --quiet \
-e HTTP_PROXY=http(s)://proxy_host:proxy_port \
--background \
--exec ${command} \
--pidfile ${pidfile} \
--stderr "${DOCKER_LOGFILE}" \
--stdout "${DOCKER_LOGFILE}" \
-- daemon --pidfile=${pidfile} ${DOCKER_OPTS}
{% endhighlight %}

重启Docker服务

{% highlight bash %}
/etc/init.d/docker restart
/etc/init.d/docker status
{% endhighlight %}

## 如何添加Docker Hub镜像

由于GFW的捣乱，拉镜像非常慢，甚至根本无法下载，解决方法如下：

### 获取镜像地址

当前镜像服务很多，我自己是用的阿里的Docker镜像服务，自己注册[帐号](http://console.d.aliyun.com/index2.html/?spm=0.0.0.0.MVZTDP#/docker/image/list)，访问[控制台](http://console.d.aliyun.com/index2.html/?spm=5176.775974865.0.0.AVs34v)。点击左边的“加速器”，记住“您的专属加速器地址：”信息。

导出：

{% highlight bash %}
pinata get daemon > myconfig.json
{% endhighlight %}

将此文修改为：

{% highlight yaml %}
{"storage-driver":"aufs","debug":true,"registry-mirrors":["上面的加速器地址”]}
{% endhighlight %}

导入：

{% highlight bash %}
pinata set daemon @myconfig.json
{% endhighlight %}

重新启动Docker服务即可生效！










