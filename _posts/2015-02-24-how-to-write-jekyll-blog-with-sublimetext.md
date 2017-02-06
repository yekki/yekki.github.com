---
layout: post
title: "如何用Sublime Text写Jekyll博客"
modified: 2015-02-24 11:26:30 +0800
tags: [博客,Jekyll,Sublime Text]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

这个网站就是用Jekyll（如果不知道是啥，自己研究下：[http://jekyllrb.com/](http://jekyllrb.com/)）维护的，而Sublime Text又是现在很火的编辑工具（真心好用，还跨平台），那么问题来了，如何用Sublime Text写Jekyll博文呢？下面博文只介绍下相关要点，避免大家走弯道，细节部分还请自己研究。

## 如何翻墙
这个本土问题一定要说道说道。安装Sublime Text完毕后一定要安装Package Control（不明白的自己研究：[https://packagecontrol.io/](https://packagecontrol.io/)）这个插件，这基本上是约定谷成的。安装这个东东没啥难的，网站上有写，可问题是你为了翻墙用了代理，这可咋办？你可以用如下命令安装，根据自己的情况替换代理设置（注意：urllib.request.ProxyHandler({"http":"http://[proxy_username]:[proxy_password]@[proxy_IP_or_host]:[proxy_port]"})) 部分）

{% highlight python %}
import urllib.request,os,hashlib; h = '7183a2d3e96f11eeadd761d777e62404e330c659d4bb41d3bdf022e94cab3cd0'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler({"http":"http://[proxy_username]:[proxy_password]@[proxy_IP_or_host]:[proxy_port]"})) ); by = urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by);
{% endhighlight %}

我们要通过Package Control安装其它插件，这时，如何设置代理呢？
点击：『Sublime Text』->『Preferences』->『Package Settings』->『Package Control』->『Settings - User』，打开Package Control.sublime-settings文件后添加（根据自己的情况修改与增减）：

{% highlight bash %}
"http_proxy": "[proxy-host]:[proxy-port]",
"https_proxy": "[proxy-host]:[proxy-port]",
"proxy_username":"[pass]",
"proxy_password":"[user]"
{% endhighlight %}

## 安装 Jekyll插件
这里用的Sublime Text插件是[sublime-jekyll](http://23maverick23.github.io/sublime-jekyll/)，具体功能参见其网站介绍。安装完插件后，最重要的一步是设置Jekyll项目。

这里需要注意，使用这个插件一定要引Sublime Text项目这个概念，也就是说你的Jekyll博客必须归属于一个Sublime Text项目。

关于Sublime Text项目，请参见：[https://www.sublimetext.com/docs/2/projects.html](https://www.sublimetext.com/docs/2/projects.html)。

打开项目设置文件，添加如下内容（根据实际情况替换其值）：

{% highlight yaml %}
{
    "folders":
    [
        {
            "follow_symlinks": true,
            "path": "/Users/gniu/Hackintosh/yekki.github.com/"
        }
    ],

    "settings":
    {
        "Jekyll":
        {
            "posts_path": "/Users/gniu/Hackintosh/yekki.github.com/_posts",
            "drafts_path": "/Users/gniu/Hackintosh/yekki.github.com/_drafts",
        }
    }
}
{% endhighlight %}

## 测试
打开你的博客项目，按：shift + cmd + p，输入：**jekyll**，就能看到相关命令了。
![sublime-jekyll](/upload/images/sublime_jekyll_screenshot.png)

**注：**由于我使用的模板是定制过的，所以，通过上述图形界面生成的博文总是不正确，所以我还是只能通过命令行创建新博文。

