---
layout: post
title: "解决启动进度条花屏问题"
modified: 2015-01-30 16:22:54 +0800
tags: [Yosemite,黑苹果,补丁]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
在升级到Yosemite后，很多电脑都会出现启动进度条花屏（仅进度条显示有问题）的问题，当然这不是什么大问题，但是对于我们这种强迫症患者来说，这是不能容忍的，因为这不完美！解决这种问题的办法有两种，一种是直接给驱动打布丁，另一种则是通过Clover补丁注入的方式来解决。作为Clover派的我，当然选择后者。

由于昨天Yosemite刚刚发布10.10.2，干脆就一步到位，以下内容只适合10.10.2（10.10.1不适用），步骤如下：

如果使用Clover Configurator（强烈推荐），则在『Kernel and Kext Patches』下的『KextsToPath』添加：

{% highlight bash %}
Name:IOGraphicsFamily
Find:4188C4EB11
Replace:4188C4EB31
Comment:Bootloader Graphics - Second Stage Patch
{% endhighlight %}

如果是直接修改config.plist，则添加：

{% highlight xml %}
<dict>
	<key>Comment</key>
	<string>Bootloader Graphics - Second Stage Patch</string>
	<key>Find</key>
	<data>QYjE6xE=</data>
	<key>Name</key>
	<string>IOGraphicsFamily</string>
	<key>Replace</key>
	<data>QYjE6zE=</data>
</dict>
{% endhighlight %}

当然，如果你没有使用Clover，那只能通过以下方式打补丁了：

{% highlight bash %}
sudo perl -i.bak -pe 's|\x41\x88\xC4\xEB\x11|\x41\x88\xC4\xEB\x31|sg' /System/Library/Extensions/IOGraphicsFamily.kext/IOGraphicsFamily
{% endhighlight %}
