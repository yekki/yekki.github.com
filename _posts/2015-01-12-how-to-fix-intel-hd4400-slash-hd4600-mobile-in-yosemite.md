---
layout: post
title: "Yosemite中如果修复Intel HD4400/HD4600 Mobile显卡驱动"
modified: 2015-01-12 11:26:30 +0800
tags: [显卡,黑苹果,HD4600,HD4400,Yosemite]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
Yosemite自带驱动不支持HD4400/HD4600 Mobile，但是支持HD4400/HD4600 Desktop。在Yosemite刚发布时，黑客们就找到了显卡注入加驱动补丁的方式让Yosemite识别HD4400/HD4600 Mobile，具体步骤这里就不赘述了。但是，这种方式是有副作用的，虽然没有人能够说清楚，但是,在我的环境中有两个明显的问题：

- 蓝牙鼠标系统偏好设置界面崩溃
- 部分和视频相关的应用程序崩溃，例如：搜狐影音。

好消息是，现有已经有其它方案可以解决此问题了，新方案无需对二进制驱动文件打补丁，其好处显而易见，一是系统更加稳定（以上我遇到的问题已解决），二是操作系统升级时无需重新给驱动打补丁。

本方案其原理是在设备与驱动中间层拦截替换PCIID，从而达到骗过系统的目的。如果对其原理有兴趣，不妨阅读项目源码：[OS-X-Fake-PCI-ID](https://github.com/RehabMan/OS-X-Fake-PCI-ID)

言归正传，具体步骤如下：

## 显卡注入

**Clover**
{% highlight xml %}
<key>Devices</key>
<dict>
 <key>FakeID</key>
 <dict>
   <key>IntelGFX</key>
   <string>0x04128086</string>
...
<key>Graphics</key>
<dict>
  <key>Inject</key>
  <dict>
   <key>Intel</key>
   <true/>
  </dict>
  <key>ig-platform-id</key>
  <string>0x0a260006</string>
{% endhighlight %}

 我个人推荐使用Clover注入方式，当然，如果你没有使用Clover或不喜欢Clover注入方式，也可以选择DSDT/SSDT补丁方式实现，补丁如下：
{% highlight bash %}
into method label _DSM parent_adr 0x00020000 remove_entry;
into device name_adr 0x00020000 insert
begin
Method (_DSM, 4, NotSerialized)n
{n
    If (LEqual (Arg2, Zero)) { Return (Buffer() { 0x03 } ) }n
    Return (Package()n
    {n
        "device-id", Buffer() { 0x12, 0x04, 0x00, 0x00 },n
        "AAPL,ig-platform-id", Buffer() { 0x06, 0x00, 0x26, 0x0a },n
        "hda-gfx", Buffer() { "onboard-1" },n
        "model", Buffer() { "Intel HD 4600" },n
    })n
}n
end;
{% endhighlight %}

## 安装驱动

驱动包下载：[OS-X-Fake-PCI-ID](https://bitbucket.org/RehabMan/os-x-fake-pci-id/downloads/RehabMan-FakePCIID-2015-0108.zip)

将以上的zip包解压后，可以看到如下文件（根据需要选择Debug版或Release版）：
- FakePCIID.kext
- FakePCIID_AR9280_as_AR946x.kext
- FakePCIID_BCM94352Z_as_BCM94360CS2.kext
- FakePCIID_HD4600_HD4400.kext
- FakePCIID_Intel_HDMI_Audio.kext

FakePCIID.kext为必装，其它的驱动根据自己的硬件情况选装，本例中我们选择：FakePCIID_HD4600_HD4400.kext

重构缓存，重新启动系统，大功告成！
