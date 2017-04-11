---
layout: post
title: "如何修复东芝z30-b针对macOS 10.12.4系统的屏幕亮度控制问题"
modified: 2017-04-11 15:52:10 +0800
tags: [黑苹果,10.12.4,Z30-b,东芝,屏幕亮度]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

macOS 10.12对于屏幕亮度的控制机制变化很大，10.12.3时还可以通过安装键盘映射软件（以前讲过的Karabiner Elements）来解决，到了10.12.4这个办法也不灵了，新的解决方案如下：

## 清理环境

- 移除已安装的ACPIBacklight.kext 或 IntelBacklight.kext
- 去除ACPI中与PLNF有关的项，例如：Clover中的AddPNLF_1000000，再比如graphics_PNLF.txt补丁等
- 确保已经将显卡设备名改为IGPU

## 准备

下载两个项目并编译guide.git：

{% highlight bash %}
git clone -b beta https://github.com/RehabMan/HP-ProBook-4x30s-DSDT-Patch probook.git
git clone https://github.com/RehabMan/OS-X-Clover-Laptop-Config.git guide.git

cd guide.git
make
{% endhighlight %}

注：为了编译成功，先下载[iasl](https://bitbucket.org/RehabMan/acpica/downloads/iasl.zip)，然后将iasl放入在/usr/bin目录下

我们实际需要三个东西：

- guide.git/build/SSDT-PNLF.aml
- guide.git/config_patches.plist
- probook.git/kexts/AppleBacklightInjector.kext


## 安装

1. 安装AppleBacklightInjector.kext到 "/资源库/Extensions/"
2. 将config_patches.plist中关于AppleBacklight的补丁部分（注释为：change %04x to xxxx for AppleBacklightInjector.kext...）复制到/EFI/EFI/CLOVER/config.plist中相应位置
3. 将SSDT-PNLF.aml复制到/EFI/EFI/CLOVER/ACPI/patched下面，并在config.plist中的SortedOrder项中加入此项，顺序上在SSDT.aml下面

重新启动

## 测试

按F1/F2，应该可以看到小太阳了！

注：我开始忘了移除IntelBacklight.kext，所以，虽然看到了小太阳，但是屏幕亮度不发生变化，移除了就好了。


更详细的内容参见：[[Guide] Laptop backlight control using AppleBacklightInjector.kext](https://www.tonymacx86.com/threads/guide-laptop-backlight-control-using-applebacklightinjector-kext.218222/)
