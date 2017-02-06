---
layout: post
title: "Clover电源管理相关项解释"
modified: 2016-01-12 12:47:01 +0800
tags: [CLOVER,SSDT,黑苹果]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

## Clover中Ivy/Sandy/Haswell机型电源管理检查列表

- SSDT.aml at EFI/Clover/ACPI/patched
- config.plist/KernelAndKextPatches/AsusAICPUPM=true (Haswell以前型号)
- config.plist/KernelAndKextPatches/KernelPm=true (Haswell及以后型号)
- config.plist/KernelAndKextPatches/KernelLapic=true (用于有Local APIC panic问题的机器)
- 根据你的CPU找到合适的系统定义，即：SMBIOS (config.plist/SMBIOS)
- 没有使用老版本AppleACPIPlatform.kext
- 没有使用NullCPUPowerManagement.kext
- 处理器对象在DSDT中的Scope (_SB) or Scope (_PR)中定义in DSDT (一般很少在OEM DSDT找不到定义)
- 校验生成的SSDT注入到DSDT相同的Scope（通常为_PR）

## 关于Drop OEM
- SSDT可以借助Clover的Generate PStates与Generate CStates等选项生成，但是DSDT不可以生成，尽管BIOS提供一个
- 被放置在/EFI/CLOVER/ACPI/patched目录下的SSDTs将被添加到OEM/BIOS提供的SSDTs
- 被放置在/EFI/CLOVER/ACPI/patched目录下的DSDT将替换OEM/BIOS提供的DSDT

### 由于SSDTs是追加模式，因此，可能存在冲突。解决方法有两个：

- 完全忽略OEM/BIOS提供的SSDTs，即：M打开Clover的Acpi/SSDT/Drop OEM
- 删除部分OEM/BIOS提供的SSDT表，即：打开Clover的Acpi/Drop Tables中的项目，例如：删除DMAR，避免在BIOS中关闭VT-D功能


### 何时需要使用Drop OEM？
对于一些老机型，例如：Sandy Bridge或更老的机型，通常需要使用Drop OEM，并且使用Generate PStates与Generate CStates来生成可变频SSDT。
但是对于现在的主流机型，如：Broadwell/Haswell/Ivy，通常无需Drop OEM就可以正常工作。

## 电源管理测试

10.11以后，很多过去可以用的软件不再能用，例如：DCPIManager.app。现在通常使用AppleIntelInfo.kext来测试电源管理是否生效。

下载并编译（至少是Xcode 7.0 Beta）[AppleIntelInfo.kext](https://github.com/Piker-Alpha/AppleIntelInfo)

设置权限与所有者AppleIntelInfo.kext
{% highlight bash %}
sudo chown -R root:wheel AppleIntelInfo.kext
sudo chmod -R 744 AppleIntelInfo.kext
{% endhighlight %}


加载AppleIntelInfo.kext
{% highlight bash %}
sudo kextload/kextutil AppleIntelInfo.kext
{% endhighlight %}

卸载AppleIntelInfo.kext
{% highlight bash %}
sudo kextunload AppleIntelInfo.kext
{% endhighlight %}

查看测试结果
{% highlight bash %}
sudo cat /tmp/AppleIntelInfo.dat
{% endhighlight %}


