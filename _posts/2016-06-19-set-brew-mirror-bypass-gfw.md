---
layout: post
title: "如何配置brew镜像"
modified: 2016-06-19 12:45:02 +0800
tags: [GFW,brew,镜像]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

今天升级iTerm2时报如下错误（估计又是GFW的缘故）：

{% highlight text %}
curl: (35) Server aborted the SSL handshake
Error: Download failed on Cask 'iterm2' with message: Download failed: https://iterm2.com/downloads/stable/iTerm2-2_1_4.zip
The incomplete download is cached at /Library/Caches/Homebrew/iterm2-2.1.4.zip
{% endhighlight %}

为了绕过GFW，只能找个镜像网站了，方法如下：

{% highlight bash %}
cd /usr/local
git remote set-url origin git://mirrors.ustc.edu.cn/brew.git
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bashrc
{% endhighlight %}

如果使用zsh，将.bashrc替换成相关配置文件。