---
layout: post
title: "内核开发快速入门"
modified: 2016-12-23 22:33:12 +0800
tags: [开发,内核]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

开发环境：Xcode 8.2.1 on Sierra

新建项目时选择：Generic Kernel Extention

测试代码如下：

{% highlight cpp %}
#include <mach/mach_types.h>
#include <libkern/libkern.h>

kern_return_t HelloWorld_start(kmod_info_t * ki, void *d);
kern_return_t HelloWorld_stop(kmod_info_t *ki, void *d);

kern_return_t HelloWorld_start(kmod_info_t * ki, void *d)
{
    printf("Hello World\n");
    return KERN_SUCCESS;
}

kern_return_t HelloWorld_stop(kmod_info_t *ki, void *d)
{
    printf("Goodbye World\n");
    return KERN_SUCCESS;
}
{% endhighlight %}

注：无需引入 Kernel.framework

修改Info.plist，在OSBundleLibraries下面添加如下项：

com.apple.kpi.libkern | String | 16.3

这里的16.3怎么来的？当然，有公式可以算出来，但是更简单的办法是通过以下命令：

{% highlight bash %}
kextlibs HelloWorld.kext
{% endhighlight %}

输出如下：
{% highlight bash %}
For all architectures:
    com.apple.kpi.libkern = 16.3
{% endhighlight %}

这个16.3就是了！

编译后将生成的HelloWorld.kext（在xcode中右键点击Products下的HelloWorld.kext）复制到桌面，执行以下命令：

{% highlight bash %}
sudo chown -R root:wheel HelloWorld.kext
sudo kextutil HelloWorld.kext //此处用kextload也可以，用kextutil的好处是可以看到更多的诊断信息
{% endhighlight %}

查看输出，Sierra中看内核日志与以前版本不同，不能使用Console.app了，要输入以下命令：

{% highlight bash %}
log show --predicate "processID == 0" --start 2016-12-23 --debug |grep HelloWorld
{% endhighlight %}

命令输出：

{% highlight bash %}
2016-12-23 22:29:22.596949+0800 0x2f419    Default     0x0                  0      kernel: (HelloWorld) Hello World
{% endhighlight %}

卸载：
{% highlight bash %}
sudo kextunload HelloWorld.kext
{% endhighlight %}
