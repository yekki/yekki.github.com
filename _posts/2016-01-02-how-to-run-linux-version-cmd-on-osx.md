---
layout: post
title: "如何在OSX上运行Linux版本的命令"
modified: 2016-01-02 14:47:14 +0800
tags: [GNU,Shell]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
有个Bash Shell脚本在Linux环境下运行正常，但是在OSX上运行报错。通过分析报错信息，发现问题是由于xargs命令在不同的平台上表现不同造成的。如果不想在OSX平台上修改脚本的情况下正常运行，就必须在脚本中调用Linux版本的xargs，那么如何在OSX运行Linux版本命令并且不与OSX相同的命令发生冲宊呢？经过研究，解决方法如下：

{% highlight bash %}
# Install GNU core utilities (those that come with OS X are outdated)
brew install coreutils

# Install GNU `find`, `locate`, `updatedb`, and `xargs`, g-prefixed
brew install findutils

# Install Bash 4
brew install bash

# Install gnu-tar, g-prefixed
brew install gnu-tar

# Install pcregrep. Learn it, live it, love it.
brew install pcre
{% endhighlight %}

安装完成后，OSX上就具有了Linux版本的相关命令了，不同的是，要运行Linux版本的命令就需要在命令前多加一个字母'g'，例如：gxargs。

接下来，如果想让脚本在Linux与OSX下都能正常运行，只需如下修改：

{% highlight bash %}
XARGS=xargs

if [[ $OSTYPE == "darwin"* ]]; then XARGS=gxargs;fi
{% endhighlight %}