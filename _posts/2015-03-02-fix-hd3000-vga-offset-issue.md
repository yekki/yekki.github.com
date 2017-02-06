---
layout: post
title: "修复HD3000 VGA外置显示器偏移问题"
modified: 2015-03-02 09:28:59 +0800
tags: [显示器,HD3000,VGA,黒苹果]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

虽然HD3000可以支持VGA模式(参见：[在Yosemite中为HD3000显卡开启VGA](http://www.yekki.me/enable-hd3000-vga-port-in-yosemite/)，但是在实际使用过程中发现，其对部分外置显示器或投影仪支持不好，尤其是对于Dell的显示器，例如：我的外置Dell显示器显示就不正常，严重的屏幕偏移，一半显示正常，另一外黑屏。我在网上查了些资料，发现很多网友碰到同样的问题，甚至是一些白苹果用户，这些用户多是使用Dell显示器出问题（买显示器还是不要碰Dell），看来这个问题并不是黑苹果特有的。经过研究，终于到到了解决办法，步骤如下：

解决这个问题要借助两个应用软件：

- [DarwinDumper](https://bitbucket.org/blackosx/darwindumper)
- [FixEDID](http://www.insanelymac.com/forum/topic/290130-fixedid-v232-application-to-generate-overrides-automatically-for-apple-displays/)

首先，用DarwinDumper得到[EDID](http://zh.wikipedia.org/wiki/EDID)数据。然后运行FixEDID，在界面中选择你的外置显示器（这里有个小技巧，你如何分辨是外置显示器还是内置显示器呢？办法是看Display Class，外置是AppleDisplay，内置是AppleBacklightDisplay），然后选择刚才生成的EDID数据，例如：edid.bin。下面有两项： IODisplayPrefs check in the display" 与 "Add/Fix Monitor Ranges"全勾上。点击『Make』,此软件将生成一个kext文件DisplayMergeNub.kext与一个名为DisplayVendorID-xxxx（XXXX是根据你的设备信息生成的）文件夹。接下来，安装生成的DisplayMergeNub.kext，将DisplayVendorID-xxxx文件夹中的文件复制到/System/Library/Displays/Overrides/DisplayVendorID-xxxx目录下。最后，重新修复权限与重建缓存。

另外，随[FixEDID](http://www.insanelymac.com/forum/topic/290130-fixedid-v232-application-to-generate-overrides-automatically-for-apple-displays/)一起的还有一个叫作RetinaDisplayMenu的应用软件，这个是在顶栏上加入分辨率（选项比系统提供得更多）选项菜单，这个也挺好用的，推荐使用。