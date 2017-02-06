---
layout: post
title: "如何修改Launchpad图标尺寸"
modified: 2015-02-18 13:53:55 +0800
tags: [图标,Launchpad]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
我的Lenovo X220中默认的Launchpad中的图标大小非常别扭（图标间距很大，图标很小），产生这个问题的原因是，Launchpad中的图标以网格排列，由于默认的行数太多，而图标横宽比是固定的，所以为了显示所有的图标，系统不得不把图标变小，而列数又不变，所以图标的间距显得很大。

解决这个问题的方法是通过如下命令：

**设置行数与列数**：
{% highlight bash %}
$defaults write com.apple.dock springboard-rows -int 4
$defaults write com.apple.dock springboard-columns -int 7
$killall Dock
{% endhighlight %}

**恢复默认行数与列数**：
{% highlight bash %}
$defaults delete com.apple.dock springboard-rows
$defaults delete com.apple.dock springboard-columns
$killall Dock
{% endhighlight %}

还有一个命令是删除图标分组（真心觉得这命令没啥用，没事儿别乱试，重新分组老麻烦了！）

{% highlight bash %}
$defaults write com.apple.dock ResetLaunchPad -bool TRUE;killall Dock
{% endhighlight %}