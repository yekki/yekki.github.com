---
layout: post
title: "Lenovo x220更换无线网卡AR9285"
modified: 2015-03-08 23:17:50 +0800
tags: [AR9285,x220,驱动,黑苹果,蓝牙,WIFI]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

最近，我一直用x220做为主力机，经过我的折腾，这个机型基本完美了，只是无线网卡（Intel Centrino Advanced-N 6205）无解。出门还要带个USB WIFI，这实在是让我不爽！逛淘宝时无意中找到一块原装联想专用版AR9285，才19块，果断拿下！AR9285虽然不能做到免驱，但是相对来说，这货在Hackintosh领域也不是新面孔了，我还是有信心搞定它得！

查了些资料，要支持这东东有四种方法：

- 方法一：借助Clover的config.plist打补丁（这当然是首选，既简单，又方便升级）
- 方法二：制作或下载驱动注入包（即：Injector）
- 方法三：直接下载修改过的原装驱动包，即：IO80211Family.kext。（这个是我最不喜欢的方式，直接排除！）
- 方法四：打DSDT补丁（这个我没搞定，还在研究中）

经过实验，方法一死活搞不定（现在正在研究中）。方法二则是我唯一能接受的方案。于是，为节省时间，直接从网上找到了个现成的驱动注入包：AtherosWiFiInjector.kext，安装好后，马上就可以用WIFI了，开始还挺高兴，用一会儿发现问题来了，蓝牙鼠标不正常了，具体表现也挺怪异：鼠标只要一磕碰，哪怕是很轻微地那种，鼠标立即跑到左上角。鼠标多指操作也不灵的，甚至是上下滑屏翻页也不成了（我的鼠标是苹果原装，本来是支持多指操作的）。

后来，又在网上找到了[toledaARPT.kext](https://github.com/toleda/wireless_half-mini/tree/master/airport_kext_enabler)，用这个一切正常了！打开这个其中的Info.plist，发现这货其实原理上和AtherosWiFiInjector.kext是一样的，只是细节上有些不同（具体差异研究中），并且，这个文件中居然还有蓝牙模块的定义，仔细看了看，蓝牙模块定义和x220原生蓝牙模块只是差了一点一点，只需将toledaARPT.kext中的idProduct改为8575（原值8573）即可。接下来就简单了，删除原来的蓝牙驱动注入文件，安装编辑好的toledaARPT.kext，修复权限与重建缓存，最后重启。一切正常了！

另外，我还尝试了使用[FakePCIID](https://bitbucket.org/RehabMan/os-x-fake-pci-id/downloads)，只需要将FakePCIID_AR9280_as_AR946x.kext/Contents/Info.plist中的『pci168c,34』换成『pci168c,2b』即可。记得安装FakePCIID.kext与FakePCIID_AR9280_as_AR946x.kext(当然，如果你觉得别扭，也可以改个名子）。这个方法虽然也能成，但是还要为蓝牙单独安装Injector，并且，分别安装驱动鼠标总是不正常，和AtherosWiFiInjector.kext问题一样，估计是我的蓝牙Injector有问题，有时间再研究具体啥原因。

总之，还是安装改造过的toledaARPT.kext最简单，虽然最后搞定了，但是不明白的地方还是挺多，有时间研究下再写！