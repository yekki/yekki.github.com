---
layout: post
title: "如何重置管理员密码"
modified: 2016-08-10 18:43:30 +0800
tags: [密码]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
如果记了（当然，这个错误正常人不太会犯）MacOS超级管理员密码，或者想黑别人电脑（这个才是重点），咋整？很简单，如下：

* 关机，启动时按住cmd+s进入命令行模式

* 磁盘检测（可选，捎带手检查下磁盘没坏处）

{% highlight bash %}
/sbin/fsck -y
{% endhighlight %}

* 加载文件系统

{% highlight bash %}
/sbin/mount -uaw
{% endhighlight %}

* 删除安装完成标志文件

{% highlight bash %}
rm /var/db/.AppleSetupDone
{% endhighlight %}

* 重新启动电脑

重新启动电脑后，会进入装机时的欢迎界面（不要怕，啥也不会丢！），只管“下一步”，向导最终会创建一个新的管理员账户（只是用来充当临时管理员账户）。用新管理员进入电脑，打开“系统偏好设置”，点击“用户与群组”，修改老管理员密码（注意：这里不需要输入老密码，这正是我们想要的！），注销当前用户，用老管理员账户登录，再将临时创建的管理员删除即可。

妥了，爱干啥干啥吧！

