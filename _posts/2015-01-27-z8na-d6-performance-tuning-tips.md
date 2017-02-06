---
layout: post
title: "Z8NA-D6 主板调优实践"
modified: 2015-01-27 19:48:07 +0800
tags: [台机,调优,黑苹果]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
昨天升级家里的台机到Yosemite，由于以前使用的是变色龙，所以这次采用了Clover重新安装。安装过程（参考：[How to Install OS X Yosemite using Clover](http://www.tonymacx86.com/yosemite-desktop-guides/144426-how-install-os-x-yosemite-using-clover.html)）没什么好说的，稍微有点别扭的的是我的华硕Z8NA-D6主板不支持UEFI，所以采用BIOS兼容模式安装，有点不爽。本文重点谈下安装完Yosemite后，通过Geekbench跑分软件发现的一些问题及解决办法。

机台配置如下：

- CPU：X5570 x 2
- 内存：KST ECC 1600MHz 16G x 3
- 显卡：GT610 1G
- 硬盘：Samsung 840 12G SSD + 希捷2TB

这台机器配了有段时间了，说起来当初配置有很多不合理之处。问题最大的就是内存，这主板支持ECC与non-ECC内存，所以没必要多花钱用ECC。其次，这款主板前端总线频率支持1066MHz与1333MHz，所以1600MHz根本发挥不出优势。最后，这款主板有六个内存插槽，最经济的做法是用6根8G条子，这样就可以充分享受三通道，而用3根16G虽然内存不少，但是只能到双通道。还有就是SSD偏小，想用来装多系统是不可能了，当然，这不是主要问题。由于我不做视频处理，GT610也就够用了。好在毕竟是服务器主板，还算稳定！

初次安装时，除了必要的显卡注入与驱动注入补丁（Trim与橘色硬盘），没有打任何DSDT补丁，也没有应用Clover DSDT补丁。结果Geekbench3跑分单核只有1000多一点，多核也就9000多！这绝对是不正常，这台机器正常跑分应该在16000以上。经过不停的试，终于找到答案，原来是上电源管理的补丁有关，用Clover Configurator在Acpi中勾选SSDT下面的"Generate P States"与"Generate CState"。重启后再跑分，总分达到了15000多！

![内存跑分](/upload/images/memlow.png)

虽然总分差不多，可以分析下跑分单项又发现了问题，内存跑分极不正常，单核与多核跑分一样，都在1200多分，这显然没有发挥双通道的威力。更吊诡的是，明明是48G内存，可是在系统信息中确显示64G，再细看，居然可以看到7个内存插槽，上面赫然显示有4根16GB的内存条。

Clover中对于内存的设置相对较少，在SMBIOS中可以设置详细的内存插槽及内存信息，但是我的理解是这只是硬件描述信息，应该不起什么作用，后来实验结果也得到了验证，所以，我觉得硬件问题的可能性大。后来尝试着将内存条移位，也就是将3根内存条全部换到其余的内存插槽，结过测试，果然有效果！虽然内存分值还是偏低（毕竟频率上不去，而且还不是三通道），但是相比以前已大大改善！跑分也上到了16500分多，这台机器曾经跑到过17000分以上，看来还是有调优空间。

正确的内存插接方式如图（红框标注）：

![主板](/upload/images/z8na-d6.jpg)

新的内存跑分：

![内存跑分](/upload/images/memhigh.png)

我个人对于台式机研究相对较少，当前这可机器只能勉强算是能用了，但是细节上还有很多问题需要研究，例如：显示器内建、电源管理等，等以后有时间再补上，至少也要自建DSDT/SSDT及补丁。下面附上config.plist文件及用到的驱动，驱动都是大陆货，就不附下载了。

- [config.plist](/upload/download/z8na-d6_config.plist.zip)
- AppleIntelE1000e.kext（以太网卡驱动）
- FakeSMC.kext（不解释）
- GenericUSBXHCI.kext（USB3）
- VoodooPS2Controller.kext（鼠标键盘）
