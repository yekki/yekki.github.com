---
layout: post
title: "OSX与Windows双系统时间同步"
modified: 2015-03-03 10:37:47 +0800
tags: [时间,黑苹果]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
如果使用双系统，尤其是双系统中有Windows，那么就会出现问题不一致的问题。即：在Windows中把时间设置正确了过后，在Mac 中的时间又不一样了，在Mac中把时间设置正确后进入Windows后，时间又不一致了。出现这种情况的原因是Windows 、 Mac 在默认情况下看待硬件时间（主板上的BOIS显示的时间）的方式不一样。 知道了问题存在的原因，我们就来解决这个问题。
注：各种Linux与Mac情况是一样的，所以，此文也适用于各种Linux与Windows双系统环境。

这个是一个关于时间的问题，我们就先来了解一下关于时间的概念

- UTC即Universal Time Coordinated，协调世界时
- GMT即Greenwich Mean Time，格林尼治平时

Windows 与 Mac/Linux 缺省看待系统硬件时间的方式是不一样的：

- Windows把系统硬件时间当作本地时间(local time)，即操作系统中显示的时间跟BIOS中显示的时间是一样的。
- Linux/Unix/Mac把硬件时间当作 UTC，操作系统中显示的时间是硬件时间经过换算得来的，比如说北京时间是GMT+8，则系统中显示时间是硬件时间+8。

这样，当PC中同时有多系统共存时，就出现了问题。假如你的Mac和WindowsXP中设置的时区都为北京时间东八区，而你在Mac中把当前系统 时间更改为9：00AM。则此时硬件中存储的实际是UTC时间1:00AM。这时你重启进入Windows后，你会发现windows系统中显示的时间是 1:00AM，比Mac中慢了八个小时。同理，你在Windows中更改或用网络同步了系统时间后，再到Mac中去看，系统就会快了8小时。在实行夏令时的地区，情况可能会更复杂些。

解决这个问题的方法也有几种，可让Mac和Linux不使用UTC时间与 Windows 保持一致。但这样改就相对复杂，而且要修改Mac和 Linux两个系统（Linux/Unix/Mac都是把硬件时间当作UTC）。建议修改 Windows 对硬件时间的对待方式，这样只在Windows 上修改后就无需在 Mac和Linux上设置了。

**解决方法**：

让Windows把硬件时间当作UTC
开始->运行->CMD，打开命令行程序(Vista则要以管理员方式打开命令行程序方可有权限访问注册表)，在命令行中输入下面命令并回车
Reg add HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1

