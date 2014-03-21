---
layout: post
title: "Koding：免费VPS服务的初体验"
date: 2014-03-21 16:01:44 +0800
comments: true
categories: Koding VPS
---

![aritical 12](/images/artical/artical12.jpg)
<!-- more -->

*“文章原创，转载请注明出处”*

***

昨天晚上在浏览“[机器学习周刊](http://ztl2004.github.io/MachineLearningWeekly/)”时，看到了一个提供免费vps服务的网站，免费的虚拟主机平台哎。那么在上面码代码呀干点什么的，不知道效果如何呢？所有抱着尝试的心态就去看了一看。


### 一见惊喜

***

![a12_1](/images/a12/1.jpg)

看到这样的网页，顿时觉得~哇。。。高大上有木有。网页做的这么能吸引我的眼球，已经成功了一半了(对于勾引我的使用来说)。然后就开始注册吧，[注册地址](https://koding.com/R/jackycode)。使用我给的注册地址，你跟我都会额外获得1G的空间。输好邮箱跟用户名之后，点击`Sign up`。很快就会收到一封邮件，点击其中的链接，然后设置自己的密码即可(**注意，这个密码将会是你虚拟主机里终端的密码。**)。

### 外表

***

注册登录之后，大致是这个样子的，看起来还是蛮不错的！

![a12_2](/images/a12/2.jpg)

顶栏的buttons对应的功能分别是：Activity; Teamwork; Terminal; Ace(默认自带的编辑器); Apps(应用中心，现在应用不多，正在从旧版往新版里面转); DevTools(搞个Koding App？); Julia; Bracket。最后两个默认是没有的，是我自己安装的。

第一印象来说，界面比较干净清洁；功能划分也比较清楚。

### 玩一下

***

外表过关了，那么接下来就看看玩起来到底给不给力。话说，Koding官方的介绍文档里面说，支持的功能很多，挑几个试试呗！(在顶栏上面有个`?`，可以点击查看帮助)

点击进入终端，界面看起来还不错哦：

![a12_3](/images/a12/3.jpg)

尝试输入一下这些命令：

![a12_4](/images/a12/4.jpg)

好吧，自带么有R语言。没关系，这不是基于Ubuntu系统的嘛，我自己装一个还不行~

```
sudo cp /etc/apt/sources.list /etc/apt/sourcesbackup.list
sudo vim /etc/apt/sources.list
```

然后在sources.list里面添加`deb http://cran.stat.ucla.edu/bin/linux/ubuntu quantal/`(建议使用国外的镜像，国内的龟速啊！！！)。OK，保存退出vim。输入命令：

```
sudo apt-get update
sudo apt-get install r-base
sudo apt-get install r-base-dev
```

最后再看看：

![a12_5](/images/a12/5.jpg)

OK了嘛！总算是有了，以后跑点什么烦躁的程序就可以放这了，^_^

玩到现在，就发现这个命令行很疼啊，bash啊~能不能换成zsh啊~试了一下，失败鸟~~算了，这就不错了，我还能要求啥呢！先这么着吧。

### 其它
***
看了一下，Koding也是支持Octopress的，而且配置很简单，跟在自己机器上差不多。具体还有什么，就靠自己去慢慢摸索了。可以肯定的是，这个平台还是很不错的，值得推荐哦！
