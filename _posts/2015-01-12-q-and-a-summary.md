---
layout: post
title: "常见问题与解决集锦"
modified: 2015-01-12 20:49:16 +0800
tags: [最佳实践,黑苹果]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

## 如何解决系统安装完后不能登录AppStore与iCloud的问题？ 

- 打开『系统偏好设置』，点击『网络』，将其中所有网络适配接口全部删除，例如：以太网、蓝牙PAN等

- 删除文件 /Library/Preferences/SystemConfiguration/NetworkInterfaces.plist

- 重新启动系统

- 打开『系统偏好设置』，点击『网络』，添加网络适配接口，记得先添加"以太网"

- 通过"ifconfig"命令查看，确保能看到en0生效

## 如何制作Yosemite USB安装盘？

- 通过App Store下载系统安装应用，注：现在OSX系统都可以免费下载

- 准备8GB的U盘，以格式为"Mac OS扩展（日志式)"，分区方案为"主引导记录"（如果你的主板支持UEFI，那么选择"GUID发区表"分区方案也可以），分区名为"Installer"，格式化U盘

- 执行如下命令：

{% highlight bash %}
sudo /Applications/Install\ OS\ X\ Yosemite.app/Contents/Resources/createinstallmedia --volume  /Volumes/Installer --applicationpath /Applications/Install\ OS\ X\ Yosemite.app --nointeraction
{% endhighlight %}

- 安装Clover到U盘，config.plist参考：[OS-X-Clover-Laptop-Config](https://github.com/RehabMan/OS-X-Clover-Laptop-Config)

**注**：安装Clover具体过程与选项本文略，只以我笔记本为例（支持UEFI）:勾选『安装Clover到EFI系统区』、勾选『CloverEFI』、勾选Drivers64EFI选项：CsmVideoDxe-64、DataHubDxe-64、OsxAptionFix2Drv-64

## 如何查找主板对应的SystemProductName，以实现用单U盘引导（Clover）多台电脑？

用Clover Configurator查看Boot.log，找到如下字样信息：

>Clover revision: 2976  running on **20AWS1BL03**
>0:100  0:000  ... with board 20AWS1BL03

上例中**20AWS1BL03**就是SystemProductName，当然，你的信息也许和我的不同。

## 为什么在日志中不停地报"mdworker (warning) import bad path"？

执行如下命令：

{% highlight bash %}
touch /Volumes/EFI/.metadata_never_index
{% endhighlight %}

## 如何解决Yosemite启动界面进度条花屏问题？

在config.plist中加入如下补丁：

{% highlight xml %}
<key>KextsToPatch</key>
<array>
	<dict>
		<key>Comment</key>
		<string>Second Stage Patch 1</string>
		<key>Find</key>
		<data>hcB0XUg=</data>
		<key>Name</key>
		<string>IOGraphicsFamily</string>
		<key>Replace</key>
		<data>McB0W0g=</data>
	</dict>
	<dict>
		<key>Comment</key>
		<string>Second Stage Patch 2</string>
		<key>Find</key>
		<data>QYjE6wM=</data>
		<key>Name</key>
		<string>IOGraphicsFamily</string>
		<key>Replace</key>
		<data>QYjE6yM=</data>
	</dict>
</array>
{% endhighlight %}

## 如何通过功能键控制屏幕亮度

- 确保内建屏幕成功，可以在『系统偏好』->『显示器』中看到亮度控制条，并且可以控制屏幕亮度

- 安装 [OS-X-ACPI-Debug.kext](https://bitbucket.org/RehabMan/os-x-acpi-debug/downloads/RehabMan-Debug-2014-1016.zip)

- 为MaciASL添加源 http://raw.github.com/RehabMan/Laptop-DSDT-Patch/master

- 借助MaciASL为DSDT应用补丁："Add DSDT Debug Methods" and "Instrument EC Queries"

- 重新启动系统，打开『控制台』应用，按亮度调节功能键，在『控制台』中找到功能键对应的码值，例如：_Q15对应F5（减屏幕亮度），_Q14对应F6（加屏幕亮度）

- 借助MaciASL为DSDT应用补丁（根据注释替换相应键值）

{% highlight bash %}
into method label _Q15 replace_content
begin
// Brightness Down\n
	Notify(\_SB.PCI0.LPC.KBD, 0x0205)\n
	Notify(\_SB.PCI0.LPC.KBD, 0x0285)\n
end;
into method label _Q14 replace_content
begin
// Brightness Up\n
	Notify(\_SB.PCI0.LPC.KBD, 0x0206)\n
	Notify(\_SB.PCI0.LPC.KBD, 0x0286)\n
end;
{% endhighlight %}

- 重新启动，大功告成！当然，你也可以用此方法设置其它键值。

