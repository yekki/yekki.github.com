---
layout: post
title: "OSX中管理多版本JDK"
modified: 2015-02-06 11:49:05 +0800
tags: [JDK,jEnv]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
由于工作中需要在多个版本中切换，手工写环境变量或改配置文件太过麻烦，因此，我特别需要一个类似于rvm（ruby多版本管理工具）的工具。昨天终于找到了这样一个工具：jEnv.

## 安装 jEnv
注：在安装jEnv前确保系统中已安装好[brew](http://brew.sh/index_zh-cn.html)。

{% highlight bash %}
$brew install jenv
$brew install caskroom/cask/brew-cask (这个虽然官网上没说，但是我实验的结果是没有这个，后面添加JDK时会报错)
{% endhighlight %}

测试下：
{% highlight bash %}
$jenv versions
* (set by /Users/gniu/.java-version)
{% endhighlight %}

"*"代表当前所选版本，由于系统还未安装JDK（jEnv不管理JDK安装的事儿），所以没有显示JDK信息。

## 安装 JDK
如果是安装最新版的JDK可以通过以下命令完成：

{% highlight bash %}
$brew cask install java
{% endhighlight %}

当然，你也可以直接下载.pkg包安装。如果你需要JDK6，下载地址：[适用于 OS X 的 Java 2014-001](http://support.apple.com/kb/DL1572)

## 注册 JDK
{% highlight bash %}
$jenv add /System/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home
oracle64-1.6.0.65 added
1.6.0.65 added
1.6 added
$jenv add /Library/Java/JavaVirtualMachines/jdk1.7.0_75.jdk/Contents/Home
oracle64-1.7.0.75 added
1.7.0.75 added
1.7 added
$jenv add /Library/Java/JavaVirtualMachines/jdk1.8.0_31.jdk/Contents/Home
oracle64-1.8.0.31 added
1.8.0.31 added
1.8 added

$jenv versions
* system (set by /Users/gniu/.jenv/version)
  1.6
  1.6.0.65
  1.7
  1.7.0.75
  1.8
  1.8.0.31
  oracle64-1.6.0.65
  oracle64-1.7.0.75
  oracle64-1.8.0.31
{% endhighlight %}

## 切换JDK
jEnv切换JDK可以针对某个目录来设置JDK，例如：
{% highlight bash %}
$jenv local oracle64-1.6.0.65
$jenv version
oracle64-1.6.0.65 (set by /Users/gniu/Blog/yekki.github.com/.java-version)
{% endhighlight %}

从上面可以看出，只要你目录/Users/gniu/Blog/yekki.github.com/中，默认的JDK就是oracle64-1.6.0.65。

当然，你也可以全局设置，例如：
{% highlight bash %}
$jenv global oracle64-1.6.0.65
或
$jenv global 1.6
{% endhighlight %}

但是这里需要注意：基于目录的设置会覆盖全局设置。例如：你已经将某个目录设定为oracle64-1.7.0.75，然后你又全局设定oracle64-1.6.0.65，那么在你处在那个目录中时，oracle64-1.7.0.75生效，而出了这个目录，则oracle64-1.6.0.65生效。如果你想删除对目录的JDK设置，只需删除目录中的.java-version文件。

## 安装老版本JDK
我工作中需要用到老版本的JDK，例如：jdk1.7.0_55.jdk。但是，在OSX上你只能安装jdk1.7系列的最新版，这时，你需要使用工具：Pacifist，用这个工具解开.pkg包，将其中的Contents目录取出，将其放入你手工创建的JDK目录，例如：/Library/Java/JavaVirtualMachines/jdk1.7.0_55。最后，记得更改权限，例如："sudo chmod -R 755 /Library/Java/JavaVirtualMachines/jdk1.7.0_55"。



