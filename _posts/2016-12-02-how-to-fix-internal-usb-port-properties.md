---
layout: post
title: "如何修复机器内部USB端口属性"
modified: 2016-12-02 11:48:30 +0800
tags: [USB]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

USB端口有个属性USBConnector，其取值有3个，分别是：0: USB2 type-A, 3: USB3 type-A, 255: proprietary。如果USB端口是个内部端口，例如：内置摄像头、蓝牙等，那么这个值应该取255。我们可以用[IORegistryExplorer](https://www.tonymacx86.com/attachments/ioregistryexplorer-slrid_v10-6-3-zip.24086/)来查看一下这个值。

例如，我的系统显示如下：

![Before Internal USB Fix](/upload/images/usb_fix_before.jpg)

要修改这个值，我用的办法如下：

首先，要屏蔽苹果内部的遮罩器，方法很多，最简单的是通过Clover的DSDT Patch实现，这里就不详述了。由于我的笔记本只有USB3总线，也就是只有XHC，这个就简单了，啥都不用做。

其次，安装Rehabman的[USBInjectAll.kext](https://github.com/RehabMan/OS-X-USB-Inject-All)

最后，制作SSDT补丁，这个有点麻烦了。

1、 在项目[https://github.com/RehabMan/OS-X-USB-Inject-All](https://github.com/RehabMan/OS-X-USB-Inject-All)找模板，里有几个SSDT开头的就是，我是用SSDT-UIAC-ALL.dsl。

2、确定模板中的有效代码片段。通过工具软件DPCIManager找到USB总线的Vendor ID与Product ID，例如：我的是8086:9CB1，那么在SSDT-UIAC-ALL.dsl中只保留对应的片段。

3、确定每个内置设备对应的USB端口。查看系统报告，找到内置USB设备对应的Location ID，然后通过IORegistryExplorer搜索XHC，找到这些Location ID对应的端口号。

例如：

![USB Location](/upload/images/usb_loc.jpg)


从上图可以看出，我这个蓝牙设备对应的是HS08。找出所有对应设备，然后在模板中将其对应的代码值改为255即可，例如：

在我的系统中对应的是HS06、HS07、HS08。

修改后的代码如下：

{% highlight text %}
// SSDT-UIAC-ALL.dsl
//
// This SSDT can be used as a template to build your own
// customization for USBInjectAll.kext.
//
// This SSDT contains all ports, so using it is the same as without
// a custom SSDT.  Delete ports that are not connected or ports you
// do not need.
//
// Change the UsbConnector or portType as needed to match your
// actual USB configuration.
//

DefinitionBlock ("", "SSDT", 2, "hack", "UIAC-ALL", 0)
{
    Device(UIAC)
    {
        Name(_HID, "UIA00000")

        Name(RMCF, Package()
        {
            "8086_9cb1", Package()
            {
                "port-count", Buffer() { 15, 0, 0, 0 },
                "ports", Package()
                {
                    "HS01", Package()
                    {
                        "UsbConnector", 3,
                        "port", Buffer() { 1, 0, 0, 0 },
                    },
                    "HS02", Package()
                    {
                        "UsbConnector", 3,
                        "port", Buffer() { 2, 0, 0, 0 },
                    },
                    "HS03", Package()
                    {
                        "UsbConnector", 3,
                        "port", Buffer() { 3, 0, 0, 0 },
                    },
                    "HS04", Package()
                    {
                        "UsbConnector", 3,
                        "port", Buffer() { 4, 0, 0, 0 },
                    },
                    "HS05", Package()
                    {
                        "UsbConnector", 3,
                        "port", Buffer() { 5, 0, 0, 0 },
                    },
                    "HS06", Package()
                    {
                        "UsbConnector", 255,
                        "port", Buffer() { 6, 0, 0, 0 },
                    },
                    "HS07", Package()
                    {
                        "UsbConnector", 255,
                        "port", Buffer() { 7, 0, 0, 0 },
                    },
                    "HS08", Package()
                    {
                        "UsbConnector", 255,
                        "port", Buffer() { 8, 0, 0, 0 },
                    },
                    "HS09", Package()
                    {
                        "UsbConnector", 3,
                        "port", Buffer() { 9, 0, 0, 0 },
                    },
                    "HS10", Package()
                    {
                        "UsbConnector", 3,
                        "port", Buffer() { 10, 0, 0, 0 },
                    },
                    "HS11", Package()
                    {
                        "UsbConnector", 3,
                        "port", Buffer() { 11, 0, 0, 0 },
                    },
                    "SSP1", Package()
                    {
                        "UsbConnector", 3,
                        "port", Buffer() { 12, 0, 0, 0 },
                    },
                    "SSP2", Package()
                    {
                        "UsbConnector", 3,
                        "port", Buffer() { 13, 0, 0, 0 },
                    },
                    "SSP3", Package()
                    {
                        "UsbConnector", 3,
                        "port", Buffer() { 14, 0, 0, 0 },
                    },
                    "SSP4", Package()
                    {
                        "UsbConnector", 3,
                        "port", Buffer() { 15, 0, 0, 0 },
                    },
                },
            },
        })
    }
}

//EOF
{% endhighlight %}


4、将此文件通过MaciASL编译，放入/EFI/EFI/CLOVER/ACPI/patched目录下，记得在config.plist的SortedOrder里加上此项。


修改后：

![After Internal USB Fix](/upload/images/usb_fix_after.jpg)


我查看了白苹果，内置设备确实是用的255，但是我并没有感觉这个值得不同对使用有什么影响（修改前是3，好象使用也不受什么影响），我原本是希望解决系统崩溃才修复这个问题的，但是似乎我的系统崩溃和这个没关系，不管怎么说，还是无限接近白苹果为主旨，至于背后的逻辑慢慢学习吧。

