---
layout: post
title: "东芝 Portege z30-b 安装 El Capitan 技巧"
modified: 2016-11-15 16:37:24 +0800
tags: [黑苹果,Clover,El Capitan,Z30-b,安装,HD5500]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

我这款笔记本比较小众（HD5500, Broadwell），安装黑苹果来比较麻烦。上次安装El Capitan时，害得哥一晚上没睡觉，说实话，虽然安装成功了，但是对于当时的一些细节，确实是记不得了。最近想重装下系统，昨天又害得哥用了一个晚上搞定它，这次记录下关键点。

首先，遇到的问题是电脑可以正常使用，但是用相同的EFI放到USB安装盘中启动后便会黑屏。从报错信息看是显卡驱动问题。我的ig-platform-id用的是0x16260006。后来才搞明白，安装时应该在Clover启动界面将其改为0x16160004，这样就可以正常进入安装界面，当然，如果你安装完全依然用0x16160004，你会发现，虽然系统可以正常进入，但是显存只有4M。

其次，是Clover的版本，开始时用的是3899版，虽然改了ig-platform-id，启动过程也不Crash了，但是进度条会挂起，也就是进度条停在一个位置死活不往下走了。后来将Clover换成3526版，问题解决！

最后，我也尝试了下安装Sierra，我发现Clover 3526不支持Sierra（进入安装界面时会自动重启），但是用3899又进不了安装界面。这就难办了，只能放弃。

对于HD5500，最讨厌的是"DVMT pre-allocated memory"。我死活找不到这款笔记本的BIOS工具，因此，也就无法得到BIOS镜像修改，只能借助Kexts注入，估计这也是安装时不得不用不同ig-platform-id的原因。

参考资料：[GUIDE: Intel HD Graphics 5500 on OS X Yosemite 10.10.3](https://www.tonymacx86.com/threads/guide-intel-hd-graphics-5500-on-os-x-yosemite-10-10-3.162062/)

注：Clover 3526在官网上已经找不到了，这个可以在我的github上找到：[Clover_v2.3k_r3526.zip](https://github.com/yekki/laptop-hackintosh-workbench/raw/master/clover/archives/Clover_v2.3k_r3526.zip)