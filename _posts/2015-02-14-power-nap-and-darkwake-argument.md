---
layout: post
title: "Power Nap功能与Darkwake参数"
modified: 2015-02-14 22:29:43 +0800
tags: [PowerNap,darkwake,黑苹果]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
## 关于Power Nap功能
处于睡眠状态时，您的 Mac 能通过 Power Nap 功能执行定期检查新邮件、日历及其他 iCloud 更新之类的操作。接通交流电源后，借助 Power Nap 功能，还可以在 Mac 进入睡眠状态后执行备份到 AirPort Time Capsule 的 Time Machine 备份和下载 OS X 软件更新之类的操作。

关于Power Nap更多的内容参见：[OS X：关于 Power Nap 功能](http://support.apple.com/zh-cn/HT5394)

## 关于Darkwake参数
darkwake是Clover的一个启动见数，其设置与Power Nap功能有着莫大的关系，其值对应的功能如下：

{% highlight bash %}
darkwake=0 -> Power Nap 禁用
darkwake=1 -> Power Nap 开启 (机器完全唤醒：风扇打开，显示器打开。每小时一次)
darkwake=2
darkwake=3
darkwake=4
darkwake=5
darkwake=6
darkwake=7
darkwake=8 -> Power Nap 开启 (机器完全唤醒：有时候显示器打开，有时候不会)
darkwake=9
darkwake=10 -> Power Nap 开启 (机器部分唤醒：风扇，显示器不打开，系统日志记录唤醒时间。 时光机备份在睡眠模式进行，每小时一次)
darkwake=11
{% endhighlight %}

如果想开启Power Nap，darkwake应该取1（我猜这个是默认值）或10，其它值（2,3,4,5,6,7,9,11）对应功能未知。

### 查看Darkwake

{% highlight bash %}
$pmset -g
Active Profiles:
Battery Power		-1
AC Power		-1*
Currently in use:
 standbydelay         10800
 standby              1
 womp                 1
 halfdim              1
 hibernatefile        /var/vm/sleepimage
 darkwakes            1
 networkoversleep     0
 disksleep            0
 sleep                0 (sleep prevented by Thunder, com.apple.serve, com.apple.serve)
 autopoweroffdelay    14400
 hibernatemode        0
 autopoweroff         1
 ttyskeepawake        1
 displaysleep         0
 lidwake              1
{% endhighlight %}

### 更改Darkwake

1. 如果使用Clover，直接在启动参数上设置darkwake=X
2. 借助pmset命令：sudo pmset -a darkwakes X

### 查看Power Nap是否生效
打开『控制台』，搜索"Wake reason"，一般会看到如下信息：

{% highlight bash %}
5：53：56 AM Kernel: Wake reason: RTC (Alarm)
6：54：42 AM Kernel: Wake reason: RTC (Alarm)
7：55：28 AM Kernel: Wake reason: RTC (Alarm)
{% endhighlight %}