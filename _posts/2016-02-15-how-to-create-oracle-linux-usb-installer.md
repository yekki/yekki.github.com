---
layout: post
title: "如何在OSX平台上制作Oracle Linux USB安装盘"
modified: 2016-02-15 12:28:16 +0800
tags: [Oracle,USB,安装]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

## 准备Oracle Linux ISO安装介制

- 在Oracle官方网站下载: [eDelivery](https://edelivery.oracle.com)
- 我找到了一个更方便的下载网站：[http://mirrors.dotsrc.org](http://mirrors.dotsrc.org/oracle-linux/OL7/u2/x86_64/)

## 制作安装盘

- 确定U盘的磁盘号

{% highlight bash %}
diskutil list 
{% endhighlight %}

- 退出U盘挂载

通过上面的命令可以确定，我的U盘是/dev/disk1，输入以下命令：

{% highlight bash %}
diskutil unmountDisk /dev/disk1
{% endhighlight %}

- 写入镜像到U盘：

命令制式：dd if=iso_file_name of=usb_device bs=bytes，例如：

{% highlight bash %}
sudo dd if=./full_image.iso of=/dev/sdb bs=512k
{% endhighlight %}

## 注意事项

- 我用的是T440P，原来启动类型为UEFI Only，这个不行，改成Both后可以正常进入安装界面了

- 如果报错：“mboot.c32: Not a com32r image”，说明你的USB安装盘插在了USB3口上，换成USB2口既可