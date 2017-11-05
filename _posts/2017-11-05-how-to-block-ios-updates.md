---
layout: post
title: "如何阻止iOS系统升级"
modified: 2017-11-05 09:44:01 +0800
tags: [iOS, 升级]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
自从将手机升级到11.2以后，屏幕一点亮就报下面信息（不停得报，烦得要死）：
![iOS升级提示](/upload/images/ios11_update_error.jpg)
我估计这是苹果的一个Bug!

*解决办法：*
用iOS自带的safari浏览器打开下面的描述文件，重新启动手机即可！
[tvos11betaprofile.mobileconfig](/upload/download/tvos11betaprofile.mobileconfig)
注：亲测有效！

另外，如果你手机型号比较老，不想升级到iOS11，可以用相同的办法安装下面这个：
[tvos11betaprofile.mobileconfig](/upload/download/NOOTA9.mobileconfig)
注：我自己没有iOS10，所以这个我自己没式过。

最后，以上方法将屏蔽iOS的升级，如果想升级系统，需要在"设置--通用--描述文件"中删除相应用描述文件。