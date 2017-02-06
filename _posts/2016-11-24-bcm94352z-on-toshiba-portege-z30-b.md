---
layout: post
title: "在Toshiba Portege z30-b上安装BCM94352z"
modified: 2016-11-24 12:21:23 +0800
tags: [z30-b,蓝牙,无线,BCM94352z]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
蓝牙与WIFI二合一的NGFF卡没啥好选得，用得最多的就是BCM 94352z(DW1560)。但是我在东芝Portege z30-b上安装这块卡时还是遇到了不少麻烦，安装过程如下：


## 下载驱动

下载：[RehabMan-FakePCIID](https://bitbucket.org/RehabMan/os-x-fake-pci-id/downloads)

下载：[RehabMan-BrcmPatchRAM](https://bitbucket.org/RehabMan/os-x-brcmpatchram/downloads)

## 安装驱动

解压缩以上文件，将BrcmFirmwareRepo.kext与BrcmPatchRAM2.kext安装到//Library/Extensions（最好用工作软件安装，例如：KCPM Utility Pro）。
将FakePCIID_Broadcom_WiFi.kext与FakePCIID.kext放入/EFI/Clover/kexts/10.11(10.12)/中。


### 配置 Clover

用Clover Configurator打开config.plist，做如下修改：

1. Devices->Fake ID->WIFI中填入0x43a014E4 (后来我试了下，似乎不填这个也没问题，并且好象去掉这个后，iPhone HotSpot功能好了)
2. SystemParameters->InjectKexts先择Yes或Detect
3. 加入Kexts注入：

{% highlight xml %}
<key>KextsToPatch</key>
<array>
	<dict>
		<key>Comment</key>
		<string>10.11-BCM94352-CC=#a-Ramalama</string>
		<key>Disabled</key>
		<false/>
		<key>Find</key>
		<data>
		QYP8/3QsSA==
		</data>
		<key>Name</key>
		<string>AirPortBrcm4360</string>
		<key>Replace</key>
		<data>
		ZscGI2HrKw==
		</data>
	</dict>
	<dict>
		<key>Comment</key>
		<string>10.11-BCM94352-Airport-Extreme</string>
		<key>Disabled</key>
		<false/>
		<key>Find</key>
		<data>
		axAAAHUN
		</data>
		<key>Name</key>
		<string>AirPortBrcm4360</string>
		<key>Replace</key>
		<data>
		axAAAJCQ
		</data>
	</dict>	
	<dict>
		<key>Comment</key>
		<string>10.11-BT4LE-Handoff-Hotspot-lisai9093</string>
		<key>Disabled</key>
		<false/>
		<key>Find</key>
		<data>
		SIX/dEdIiwc=
		</data>
		<key>Name</key>
		<string>IOBluetoothFamily</string>
		<key>Replace</key>
		<data>
		Qb4PAAAA60Q=
		</data>
	</dict>
	<dict>
		<key>Comment</key>
		<string>10.11-BCM94352-5GHz-US-FCC-dv</string>
		<key>Disabled</key>
		<false/>
		<key>Find</key>
		<data>
		QYP8/3QsSA==
		</data>
		<key>Name</key>
		<string>AirPortBrcm4360</string>
		<key>Replace</key>
		<data>
		ZscGVVPrKw==
		</data>
	</dict>
	<dict>
		<key>Comment</key>
		<string>10.11-BCM94352-Whitelest-0x4331-iMac14,3</string>
		<key>Disabled</key>
		<false/>
		<key>Find</key>
		<data>
		TWFjLUM2RUZBNjM5NjJGQzZFQTA=
		</data>
		<key>Name</key>
		<string>AirPortBrcm4360</string>
		<key>Replace</key>
		<data>
		TWFjLTI3QURCQjdCNENFRThFNjE=
		</data>
	</dict>
	<dict>
		<key>Comment</key>
		<string>10.11-BCM94352-Whitelest-0x4353-MacBoolAir5,2</string>
		<key>Disabled</key>
		<false/>
		<key>Find</key>
		<data>
		TWFjLUM2RUZBNjM5NjJGQzZFQTA=
		</data>
		<key>Name</key>
		<string>AirPortBrcm4360</string>
		<key>Replace</key>
		<data>
		TWFjLTI3QURCQjdCNENFRThFNjE=
		</data>
	</dict>
</array>
{% endhighlight %}

## 关于无法找到蓝牙模块的问题

安装好配置好后启动系统，就是死活找不到蓝牙模块，按说这卡的蓝牙是走USB的，就算驱动不了这个蓝牙模块，也是可以在System Reports的USB项中看到这个设备，后来，为了验证是否是卡的问题，我甚至重装了Windows，但是在Windows中也没有找到蓝牙模块。联系了卖家，卖家发誓说卡是新的，质量肯定没问题，我看这卡似乎质量不错，看来，这不是软件问题，而是硬件问题。后来，查阅了无数资料，终于找到了解决方案，说起来也很简单，就是将卡上的两个脚屏蔽掉，如下图：

![94352z Shield Two Legs](/upload/images/94352z_shield.jpg)


## 功能测试

测试通过的功能：

1. Handoff
2. 5Ghz
3. AirDrop
4. iPhone HotSpot

