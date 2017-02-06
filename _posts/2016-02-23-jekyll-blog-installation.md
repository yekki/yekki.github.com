---
layout: post
title: "本博客安装简略"
modified: 2016-02-23 22:32:39 +0800
tags: [Jekyll,安装,博客]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

## 下载博客项目
{% highlight bash %}
git clone https://github.com/yekki/yekki.github.com
cd yekki.github.com
{% endhighlight %}

## 穿越GFW

{% highlight bash %}
gem sources --remove https://rubygems.org/

gem sources -a https://ruby.taobao.org/

gem install bundle

bundle config 'mirror.https://rubygems.org' 'https://ruby.taobao.org'
{% endhighlight %}

## 安装博客

{% highlight bash %}
gem install jekyll
bundle install
{% endhighlight %}

## 配置域名
在根目录下创建文件CNAME，内容为一行域名，例如：

{% highlight text %}
www.yekki.me
{% endhighlight %}

记得修改_config.yml，尤其是记得添加域名配置
{% highlight text %}
url:  http://www.yekki.me
{% endhighlight %}

## 备注
做完升级后想执行 jekyll server 预览效果，结果报如下错：

{% highlight text %}
Configuration file: /Users/gniu/Workbench/blogs/yekki/_config.yml
  Dependency Error: Yikes! It looks like you don't have jekyll-sitemap or one of its dependencies installed. In order to use Jekyll as currently configured, you'll need to install this gem. The full error message from Ruby is: 'cannot load such file -- jekyll-sitemap' If you run into trouble, you can find helpful resources at http://jekyllrb.com/help/!
  jekyll 3.1.2 | Error:  jekyll-sitemap
{% endhighlight %}


修复办法：

{% highlight bash %}
bundle update
{% endhighlight %}

