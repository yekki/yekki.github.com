---
layout: post
title: "IOKit基础知识"
modified: 2015-03-16 14:11:15 +0800
tags: [IOKit,驱动,开发]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
## Families、Drivers、Nubs
一个Family以C与C++类定义了一类设备中所有设备通用功能的高度抽象。Families为很多类型的设备提供服务，例如：总线协议（例如：SCSI Parallel、USB与FireWire）、存储服务、网络服务、人机交互（键盘、鼠标等）等。
在Family中的所有驱动可以方便的共享Family实现的抽象通用功能（例如：所有的SCSI控制器共享SCSI Family总线扫描功能）。Family是动态加载的，系统需要时加载，不需要时将其卸载。

一个驱动是一个I/O Kit对象，管理一个具体的设备或总线，为系统中的其它设备提供一个更加抽象的视图。当一个驱动加载时，与其相关的Families也将加载，这个关联关系在驱动的属性清单里定义。

一个Nub也是一个I/O Kit对象，用于定义一个驱动的连接点，用于表现一个可控实体，例如：磁盘或总线。一个Nub作为Family的一个部分被实例化，每个Nub提供设备或服务的访问，展现与提供匹配、仲裁与电源管理功能。Nub就象是电视机与有线服务接口中间的连线，一个驱动为其控制的每个设备或服务发布一个Nub。

举例：一个SCCI磁盘驱动如何连接到PCI Bus

1 PCI Bus驱动发现一个PCI设备，于是，创建一个Nub（IOPCIDevice）,Nub类是由PCI Family定义的。

![驱动举例](/upload/images/drv_1.png)

2 PCI Bus驱动通过驱动匹配查找驱动，最终SCSI控制器驱动被找到并加载，控制器驱动的加载导致其所需Families也被加载。如上例：SCSI Family与PCI Family被加载。SCSI控制器驱动拥有一个IOPCIDevice Nub的引用。

3 SCSI控制器驱动扫描SCSI总线，找到一个设备，并创建一个Nub(IOSCSIDevice，也是由SCSI Family定义)表示新发现设备。

![驱动举例](/upload/images/drv_2.png)

4 PCI控制器驱动通过匹配查找驱动，最终磁盘驱动被找到并加载。磁盘驱动加载导致其其所需Families也被加载。如上例：Storage Family与SCSI Family被加载。磁盘驱动拥有一个IOSCSIDevice Nub引用。

![驱动举例](/upload/images/drv_3.png)

当硬件接入主机时，系统会根据硬件类型创建此硬件的软件表示，即：nub。nub本身就归属于某个Family。例如：接入的是PCI设备，那么就会有一个IOPCIDevice类型的nub与之对应。借助nub，屏蔽硬件细节，为其驱动提供了一个硬件服务的软件服务抽象表示。而硬件所对应的具体软件表示就是驱动，驱动继承自某个Family，以获取此类设备通用服务与数据结构（例如：IOAudioDevice）。

驱动一般至少会与两个Family发生关系，一个服务提供者，即：为设备提供服务的上游对象，以nub形式表示；一个是父类，即：此驱动类的父类。

例如：

![驱动举例](/upload/images/audio_drv.png)

说明：声卡有可能基于USB，也有可能基于PCI，但不管基于什么，其核心功能都是一样的。此处I/O Kit的威力就显现了，我们的驱动继承自IOAudioDevice Family，以获取声卡实现的通用服务与数据结构，我们只需实现不同的服务提供者（分别基于IOPCIDevice与IOUSBDevice），即可屏蔽对于硬件访问的差异性。

## I/O Registry与I/O Catalog
I/O Registry是对于系统中当前硬件对应的软件动态表示。系统启动后会扫描当前机器上的硬件并加载相关驱动，创建驱动对象实例。这些驱动对象实例就记录在I/O Registry中。当硬件发生变化时（例如：拔下U盘），I/O Registry信息立即更新。

I/O Registry中的信息以树状表示（所有的驱动均继承自IOService，而IOService则继承自IORegistryEntry），查看可以用图形化工具IORegistryExplorer或命令行工具ioreg。

![驱动类继承关系](/upload/images/class_diagram.gif)

I/O Catalog则是对系统中所有驱动（不管当前有没有硬件激活）的软件静态表示。
当有硬件接入系统时，将首先在I/O Catalog中查找匹配，当找到相关驱动后则记录在I/O Registry中。

## 驱动匹配
- 类匹配：根据nub对应的服务提供者类筛选掉不匹配的驱动
- 被动（Passive）匹配：根据驱动Info.plist中的匹配列表筛选掉不匹配的驱动
- 主动（Active）匹配：调用驱动的probe方法，筛选掉不匹配驱动，最后probe分数最高的驱动胜出

## 设备匹配
用户应用需要访问某个设备时必需首先搜索到此设备并且获取合适的设备接口与之通讯。这个过程被称为设备匹配。与驱动匹配不同，设备匹配是搜索I/O Registry，即：搜索已加载驱动。基本过程如下：
- 获取Mach Port建立与I/O Kit的连接
- 创建设备属性字典（Dictionary），填入搜索所需设备属性
- 获取设备列表，定位所需设备
- 获取定位设备接口，访问之