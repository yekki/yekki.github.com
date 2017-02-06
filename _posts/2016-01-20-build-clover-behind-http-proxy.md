---
layout: post
title: "借助CloverGrowerPro编译Clover如何设置代理"
modified: 2016-01-20 14:04:49 +0800
tags: [编译,Clover,CloverGrowerPro,代理]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
为了得到最新版本的Clover，我使用[CloverGrowerPro](https://github.com/yekki/CloverGrowerPro)从源码编译。由于Clover使用SVN管理源代码，而我们公司网络需要设置代理才能访问，因此，我不得不修改CloverGrowerPro以满足要求。具体步骤如下：

- 修改~/.subversion/servers

{% highlight ini %}
[global]
http-proxy-host=yourproxyhost
http-proxy-port=yourproxyport
....
{% endhighlight %}

- 修改CloverGrowerPro.sh

由于使用HTTP代理，我们不得不把所有 "svn://" 改成 "https://"。这里，我们需要改写的是

将
{% highlight ini %}
CLOVERSVNURL='svn://svn.code.sf.net/p/cloverefiboot/code'
{% endhighlight %}
改为：
{% highlight ini %}
CLOVERSVNURL='https://svn.code.sf.net/p/cloverefiboot/code'
{% endhighlight %}

另外，在函数installToolchain()，installGettext()，installNasm()中也有对于SVN的调用，我们需要做如下修改

将
{% highlight bash %}
svn export --force "$CLOVERSVNURL"/buildnasm.sh "$srcDIR"/buildnasm.sh >/dev/null
{% endhighlight %}

改为：
{% highlight bash %}
svn export --config-option servers:global:http-proxy-host=yourproxyhost --config-option servers:global:http-proxy-port=yourproxyport --force "$CLOVERSVNURL"/buildnasm.sh "$srcDIR"/buildnasm.sh >/dev/null
{% endhighlight %}

当然，为了方便使用，你也可以定义个变量存放代理设置，例如：

{% highlight bash %}
PROXY="--config-option servers:global:http-proxy-host=yourproxyhost --config-option servers:global:http-proxy-port=yourproxyport"

......

svn export $PROXY --force "$CLOVERSVNURL"/buildnasm.sh "$srcDIR"/buildnasm.sh >/dev/null
{% endhighlight %}

另外，在运行 "./CloverGrowerPro.sh -s" 时，记得将svn改成https，例如：

{% highlight bash %}
EDK2 svn url to use [svn://svn.code.sf.net/p/edk2/code/trunk/edk2]:https://svn.code.sf.net/p/edk2/code/trunk/edk2
{% endhighlight %}





