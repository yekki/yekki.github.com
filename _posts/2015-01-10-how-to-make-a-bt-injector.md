---
layout: post
title: "如何制作蓝牙注入式驱动"
modified: 2015-01-10 18:05:52 +0800
tags: [蓝牙,驱动,黑苹果]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

如果你使用外接蓝牙适配器，多数情况不会完美，例如：无法控制蓝牙设备的'打开/关闭'。毕竟我们不能保证我们使用的蓝牙适配器型号与白苹果完全一样，对于此类问题，我们可以借助苹果驱动的注入技术来解决。

这里先解释下什么是驱动注入。简单来讲，就是有些硬件设备虽然供应商或产品型号不同，但是其芯片与驱动却是相同的。也就是说，在很多情况下，苹果自带的驱动其实是可用的，只是苹果没考虑到众多的外接设备（毕竟人家不会支持黑苹果），因此，其并没有将各种设备的信息（例如：供应商ID与产品ID）写入驱动，从而造成由于驱动不能正确加载而带来的各种问题。由此可见，既然驱动可用，只是供应商ID与产品ID对不上，我们就可以写一个驱动，这个驱动只有一个Info.plist描述文件，此文件中加入我们设备的供应商ID与产品ID，同时，指明此设备所需要使用的驱动类文件（至于其具体的原因与原理，由于相对复杂，这里不赘述，其后博文详叙），这样就可以正确加载硬件设备驱动了。这种技术我们叫作驱动注入（Injector），那么如何实现呢？


## 获取设备信息
点击左上角的苹果图标，选择『关于本机』->『系统报告』，从左侧栏选择『蓝牙』，以我自己的环境为例，如图：

![蓝牙](/upload/images/bt_info.png)

这里我们可以看到，供应商ID为0x0A12（十进制2578），产品ID为0x0001(十进制1)。

## 制作驱动
我们可以借助以下命令生成驱动文件：

{% highlight bash %}
mkdir ~/Desktop/YekkiBluetoothInjector.kext
mkdir ~/Desktop/YekkiBluetoothInjector.kext/Contents
cp /System/Library/Extensions/IOBluetoothFamily.kext/Contents/PlugIns/BroadcomBluetoothHostControllerUSBTransport.kext/Contents/Info.plist ~/Desktop/YekkiBluetoothInjector.kext/Contents/Info.plist
​{% endhighlight %}

找到生成驱动中的Info.plist，在IOKitPersonalities下面添加如下代码（注意：填入上面得到的供应商ID与产品ID）：

{% highlight xml %}
<key>Yekki Bluetooth 2.0 USB Dongle</key>
<dict>
	<key>CFBundleIdentifier</key>
	<string>com.apple.iokit.BroadcomBluetoothHostControllerUSBTransport</string>
	<key>IOClass</key>
	<string>BroadcomBluetoothHostControllerUSBTransport</string>
	<key>IOProviderClass</key>
	<string>IOUSBDevice</string>
	<key>idProduct</key>
	<integer>1</integer>
	<key>idVendor</key>
	<integer>2578</integer>
</dict>
{% endhighlight %}

## 安装驱动
根据个人喜好选择驱动安装方式，我个人喜欢使用Kext Wizard，这里不详述。记得安装完成后重新启动使其生效。

