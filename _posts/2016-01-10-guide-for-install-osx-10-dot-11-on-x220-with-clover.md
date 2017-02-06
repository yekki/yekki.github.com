---
layout: post
title: "Lenovo x220 安装EI Capitan与Clover"
modified: 2016-01-10 15:05:34 +0800
tags: [安装,El Capitan,x220,黑苹果]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

注：本文只记录安装过程中的重点信息，并非Step by Step风格的安装向导。

## BIOS设置

- "VT-d" 关闭，如果不能关闭则在config.plist设置dart=0
- "DEP" (data execution prevention) 打开
- "secure boot " 关闭
- "legacy boot" 选UEFI Only或Both
- "fast boot" 关闭
- "CSM" 可选
- "boot from USB" or "boot from external" 打开

## USB安装盘准备

### 格式化U盘

我使用的是GPT，格式化U盘命令（磁盘号通过"diskutil list"查看）：

{% highlight bash %}
diskutil partitionDisk /dev/disk3 1 GPT HFS+J "install_osx" R
{% endhighlight %}

### 安装Clover

- 选择 "仅安装UEFI开机版本"，"安装Clover到EFI系统区"将自动被选择
- 选择 "Bluemac" 开机主题（我喜欢这个）
- 在Drivers64UEFI中选择"OsxAptioFixDrv-64"

下载[HFSPlus.efi](https://github.com/JrCs/CloverGrowerPro/raw/master/Files/HFSPlus/X64/HFSPlus.efi)，将其放入此U盘的/EFI/CLOVER/drivers64UEFI目录中，删除此目录中的VboxHfs-64.efi

下载[config.plist](https://github.com/yekki/hackintosh-laptop/blob/master/repo/laptop/x220/solution/clover/config.plist)，替换U盘/EFI/CLOVER/目录下的config.plist

### 安装必要的Kexts

- [FakeSMC.kext](https://github.com/RehabMan/OS-X-FakeSMC-kozlek)

- [VoodooPS2Controller.kext](https://github.com/RehabMan/OS-X-Voodoo-PS2-Controller)

- [AppleIntelE1000e.kext](http://www.insanelymac.com/forum/topic/205771-appleintele1000ekext-for-108107106105/)

复制以上kexts到U盘/EFI/CLOVER/kexts/10.11目录中

### 制作安装盘

{% highlight bash %}
sudo "/Applications/Install OS X El Capitan.app/Contents/Resources/createinstallmedia" --volume  /Volumes/install_osx --applicationpath "/Applications/Install OS X El Capitan.app" --nointeraction
{% endhighlight %}

## 安装EI Capitan

安装过程和白苹果一样，此处略！安装完成后安装Clover，过程与选项同上，复制U盘中的config.plist及kexts到电脑硬盘的相应位置

## 安装后系统修复

### DSDT/SSDT修复

对于x220机型，有用的是DSDT、SSDT、SSDT-1三个文件。

SSDT借助[ssdtPRGen.sh](https://github.com/Piker-Alpha/ssdtPRGen.sh)获取，具体细节略。

以下DSDT补丁来源：https://github.com/RehabMan/Laptop-DSDT-Patch

- DSDT相关补丁

graphics_Rename-PCI0_VID,graphics_HD3K_low,battery_Lenovo-x220,usb_6-series,graphics_PNLF_ivy_sandy,system_ADP1,system_Mutex,system_IRQ,system_SMBUS, system_HPET,system_MCHC,system_IMEI,system_PNOT,system_OSYS_win8,audio_HDEF-layout3

- SSDT-1相关补丁

graphics_Rename-PCI0_VID

### 修复WIFI

下载[ProBookAtheros.kext](https://github.com/RehabMan/HP-ProBook-4x30s-DSDT-Patch/tree/master/kexts)，将放入硬盘的/EFI/CLOVER/kexts/10.11目录中。

注：由于x220原装WIFI卡不被黑苹果支持，所以我将其换成了AR9285卡，如果不想换笔记本内置卡，可以买个兼容的外接USB WIFI无线网卡。

### 修复蓝牙

编辑： IOBluetoothFamiliy.kext/Contents/Plugins/BroadcomBluetoothHostControllerUSBTransport.kext/Contents/Info.plist

查找：
{% highlight xml %}
<key>idProduct</key>
    <integer>33292</integer>
<key>idVendor</key>
    <integer>1452</integer> 
{% endhighlight %}

替换为：
{% highlight xml %}
<key>idProduct</key>
    <integer>8575</integer>
<key>idVendor</key>
    <integer>2652</integer>
{% endhighlight %}

修复权限与缓存

### 修复声卡

删除/S/L/E目录下的AppleHDA，替换为[AppleHDA](https://github.com/Mirone/AppleHDA_10.11/blob/master/Laptop's/AppleHDA-272.50-CX20590.zip)

修复权限与缓存

### 修复背光

安装[IntelBacklight.kext](https://bitbucket.org/RehabMan/os-x-intel-backlight/downloads)

放在/Library/Extensions或/EFI/CLOVER/kexts/10.11

### 修复电源管理

安装[ACPIBatteryManager.kext](https://bitbucket.org/RehabMan/os-x-acpi-battery-driver/downloads)

放在/Library/Extensions或/EFI/CLOVER/kexts/10.11


## 其它

### 关闭休眠模式

{% highlight bash %}
sudo pmset -a hibernatemode 0
if [ -e /private/var/vm/sleepimage ]; then sudo rm /private/var/vm/sleepimage;fi
{% endhighlight %}

### 忽略固件升级

{% highlight bash %}
sudo softwareupdate --ignore MacBookProEFIUpdate2.7
{% endhighlight %}

注：本文将持续更新





