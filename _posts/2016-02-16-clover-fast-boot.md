---
layout: post
title: "关于Clover快速启动参数问题的解决"
modified: 2016-02-16 19:33:35 +0800
tags: [Clover,启动,黑苹果]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
每次启动电脑时都要在Clover启动界面回车才能进入系统，这很麻烦，我想直接进入系统，如白苹果那样。用Clover Configurator打开config.plist，其中Boot->Fast选项可以实现这个功能。但是我选中此项后还是会显示Clover启动菜单，所不同的是这个启动菜单没有主题，而是一个亮蓝色背景，上面有两个黑色礼帽（上面还有苹果图标）代表的启动项。后来试了几次才搞明白，要想使用这个选项，必须填写此选项上面的“Default Boot Volume”字段。这个字段可以输入磁盘的UUID，当然，更简单的方法是填写磁盘名，例如，在我的电脑上磁盘名是：Hackintosh HD。重启，完美直接启动了！