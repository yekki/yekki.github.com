---
layout: post
title: "如何使用自定义的按键影射功能"
modified: 2016-02-20 15:14:02 +0800
tags: [键盘,PS2,黑苹果,ADB]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

## PS/2 or ACPI

下载 [OS-X-ioio](https://bitbucket.org/RehabMan/os-x-ioio/downloads)，执行：

{% highlight bash %}
ioio -s ApplePS2Keyboard LogScanCodes 1
{% endhighlight %}

按下功能键（例如：F1），用控制台应用查看system.log看看是否有如下日志输出，如果有，说明功能键受PS/2控制，可以继续下去。例如：

{% highlight bash %}
Feb 20 17:32:15 gniu-z30 kernel[0]: ApplePS2Keyboard: sending key 3b=90 down
{% endhighlight %}

按下每个功能键，记录下每个功能键的PS/2扫描码，例如：我的Z30-B上的是：

{% highlight bash %}
F1=3c
F2=3b
F3=3d
F4=3e
F7=41
F8=42
F9=43
F10=44
F11=57
F12=58
{% endhighlight %}

## 查找映射码

查看[ApplePS2ToADBMap.h](/upload/download/ApplePS2ToADBMap.h.zip)文件，注意映射关系：

{% highlight bash %}
/*ADB码   PS/2扫描码  按键
======================== */
......
0x7a,   // 3b  F1
0x78,   // 3c  F2
0x63,   // 3d  F3
0x76,   // 3e  F4
0x60,   // 3f  F5
0x61,   // 40  F6
0x62,   // 41  F7
0x64,   // 42  F8
0x65,   // 43  F9
0x6d,   // 44  F10
......
0x4a,   // e0 20  Mute (hp Fn+F7)
DEADKEY,// e0 21  Calculator
0x34,   // e0 22  Play/Pause (hp Fn+F11)
{% endhighlight %}

注意：有些键是有扩展码的，前面都有个e0。
根据你的需求，将PS扫描码或ADB码映射到按键功能，例如：

{% highlight bash %}
# ADB映射
3b=90;F2_brightness_down
3c=91;F1_brightness_up

# PS/2映射
44=e020;F10_mute
57=e02e;F11_volume_down
58=e030;F12_volume_up
3e=e009;F4_launchpad
3d=e00a;F3_mission_control
42=e022;F8_play_pause
41=e010;F7_previous_track
43=e019;F9_next_track
{% endhighlight %}

注：";"后面是描述，根据具体情况填写

## 修改VoodooPS2Controller.kext

用文本编辑器打开：

VoodooPS2Controller.kext->Contents->Plugins->VoodooPS2Keyboard.kext->Contents->Info.plist

找到IOKitPersonalities->ApplePS2Keyboard->Platform Profile->Default->Custom PS2 Map/Custom ADB Map，根据上面找出的映射关系填写，例如：

{% highlight xml %}
<key>Custom ADB Map</key>
<array>
	<string>3c=91;F1_brightness_up</string>
	<string>3b=90;F2_brightness_down</string>
	<string>;Items must be strings in the form of scanfrom=adbto (in hex)</string>
</array>
<key>Custom PS2 Map</key>
<array>
	<string>43=e019;F9_next_track</string>
	<string>41=e010;F7_previous_track</string>
	<string>42=e022;F8_play_pause</string>
	<string>3d=e00a;F3_mission_control</string>
	<string>3e=e009;F4_launchpad</string>
	<string>58=e030;F12_volume_up</string>
	<string>57=e02e;F11_volume_down</string>
	<string>44=e020;F10_mute</string>
	<string>;Items must be strings in the form of scanfrom=scanto (in hex)</string>
	<string>e027=0;disable discrete fnkeys toggle</string>
	<string>e028=0;disable discrete trackpad toggle</string>
</array>
{% endhighlight %}


## 重新安装VoodooPS2Controller.kext，重启生效
