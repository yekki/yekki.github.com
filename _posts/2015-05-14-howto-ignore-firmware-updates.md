---
layout: post
title: "如何忽略系统固件更新"
modified: 2015-05-14 12:42:33 +0800
tags: [固件,黑苹果,补丁]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

最近忙着挣钱儿，没时间更新博客，兴趣重要，但是，给女儿挣奶粉钱更重要啊！

言归正传，苹果推送了固件更新补丁，对于我的黑苹果，当然是没有这个所谓的固件了，因此，就算是安装了这个更新，还是会在系统中显示有1个可用更新，我是个完美主义者，怎么能允许这种情况发生呢，今天无意间找到了解决办法，只需打开终端输入如下命令：

{% highlight bash %}
sudo softwareupdate --ignore MacBookProEFIUpdate2.7
{% endhighlight %}

妥了！