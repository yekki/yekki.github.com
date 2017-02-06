---
layout: post
title: "理解命令行应用中的几个概念"
modified: 2015-02-20 16:31:10 +0800
tags: [开发]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
最近在重构[hackintosh-laptop](https://github.com/yekki/hackintosh-laptop)，其中用到非常多的脚本知识，脚本这东东是非常吃经验的，而这也是我的弱项。因此，今天起我会分享些重构过程中我认为重要的知识点。
先看一个典型的命令行：

{% highlight bash %}
grep --ignore-case -r "some string" /tmp
{% endhighlight %}

这个典型的命令行中分为三部分：

- 可执行命令：grep
- 选项（Options）：--ignore-case与-r
- 参数（Arguments）：some string与/tmp

选项主要作用是改变应用的行为，比如："ls -l"，"-l"这个选项就是改变文件列表的显示方式。
选项分两种表现形式，即：长格式与短格式。例如:上面的"--ignore-case"就是长格式，注意前面的"--"；而"-l"就是短格式。短格式可以将多个选项写在一起，例如："ls -l -a -t"也可以写作"ls -lat"，当然，长格式更具描述性。

选项可以带参数也可以不带参数，带参数叫作标志（Flag），不带叫作开关（Switch）。长格式标志要用"="赋参数值，例如：

{% highlight bash %}
curl --request=POST http://www.google.com
{% endhighlight %}

长格式开关的开与关标志一般如下表示：--foo和--no-foo。

再复杂一点的命令行应用会引入命令（Commmands）这一概念，例如：

{% highlight bash %}
git --no-papger push -v origin_master
{% endhighlight %}

这个命令行分为五个部分：

- 可执行命令：git
- 全局选项（Global Options）：--no-pager
- 命令（Command）：push
- 命令选项（Command Options）：-v
- 参数（Arguments）：origin_master

全局选项顾名思义应该是对所有的命令起作用，而命令选项则是只适用于其所跟随的命令。命令的命名应该简短明确。
