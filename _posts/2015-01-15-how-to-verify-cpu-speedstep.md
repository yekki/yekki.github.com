---
layout: post
title: "如何检查CPU的Speedstep是否生效"
modified: 2015-01-15 09:07:09 +0800
tags: [CPU,Speedstep,黑苹果]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
## 为什么要检查CPU Speedstep
很多果友试着自己装Hackintosh，运气好的没几下就装完了，运行着也挺是那么回事儿，可用一段时间发现不对劲了，笔记本用一会儿就烫手，电池消耗巨快。一般遇到这种情况，多半是两个原因：

- 没有关闭独显（就是说你电脑上有两个显卡同时工作，没有禁用不使用的显卡，此问题的解决本文不赘述
- 没有开启CPU Speedstep功能，至于如何开启的话题本文不赘述，这里只谈谈如何判断

## 什么是Speedstep
通俗得讲，Speedstep和汽车档位差不多意思，只不过这里控制的不是转速，而是CPU频率。也就是说，通过使CPU在不同的频率下工作（需要运行的程序多就高频率，没啥应用跑就低频），以实现减少CPU耗电和减少热量的目的，其延伸的好处还有减少风扇的耗电和延长电池寿命等等。因此，不难理解，这项技术最早用于笔记本移动版CPU。扫盲到此结束，如果还有兴趣参与：[SpeedStep](http://zh.wikipedia.org/wiki/SpeedStep)

## 具体步骤
首先要明确一点，检测的方法有很多，例如使用DPCIManager。这里，讲讲哪何用内核扩展的方式实现。

- 准备AppleCPUPowerManagerInfo.kext，[下载](https://www.dropbox.com/s/d2r6fodk92hfc6e/AppleIntelCPUPowerManagementInfo.kext.zip)

- 加载AppleCPUPowerManagerInfo.kext
{% highlight bash %}
sudo chown -R root:wheel ./AppleIntelCPUPowerManagementInfo.kext
sudo chmod -R 755 ./AppleIntelCPUPowerManagementInfo.kext
​sudo kextload ./AppleIntelCPUPowerManagementInfo.kext
{% endhighlight %}
- 打开『控制台』，搜索"AICPUPM"，以我的环境为例，输出如下：
{% highlight bash %}
15/1/15 上午9:28:58.000 kernel[0]: AICPUPMI: CPU P-States [ 8 32 ]
15/1/15 上午9:29:00.000 kernel[0]: AICPUPMI: CPU P-States [ 8 17 32 ]
15/1/15 上午9:29:01.000 kernel[0]: AICPUPMI: CPU P-States [ 8 17 29 32 ]
15/1/15 上午9:29:02.000 kernel[0]: AICPUPMI: CPU P-States [ 8 17 26 29 32 ]
15/1/15 上午9:30:13.000 kernel[0]: AICPUPMI: CPU P-States [ 8 17 26 29 31 32 ]
{% endhighlight %}
以上就说明Speedstep工作正常！

- 卸载AppleIntelCPUPowerManagementInfo.kext
{% highlight bash %}
sudo kextunload AppleIntelCPUPowerManagementInfo.kext
{% endhighlight %}