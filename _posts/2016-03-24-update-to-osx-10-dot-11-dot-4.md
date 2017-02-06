---
layout: post
title: "Toshiba Portege Z30-B 升级到OSX 10.11.4"
modified: 2016-03-24 21:54:29 +0800
tags: [升级,Z30-B,黑苹果]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

今天将Toshiba Portege Z30-B升级到OSX 10.11.4（为了安全起见，我使用App Store）。升级完重启就再来启动不起来了，我只能做了一个USB启动盘，启动时用-v参数，得到以下信息：

![OsxAptioFix2Drv-64.efi](/upload/images/OsxAptioFix2Drv-64.jpg)

Google了半天，说是将OsxAptioFixDrv-64.efi换成OsxAptioFix2Drv-64.efi即可，经过尝试，果然有效果！

