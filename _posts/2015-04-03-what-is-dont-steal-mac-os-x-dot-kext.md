---
layout: post
title: "Dont Steal Mac OS X.kext 有什么用"
modified: 2015-04-03 12:53:43 +0800
tags: [DSMOS,解密,SMC]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
无意中发现在SLE目录下有个Dont Steal Mac OS X.kext这么个东东，苹果居然用这么个雷人名子！这玩意儿何用呢？查了下资料，原来这东东是防止不良用户将苹果操作系统安装在PC兼容机上的！具体来讲，这东东就是用来解密的！解密啥？看看他包含的解密函数名page_transform()便知，它是用来解密内存页的。也就是说，Dont Steal Mac OS X.kext先从SMC(System Management Controller，眼熟吧？这东东只有苹果上有，还记得黑苹果上的FakeSMC不，对，就是它！)拿到密钥，然后Dont Steal Mac OS X.kext用此密钥解密加载到内存页的程序。有很多用户安装黑苹果时总是停在『Waiting for DSMOS』，这里的DSMOS就是Dont Steal Mac OS X.kext！