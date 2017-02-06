---
layout: post
title: "如何通过命令行批量重定义图片尺寸"
modified: 2017-01-17 16:53:06 +0800
tags: [图片]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
为了应付考试，同事发来了Brain Dump，由于题目是通过iPhone手机拍的，所以图片尺寸很大，足足有140多兆。其实我根本用不了这么高的精度，因此，想对这些图片进行批量处理，缩小图片尺寸。经过Google，发现这事儿很简单，只需一行命令即可：

{% highlight bash %}
sips -Z 640 *.JPG
{% endhighlight %}

注：我记不得sips是不是原生命令了，另外，我这里用了640，你也可以根据自己的需求改成其它值来控制图片大小。最后处理完我看了下，具然总文件尺寸减到了6兆，打开每张看了看，能用！