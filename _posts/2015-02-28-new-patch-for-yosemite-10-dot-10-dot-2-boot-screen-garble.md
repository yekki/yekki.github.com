---
layout: post
title: "Yosemite 10.10.2 启动花屏补丁更新"
modified: 2015-02-28 09:10:38 +0800
tags: [补丁,黑苹果,花屏,Yosemite]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

关于启动花屏（包括在系统使用中花屏现象，例如：屏幕任一位置出现的无规则长方型或正方型的色块）的问题，参见我以前的博文：[《解决启动进度条花屏问题》](http://www.yekki.me/resolve-boot-screen/)，今天给出更新。

直接编辑：

{% highlight xml %}
<dict>
	<key>Comment</key>
	<string>Boot graphics glitch, 10.10.2 (1 of 2)</string>
	<key>Name</key>
	<string>IOGraphicsFamily</string>
	<key>Find</key>
	<data>QYjE6xE=</data>
	<key>Replace</key>
	<data>QYjE6zE=</data>
</dict>
<dict>
	<key>Comment</key>
	<string>Boot graphics glitch, 10.10.2 (2 of 2) (seems to have no effect)</string>
	<key>Name</key>
	<string>#IOGraphicsFamily</string>
	<key>Find</key>
	<data>hcB0a0g=</data>
	<key>Replace</key>
	<data>McB0W0g=</data>
</dict>
{% endhighlight %}

借助Clover Configurator:

{% highlight bash %}
Name: IOGraphicsFamily
Find: 4188C4EB11
Replace: 4188C4EB31
Comment: Boot graphics glitch, 10.10.2 (1 of 2)

Name: IOGraphicsFamily
Find: 85C0746B48
Replace: 31C0745B48
Comment: Boot graphics glitch, 10.10.2 (2 of 2)
{% endhighlight %}

