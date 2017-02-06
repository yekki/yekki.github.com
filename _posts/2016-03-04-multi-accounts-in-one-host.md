---
layout: post
title: "如何在一台机器上使用多个github账号"
modified: 2016-03-04 01:32:10 +0800
tags: [Github,账号]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---
想建个金融理财相关的博客，所以想用个新的github账号，以区别现在这个技术为主的博客。这就需要在一台机器上支持多个github账户，配置过程如下：

## 生成SSH Key

{% highlight bash %}
cd ~/.ssh
ssh-keygen -t rsa -C "your-email-address"
{% endhighlight %}

存储key的时候，不要覆盖现有的id_rsa，使用一个新的名字，比如id_rsa_m

## 把id_rsa_m.pub添加到github账号中
进入github，在右上角点击账号，在下拉菜单中点击"Settings"，在左边点击"SSH Keys"，点击右边的“New SSH Key”，把id_rsa_m.pub内容复制过去即可。

## 添加Key到SSH Agent上

{% highlight bash %}
ssh-add ~/.ssh/id_rsa_m
{% endhighlight %}

查看：
{% highlight bash %}
ssh-add -l
{% endhighlight %}

## 配置config文件
编辑（没有就创建）~/.ssh/config，内容如下：

{% highlight bash %}
Host github.com
  HostName github.com
  IdentityFile ~/.ssh/id_rsa

Host github_blog
  HostName github.com
  IdentityFile ~/.ssh/id_rsa_m
{% endhighlight %}

## 提交博客内容

{% highlight bash %}
git init
git commit -m 'init'
git remote add origin git@github_blog:moneyniu/moneyniu.github.com.git
git push origin master
{% endhighlight %}

