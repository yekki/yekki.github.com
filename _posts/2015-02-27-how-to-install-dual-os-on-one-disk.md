---
layout: post
title: "如何在一块磁盘上安装双系统"
modified: 2015-02-27 11:34:19 +0800
tags: [rufus,双系统,黑苹果]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

由于x220的VGA口不给力（关于Hackintosh VGA的问题以后详谈），为应对不时之需，不得不考虑安装个Windows系统做备胎。为节省磁盘，决定尝试在一块SSD磁盘上安装双系统。
注：本文只记录关键点，细节各位自己研究。

整个安装过程比较简单，其中关键步骤为：
 
- 制作Windows（这里主要指Windows 7/8，XP啥的老货就不考虑了）安装盘
- 磁盘分区与Windows安装
- Clover配置

## Windows安装盘制作
这里需要注意，这里的安装盘一定要支持UEFI，不是随便拿个安装盘就可以使用的，由于制作UEFI安装盘比较麻烦，这里推荐个工具：[rufus](https://rufus.akeo.ie/downloads/)，这东东真心好使，只要插入USB盘，选择好安装盘格式及.iso文件即可。

![内存跑分](/upload/images/rufus.png)

## 磁盘分区与Windows安装
很多文章都说先装Windows再装OSX，其原因是老一代玩家都是用变色龙啥的，那个东东不支持EFI，另外，老版本的Windows也没有EFI支持，但是现在，这些都不是事儿了，完全可以根据需求选择安装顺序，我个人就是先装的OSX。在OSX中，用『磁盘工具』为Windows留空未分区空间即可，切记，不要用此工具创建DOS或NFS分区！
Windows安装过程没什么好讲的，只是在选择安装分区时选择在未分区空间创建分区即可，剩下步骤和传统的Windows安装无区别。


## Clover配置
安装完Windows后，磁盘的启动就被Windows Boot Manager接管了，这不是我们想要的结果，我们更希望由Clover统一管理。如何实现呢？

挂载EFI分区，将/EFI/CLOVER/CLOVERX64.efi复制到/EFI/Microsoft/Boot/目录下，将/EFI/Microsoft/Boot/目录下的bootmgfw.efi重命名为bootmgfw-orig.efi（这里需要注意了，必须用bootmgfw-orig.efi这个文件名），最后，将复制来的CLOVERX64.efi改名为bootmgfw.efi。

Clover会自动识别Windows，你在启动进入Clover界面时会看到有个Windows的选项，如果你对于显示的图标下面的文字不满意或想把这个启动项隐藏起来，你可以在用Clover Configurator的Gui段添加启动项。