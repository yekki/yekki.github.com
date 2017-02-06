---
layout: post
title: "OSX El Capitan中如何修复启动花屏"
modified: 2016-01-10 11:50:11 +0800
tags: [El Capitan,花屏,黑苹果]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
config.plist:

{% highlight xml %}
<key>KextsToPatch</key>
<array>
<dict>
    <key>Comment</key>
    <string>Boot graphics glitch, (credit lisai9093, cecekpawon)</string>
    <key>Disabled</key>
    <true/>
    <key>Find</key>
    <data>AQAAdRc=</data>
    <key>Name</key>
    <string>IOGraphicsFamily</string>
    <key>Replace</key>
    <data>AQAA6xc=</data>
</dict>
</array>
{% endhighlight %}

 注：开启CSM或legacy boot (continue to boot UEFI)效果更佳
