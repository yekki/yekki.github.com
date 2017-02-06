---
layout: post
title: "Apple SIP简介及在Clover中如何控制"
modified: 2016-01-10 10:16:21 +0800
tags: [SIP,Clover,安全,El Capitan,黑苹果]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

## 什么是Apple SIP
Apple SIP（System Integrity Protection）机制是OSX 10.11开始启用的一套关键的安全保护技术体系。
SIP技术的整个体系主要分为：

- 文件系统保护（Filesystem protection）

对于系统文件通过沙盒限制root权限，比如：就算你有root根限，也无法往/usr/bin目录写入。

- 运行时保护（Runtime protection）

受保护的关键系统进程在运行状态下无法被代码注入，挂调试器调试，以及限制内核调试等

- 内核扩展签名（Kext signing）

10.10中强制要求签名，要想绕过这个限制，就必需加入启动参数“kext-dev-mode=1”（10.11 DB5开始，"rootless=0"的启动参数也被废除了），这个启动参数在10.11中被废除。另外，10.11官方要求第三方kext必须被安装至/Library/Extensions。

## Apple官方如何对SIP保护技术进行配置

- 进入10.11的安装程序或Recovery HD

使用其中所带的终端进行相关操作。在此环境下，由于特殊启动标志位的存在，整个SIP保护技术处在完全关闭状态。可正常修改受保护文件的权限以及所有者。

- Apple已经提供了csrutil程序来配置SIP

此程序在正常系统环境下只支持检查当前SIP开关状态，若需要开关或调整SIP状态必须前往Recovery或安装程序中执行。
注意：该工具修改SIP配置的功能仅在部分原生支持NVRAM写入的机子上有效。

**csrutil命令行工具使用方法：**

- 完全启用SIP(csr-active-config=0x10)
{% highlight bash %}
csrutil enable
{% endhighlight %}

- 也可以添加更多参数实现按需开关各项保护技术
{% highlight bash %}
csrutil enable [--without kext|fs|debug|dtrace|nvram] [--no-internal]
{% endhighlight %}

- 禁用SIP(csr-active-config=0x77)
{% highlight bash %}
csrutil disable
{% endhighlight %}

等效命令：
{% highlight bash %}csrutil enable --without kext --without fs --without debug --without dtrace --without nvram
{% endhighlight %}

- 清除SIP标志位（将csr-active-config项从NVRAM中移除，等同于SIP完全开启）：
{% highlight bash %}
csrutil clear
{% endhighlight %}

- 检查并报告当前SIP开关详细状态
{% highlight bash %}
csrutil status
{% endhighlight %}
## 其它可以控制SIP方法 

- 借助Clover（版本>=3250）实现SIP控制

{% highlight xml %}
<key>RtVariables</key>
        <dict>
                <key>CsrActiveConfig</key>
                <string>0x13</string>
                <key>BooterConfig</key>
                <string>0x49</string>
        </dict>
{% endhighlight %}

- 借助第三方应用（例如：SIPUtility.app）
这类应用一般需要在Recovery环境下使用，并且要求原生支持NVRAM写入，尽量避免使用。

- 直接写入NVRAM

例如：
{% highlight bash %}
sudo nvram 7C436110-AB2A-4BBB-A880-FE41995C9F82:csr-active-config=%13%00%00%00
{% endhighlight %}

## SIP自定义配置及各标志位的含义

举例：csr-active-config=0x13，0x13 = 0b00010011
{% highlight xml %}
____ ___1 (bit 0): [kext] 允许加载不受信任的kext（与已被废除的kext-dev-mode=1等效）
____ __1_ (bit 1): [fs] 解锁文件系统限制
____ _1__ (bit 2): [debug] 允许task_for_pid()调用
____ 1___ (bit 3): [n/a] 允许内核调试 （官方的csrutil工具无法设置此位）
___1 ____ (bit 4): [internal] Apple内部保留位（csrutil默认会设置此位，实际不会起作用。设置与否均可）
__1_ ____ (bit 5): [dtrace] 解锁dtrace限制
_1__ ____ (bit 6): [nvram] 解锁NVRAM限制
1___ ____ (bit 7): [n/a] 允许设备配置，用于Recovery/安装环境
{% endhighlight %}

**更多示例及建议值：**

- Clover(需要修改原版kext但未使用kextpatch)，建议仅解锁kext加载和文件系统限制：

csr-active-config=0x13或0x3(csrutil enable --without kext --without fs [--no-internal])

- Clover(已正确配置kextpatch对原版kext进行修改)，建议仅解锁kext加载限制以加载第三方未签名kext：
csr-active-config=0x11或0x1 (csrutil enable --without kext [--no-internal])

- Clover(愿意依赖Kext注入功能+已正确配置kextpatch对原版kext进行修改)，可完全开启SIP：

csr-active-config=0x10或0x0 (csrutil enable [--no-internal] 或 curutil clear)

注：部分kext无法通过Clover的kext注入来正常工作，例如AppleHDA Injector，CodecCommander.kext等。

- 关闭SIP中的所有防护，不推荐：

csr-active-config=0xff
在非必要的情况下，不要把提供的这些保护全部关闭，也尽量避免使用Clover默认注入的0x67参数。


至于系统启动标志位，即Clover的config.plist中BooterConfig的含义：

举例：BooterConfig=0x49, 0x0049 = 0b001001001
{% highlight xml %}
_ ____ ___1 (bit 0): RebootOnPanic，遇到内核崩溃自动重启
_ ____ __1_ (bit 1): HiDPI，在启动过程中使用HiDPI模式显示
_ ____ _1__ (bit 2): Black，在启动过程中不显示进度条
_ ____ 1___ (bit 3): CSRActiveConfig，将读取当前生效的SIP控制标志位
_ ___1 ____ (bit 4): CSRConfigMode，仅用于Recovery/安装环境，将允许对SIP进行配置
_ __1_ ____ (bit 5): CSRBoot，仅用于Recovery/安装环境，SIP将完全禁用
_ _1__ ____ (bit 6): BlackBg，在启动过程中使用黑色背景
_ 1___ ____ (bit 7): LoginUI，在启动过程中使用登陆界面作为背景
1 ____ ____ (bit 8): InstallUI
{% endhighlight %}

把需要的打开的启动标志位设置为1即可。BooterConfig设置为Clover自有设置，可能需要机器支持原生NVRAM读写方可生效。如无特殊需要建议可以不在config.plist中添加此项，等待Clover的后续更新。

## SIP对于kext修改及安装的影响
这方面的影响主要体现在强制kext签名以及文件系统保护2个方面：

- kext签名保护：由于10.10中可用的kext-dev-mode=1启动参数在10.11中已经废除，请参照上面配置SIP以关闭此项保护。
Clover的kext注入功能可绕过kext签名保护从而强制加载第三方kext，但部分kext使用注入功能可能无法正常工作，例如AppleHDA Injector，CodecCommander.kext等。

- 文件系统保护：如不考虑安全性需求，也请参照上面配置SIP以关闭此项保护，关闭之后即可对任意系统文件进行修改，包括直接修改"/System/Library/Extensions"下的原版kext。
如希望此项技术保持开启，则"/System/Library/Extensions"目录中的kext受到保护，无法直接进行修改。Apple要求第三方kext必须安装至"/Library/Extensions"，此目录并不在保护列表中，因此可在root权限下自由操作。若有需要对SLE下原版kext进行修改也可通过充分利用Clover Kextpatch/kext Injector等技术实现。








