---
layout: post
title: "如何在OS X El Capitan系统中修复Intel USB端口"
modified: 2016-01-05 20:49:55 +0800
tags: [USB,EL Capitan,黑苹果]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
EI Capitan 引入了依据SMBIOS信息限制USB端口的新机制，如果在不借助GenericUSBXHCI的前题下原来正常工作的USB端口在升级到10.11后不工作了，多半是由于此机制作怪。

## 加载USB Kexts

**USB 2.0**

/System/Library/Extensions/IOUSBHostFamily.kext/Contents/PlugIns/AppleUSBEHCIPCI.kext/Contents/Info.plist

![EHCI Info](/upload/images/ehci_info.png)

- IONameMatch：用于匹配USB设备名，一般为EHC1 & EHC2。
- port-count：最大USB端口地址（此处命名不合理）
- port：USB端口地址
- UsbConnector：接口类型（0:USB2 type-A, 3:USB3 type-A, 255: proprietary）

如上图，对于iMac13，只有一个USB设备（EHC1），此USB设备只有一个USB端口，USB端口地址为1，类型为proprietary。如果DSDT中USB设备与SMBIOS定义的不同，那么USB端口就不会工作了。

**USB 2.0 & USB 3.0**

/System/Library/Extensions/IOUSBHostFamily.kext/Contents/PlugIns/AppleUSBXHCIPCI.kext/Contents/Info.plist

AppleUSBXHCIPCI与AppleUSBEHCIPCI略有不同，AppleUSBXHCIPCI主要是依据IOPCIPrimaryMatch，也就是说依据USB 3设备在DSDT中的设备号。

![XHCI Info](/upload/images/xhci_info.png)

## USB端口限制

USB如果要工作正常，要满足：

- DSDT中USB设备名要与SMBIOS类型定义相同
- DSDT中USB端口数量小于等于SMBIOS中定义的端口数
- DSDT中USB端口地址与SMBIOS中定义的端口地址相同

注：对于USB 3.0设备，如果DSDT中的名子不是XHC1，那么，缺省就没有USB端口限制，当然，如果工作不正常，这就需要恢复USB端口限制功能，手工添加端口限制。

例如：MacBookPro 9,2 可用USB端口地址为：1, 2, 5, 6：

![MacBookPro92](/upload/images/mac_pro92.png)


### 方法一：移除USB端口限制

对于 USB 3.0，你可以移除port-count与ports限制，或者干脆在DSDT中重命名USB设备名为XHC（任意名子，只要不是XHC1）。
一些SMBIOS，例如：iMac14,x 对于USB 2.0并没有端口限制，所以，如果由于USB 2.0端口不正常，可以尝试这些SMBIOS。

![Remove Ports](/upload/images/remove_ports.png)

### 方法二：扩展USB端口限制

- 对于 USB 2.0，在DSDT中重命名USB设备名为EHC1/EHC2，或者直接改IONameMatch
- 对于 USB 3.0, 在DSDT中重命名USB设备名为XHC1，或者更改IONameMatch

例如：下面DSDT中XHC1端口地址为：1, 2, 3, 4

![XHC DSDT](/upload/images/dsdt_xhc.png)

然而，SMBIOS MacBookPro9,2 定义的端口地址为：1, 2, 5, 6

![MacBookPro92](/upload/images/mac_pro92.png)

所以，需要手工添加端口地址：3, 4

![Add Port](/upload/images/add_port.png)

注：USB端口总数不能超过15个

## 注入 USB Kexts

为了避免升级造成的麻烦，我们可以使用驱动注入的方式实现USB修复。
如果是EHCI，在DSDT中重命名EHC1/EHC2，例如：EH01/EH02，这样原生的Apple kexts中的端口限制就失效了，然后编写一个injector kext为EH01/EH02添加USB端口信息。

对于XHCI，同样修改DSDT中的设备名XHC1，例如：XHC，然后然后编写一个injector kext为EH01/EH02添加USB端口信息。

下载：[Info.plist](/upload/download/usb_Info.plist.zip)


