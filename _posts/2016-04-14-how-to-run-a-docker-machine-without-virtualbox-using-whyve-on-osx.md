---
layout: post
title: "如何不借助Virtualbox在OSX平台上基于xhyve运行docker-machine"
modified: 2016-04-14 16:47:50 +0800
tags: [xhyve,Docker]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
相对于Virtualbox，[xhyve](https://github.com/mist64/xhyve)更加轻量级，这也是Docker Toolbox下一个版本的重大更新，虽然我提交了Docker Toolbox Beta的申请，但是，截至本文写作时，还未得到安装介质。

## 安装xhyve与docker

{% highlight bash %}
brew install xhyve docker docker-machine docker-compose docker-machine-driver-xhyve
{% endhighlight %}

注意：如果你在安装前已经安装了Docker Toolbox，安装时会遇到错误提示，所以，执行以上命令前，要么卸载Docker Toolbox，要么根据提示修复错误，我自己实践地是后一种方式。

## 创建docker-machine

{% highlight bash %}
sudo chown root:wheel $(brew --prefix)/opt/docker-machine-driver-xhyve/bin/docker-machine-driver-xhyve
sudo chmod u+s $(brew --prefix)/opt/docker-machine-driver-xhyve/bin/docker-machine-driver-xhyve
docker-machine create default --driver xhyve
{% endhighlight %}

初始化Shell：
{% highlight bash %}
eval $(docker-machine env default)
{% endhighlight %}


## 设置docker-machine代理

由于在公司实验，所以不得不设置代理。执行以下命令进入boot2docker：

{% highlight bash %}
docker-machine ssh default
{% endhighlight %}

编辑profile文件：
{% highlight bash %}
sudo vi /var/lib/boot2docker/profile
{% endhighlight %}

添加以下内容：

{% highlight bash %}
export http_proxy=http://your-proxy-host:your-proxy-port
export https_proxy=http://your-proxy-host:your-proxy-port
{% endhighlight %}

重新启动：

{% highlight bash %}
docker-machine restart default
{% endhighlight %}

## 测试

打开Terminal，执行：

{% highlight bash %}
eval $(docker-machine env default)
{% endhighlight %}

下载容器镜像:

{% highlight bash %}
docker pull ubuntu
{% endhighlight %}

查看容器镜像:
{% highlight bash %}
docker images
{% endhighlight %}

妥了！以后再也不用Virtualbox这个笨家伙了！
