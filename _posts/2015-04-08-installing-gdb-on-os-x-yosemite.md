---
layout: post
title: "Yosemite上为Go语言安装GDB断点调试工具"
modified: 2015-04-08 12:14:59 +0800
tags: [Yosemite,Golang,gdb,证书]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
最近研究golang，而golang的调试功能要借助gdb，这个东东在golang的安装包中是没有的，需要自己搞定。过程如下：

## 安装GDB
最简单的方法就是借助brew，执行以下命令：

{% highlight bash %}
$ brew tap homebrew/dupes
$ brew install gdb
{% endhighlight %}

## 为GDB创建自签名证书

打开应用『钥匙访问』，从菜单上依次点击：『钥匙串访问』->『证书助理』->『创建证书』，

![创建证书](/upload/images/gdb_cert1.png)
注：证书类型选择『代码签名』和『让我覆盖这些默认值』

![更改有效期](/upload/images/gdb_cert2.png)
注：更改完『有效期』后，一直点『继续』直到点到『指定用于该证书的位置』这一页，如下图：

![选择证书存放位置](/upload/images/gdb_cert3.png)

![主板](/upload/images/gdb_cert4.png)
注：在『系统』中找到已创建的证书，右键点击『选择简介』

![主板](/upload/images/gdb_cert5.png)
注：在『信任』中将『代码签名』选择为『始终信任』

## 签名GDB

{% highlight bash %}
$ ps -e|grep taskgated
   78 ??         0:02.23 /usr/libexec/taskgated -s
 6699 ttys000    0:00.00 grep taskgated
$ sudo kill -9 78

$ codesign -s gdb-cert $(which gdb)
{% endhighlight %}

完成！我已在LiteIDE中测试通过！