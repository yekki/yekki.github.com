---
layout: post
title: "如何使Recovery分区正常工作"
modified: 2016-12-02 11:28:27 +0800
tags: [分区,Recovery HD]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

通常安装完系统后，在进入Clover菜单选择Recovery分区后是进不去的，对于我这种完美强迫症患者来说这是不能忍的，最后，终于在网上找到个简单办法让它工作，废话不多说，上命令：

先找到Recovery分区的磁盘号

{% highlight bash %}

diskutil list

/dev/disk0 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *256.1 GB   disk0
   1:                        EFI EFI                     209.7 MB   disk0s1
   2:          Apple_CoreStorage Hackintosh HD           255.2 GB   disk0s2
   3:                 Apple_Boot Recovery HD             650.0 MB   disk0s3

/dev/disk1 (internal, virtual):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:                            Hackintosh HD          +254.8 GB   disk1
                                 Logical Volume on disk0s2
                                 735275EA-AEDA-4B26-9081-D5100CE40CF5
                                 Unencrypted

{% endhighlight %}

显然，是disk0s3（一般是这个）

{% highlight bash %}

sudo mkdir /Volumes/Recovery\ HD

sudo mount -t hfs /dev/disk0s3 /Volumes/Recovery\ HD

cd /Volumes/Recovery\ HD/com.apple.recovery.boot

sudo rm -rf prelinkedkernel

sudo cp /System/Library/PrelinkedKernels/prelinkedkernel /Volumes/Recovery\ HD/com.apple.recovery.boot/

sudo touch prelinkedkernel

{% endhighlight %}


重启，妥了！

注：我测试用的机器是Toshiba Portege z30-b(HD5500)，系统是10.12，进入Recovery分区后虽然显示了图形化系统，但是似乎鼠标有点问题，点击响应有点滞后，用键盘操作更方便些。


