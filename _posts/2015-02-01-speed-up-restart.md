---
layout: post
title: "如何加速黑苹果重启速度"
modified: 2015-02-01 14:12:23 +0800
tags: [黑苹果,关机]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
有的黑苹果重启速度特别慢（大约2、3分钟），这问题多半与Clover的『Reset Address』与『Reset Value』有关。我不知道变色龙与番茄啥的是否有对应的参数，这里只针对Clover。

##基础知识

###FACP与FADT
我看过好几台机器的ACPI表，都没有找到FADT表，后来才搞明白，原来FACP与FADT是一回事儿。FACP是签名（Signature），FADT（Fixed ACPI Description Table）是表描述名。一般获取的ACPI表文件名为FACP，编译后就会看到如下信息：

{% highlight xml %}
[000h 0000   4]Signature : "FACP"    [Fixed ACPI Description Table (FADT)]
{% endhighlight %}

### 如何快速获取ACPI表文件
如果你手头没有ACPI表文件，但是又想查看其中的内容，最便捷的方式就是借助[DarwinDumper](https://bitbucket.org/blackosx/darwindumper/downloads)。这个工具能够非常方便地帮助用户获取各种信息，并且可以生成一个基于Web的图形界面帮助用户查找信息。

## 如何修复启动速度
刚才说过，这个问题和『Reset Address』与『Reset Value』参数有关，这两个参数位于Clover Configurator的Acpi部分左下方。如果什么都不填写默认值分别为：0x64与0xFE，即：重启通过PS控制器完成，PS控制器只有黑苹果才有，白苹果则是通过PCI实现，所以，如果你的电脑恰好也是通过PCI控制，那么就会有问题了。针对PCI这两个值应该填写：0x0CF9与0x06。
当然，还有一种更精确找到该值的办法，即：通过FACP表。用DarwinDumper生成ACPI表，找到FACP，搜索：**Reset Register**，下图标红部分即是我们要找的值：
![FACP](/upload/images/facp_reset.png)
