---
layout: post
title: "在Yosemite中为HD3000显卡开启VGA"
modified: 2015-01-13 15:26:52 +0800
tags: [显卡,HD3000,Yosemite,VGA,黑苹果]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

我的x220笔记本没有Mini-DP口，只有一个VGA口，我们知道苹果设备是不支持VGA口的，由于工作中常常需要讲PPT，因此，不能接投影仪对我来说是无法接收的。还好，x220使用的是HD3000核显，而这个型号的核显是可以实现VGA输出的，基本步骤如下：

## 设置SMBIOS
如果是Clover，在Clover Configurator中选择左侧的SMBIOS，右侧就是SMBIOS信息了，其中最重要的是『Product Name』，这项填入"MacBookPro8,1"	，当然，如果想省事儿，也可以向导生成（点击电脑图标右下的小魔法棒）。

## 应用DSDT补丁
为MaciASL添加源 http://raw.github.com/RehabMan/Laptop-DSDT-Patch/master，应用补丁"graphics_HD3K_low.txt"

## 修改AppleIntelSNBGraphicsFB.kext

### 方法一
- 备份/System/Library/Extensions/AppleIntelSNBGraphicsFB.kext

- 用二进制编辑器[HEX Friend](http://ridiculousfish.com/hexfiend/files/HexFiend.zip)打开 /Ccontents/MacOS/AppleIntelSNBGraphicsFB
查找：

>0102 0400 1007 0000 1007 0000 0503 0000 0200 0000 3000 0000 0205 0000 0004 0000 0700 0000 0304 0000 0004 0000 0900 0000 0406 0000 0004 0000 0900 0000

替换为：

>0102 0300 1007 0000 1007 0000 0503 0000 0200 0000 3000 0000 0205 0000 0008 0000 0600 0000 0602 0000 0001 0000 0900 0000 0000 0000 0000 0000 0000 0000


### 方法二
使用Clover Configurator，在『Kernel and Kext Patches』-> 『KextsToPath』中添加：

{% highlight bash %}
Name:AppleIntelSNBGraphicsFB
Find:010204001007000010070000050300000200000030000000020500000004000007000000030400000004000009000000040600000004000009000000
Replace:010203001007000010070000050300000200000030000000020500000008000006000000060200000001000009000000000000000000000000000000
{% endhighlight %}

或者真接修改config.plist，添加：

{% highlight xml %}
<dict>
	<key>Find</key>
	<data>
	AQIEABAHAAAQBwAABQMAAAIAAAAwAAAAAgUAAAAEAAAH
	AAAAAwQAAAAEAAAJAAAABAYAAAAEAAAJAAAA
	</data>
	<key>Name</key>
	<string>AppleIntelSNBGraphicsFB</string>
	<key>Replace</key>
	<data>
	AQIDABAHAAAQBwAABQMAAAIAAAAwAAAAAgUAAAAIAAAG
	AAAABgIAAAABAAAJAAAAAAAAAAAAAAAAAAAA
	</data>
</dict>
{% endhighlight %}

## 基本原理解释

{% highlight bash %}
原始值
0102 0400 1007 0000 1007 0000 //这句话表示你机器的接口数
0503 0000 0200 0000 3000 0000 //笔记本显示器接口
0205 0000 0004 0000 0700 0000 //下面3个都是用来显示的接口(DVI)
0304 0000 0004 0000 0900 0000 
0406 0000 0004 0000 0900 0000

修改后
0102 0300 1007 0000 1007 0000 //3个接口
0503 0000 0200 0000 3000 0000 //LVDS 笔记本内置显示器接口
0205 0000 0008 0000 0600 0000 //HDMI（除了0205，可能值：0304/0406）
0602 0000 0001 0000 0900 0000 //VGA
0000 0000 0000 0000 0000 0000 //NONE
{% endhighlight %}

## 重新安装修改后的AppleIntelSNBGraphicsFB.kext，重启生效！
**小技巧**：有时候通过VGA口接上外显或投影仪时，苹果不自动探测，此时，点击『系统偏好设置』->『显示器』，按住ALT键，你会看到右下的『集合窗口』按钮变为了『探测显示器』，点击就是了！