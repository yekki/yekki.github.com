---
layout: post
title: "如何修复T440P在Sierra上显示器设置面板异常的问题"
modified: 2016-10-08 19:07:30 +0800
tags: [Sierra,EDID,T440P]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

Sierra系统中，有些笔记本会出现“系统偏好设置->显示器”无法打开的问题。查了些资料，说这个问题是由于显示器的EDID数据与白苹果不匹配造成的。Lenovo T440P的解决方案如下：

## 工具软件准备

* [DarwinDumper v3.0.1](https://bitbucket.org/blackosx/darwindumper/downloads)
* [FixEDID 2.4](https://github.com/andyvand/FixEDID_Devel)

## 修复EDID数据

1. 用DarwinDumper生成EDID数据（过程略）
2. 打开FixEDID，"Open EDID binary file"选择DarwinDumper生成的EDID.bin,一定要选择“Apple MacBook Air Display (16:9)”，点击“Make”，将生成3个文件，其实有用的是DisplayMergeNub.kext，用Kext Wizard安装即可。

注：Lenovo T440P屏幕是16:9，而我用的SMBIOS是MacBookPro11,2（屏幕应该是16:10），也许问题就出在显示比例对不上。

![fixedid](/upload/images/fixedid.png)

## 后续

过两天试试用Clover注入EDID数据试试，兴许也可以！



