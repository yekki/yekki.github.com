---
layout: post
title: "Toshiba Portege Z30-B 安装经验总结"
modified: 2016-02-05 19:30:28 +0800
tags: [Z30-B,安装,东芝,黑苹果]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

公司每两年更新一次电脑，这次，我没有选择Lenovo系列，而是选择了东芝的Z30-B。为啥选择东芝呢？一是Lenovo品质越来越差，我早已对其失望透顶;二是我现在成功黑苹果的都是Lenovo系列，这次想试试东芝系列。经过几天的努力，终于将其成功拿下！虽然整个过程还算顺利，但是也是意外频发，害得我熬了一夜，现在写篇总结，避免后人走弯路。

总得来说，这个机型最难搞得是其显卡HD5500（Broadwell核显）。我使用的方法是AAPL, ig-platform-id注入(参见：[Framebuffer data extracted from AppleIntelBDWGraphicsFramebuffer binary](https://www.firewolf.science/2015/04/framebuffer-data-extracted-from-appleintelbdwgraphicsframebuffer-binary/)）。但是，这里需要注意的是：苹果在AppleIntelBDWGraphicsFramebufferbinary里增加了最小被盗内存（minimum stolen memory），如果你的DVMT预置内存小于66MB，这将会导致五国或异常重启。台式机可以在BIOS自由调节DVMT预置内存。笔记本中BIOS默认DVMT内存为32MB，OEM厂商不提供修改的选项。解决这个问题的方法有两个：

## 方法一：通过Clover Kexts注入，修改DVMT断言验证，具体步骤：

- 设置SMBIOS

{% highlight xml %}

<key>SMBIOS</key>
<dict>
	<key>ProductName-Comment</key>
	<string>Using Haswell MacBookAir6,2 until Clover has support for Broadwell identifiers</string>
	<key>ProductName</key>
	<string>MacBookAir6,2</string>
	<key>Trust</key>
	<true/>
</dict>

{% endhighlight %}

- ig-platform-id注入

{% highlight xml %}

<key>ig-platform-id</key>
<string>0x16260006</string>

{% endhighlight %}

- AppleIntelBDWGraphicsFramebuffer.kext 注入

{% highlight xml %}
<dict>
	<key>Comment</key>
	<string>Disable minStolenSize less or equal fStolenMemorySize assertion, 10.11.beta ( (based on Austere.J patch)</string>
	<key>Disabled</key>
	<false/>
	<key>Find</key>
	<data>
	QTnEdj4=
	</data>
	<key>Name</key>
	<string>AppleIntelBDWGraphicsFramebuffer</string>
	<key>Replace</key>
	<data>
	QTnE6z4=
	</data>
</dict>
{% endhighlight %}


## 方法二：修改BIOS，增加DVMT值

我没有试这种方法，实在是太麻烦，更何况修改BIOS是件很危险的事情，搞不好就变砖了。当然，如果有实力，这种方法也是最解决问题的！具体步骤参见：

[[GUIDE] Intel HD Graphics 5500 on OS X Yosemite 10.10.3](http://www.tonymacx86.com/yosemite-laptop-support/162062-guide-intel-hd-graphics-5500-os-x-yosemite-10-10-3-a.html)

## 注意事项

在整个过程中，我还是犯了以下错误，以后要尽量避免：

- config.plist 中忘了添加 SortedOrder 相关条目
- 如果包含了所有SSDTs，不要打system_PNOT补丁
- 只有在USB3工作不正常时才需要加入GenericUSBXHCI.kext
- 打补丁misc_Lid_PRW，避免“睡眠快醒”现象
- 外接WIFI会影响电脑睡眠

## 处理突发问题

本来安装好后基本完美了，然后想调试下ALPS多点触控功能，于是，把相关Kext放在了/Library/Extensions目录下了，然后就再也进不去了（中间过程中，我用了KCPM Utility Pro 3.3修复权限，此软件用了很长时间，我中途手工中止又重试，所以，我也搞不懂到底是哪个步骤出了问题）。我甚至想用USB启动重装系统，但是还是无法进入系统。后来，我试着在Clover启动界面中手工将ig-platform-id值改为0x16260001，这下倒是进去了，但是显存只有4M了，基本无法使用。我试着重装了一下，然后将系统升级到10.11.3，然后重新修复权限，将ig-platform-id重新改回0x16260006，然后就正常了，显存为1536M。问题虽然解决了，但是有点莫名其妙！

## 还不完善的地方
相对来讲，此型号笔记本不太适合黑苹果，有些问题是无法绕过的，问题如下：

- 内置Intel 7265NGW（NGFF接口，似乎苹果产品从未使用过此接口，所以找不到合适的卡替换）无解
- ALPS触摸板多指触摸功能还未调试成功
- SD读卡器无解（这个基本不怎么用，所以也没认真研究）



