---
layout: post
title: "Mac 系统重装"
date: 2014-04-30 20:57:58 +0800
comments: true
categories: Records Mac
---

![artical 28](/images/artical/artical28.jpg)
<!-- more -->

*“文章原创，转载请注明出处”*

***

话说Mac一般来讲不需要什么重装啊，不过世事无绝对啊，有时候人就是喜欢作死！！！比如前几天的我~~~算了，具体情况就不说了，反正就是作死，Mac系统出了问题。本来想着用Time Machine恢复一下就得了，也省事。不过仔细想了一想，很想自己安装一遍（博主的折腾精神有时候很是让自己“佩服”），然后就开始了Mac系统重装之旅！

### 一、制作USB安装盘
***

要安装首先就得制作一个启动U盘，当然你也可以使用Mac的Internet Recovery，不过我试了一下，反正我是连不上！不过即使连上了，那个下载应该也需要超久的时间，不是我可以等得了的。我之前更新Mac时，已经下载好了Mavericks（OS X 10.9），所以直接拿来用了。

#### 具体操作如下：

1. 将OS X 10.9安装文件放到桌面上，右键选择“显示包内容”；
2. 复制"Contents/Resources/createinstallmedia"到桌面；
3. 打开终端，执行命令：`sudo -s`，切换到root（成功的话，应该会显示`bash-3.2#`）；
4. 将U盘（8G以上）接入Mac，用磁盘工具进行格式化，格式选择**"Mac OS扩展（日志式）"**，名称定为**“OSX”**；
5. 在终端中执行下面的命令：（将其中的“username”换成你自己的用户名）
    
    ```
    /Users/username/Desktop/createinstallmedia --volume /Volumes/OSX --
    ```

    ```
    applicationpath /Users/username/Desktop/"Install OS X Mavericks.app"
    ```

6. 等待终端完成。因为需要将安装的文件全部拷贝到U盘中，需要的时间可能有些久，不用着急，喝杯咖啡看看网页！完成后的终端会显示“Copy complete.Done.”

根据上面的流程，制作的安装盘是完整的，也就是安装完成之后，Mac是有Recovery HD的（就是开机按住option可以看到得“恢复10.9”），也可以使用FileVault和find my Mac功能。网上一些直接利用磁盘工具制作的安装盘，应该是不具备这个能力的，所以不建议使用那些方法。


### 二、加密与备份
***

#### 安装之前：

在重装Mac系统之前，需要对Mac里面的文件进行一个备份。也建议使用Dropbox这样的网盘，将自己的一部分文件同步在网盘上，这样就不会存在丢失的情况。备份好之后，就可以开始重新安装Mac系统了。

#### 安装之后：

1. 在安装好了Mac系统之后，我首先进行了一些基本的设置：触控板、输入源、iCloud账号设置等等；
2. 在完成这些基础设置之后，你可以选择打开FileVault加密，增强电脑的安全系数（不过打开后，开机速度可能会变慢一些）；
3. 强烈建议打开Time Machine，给电脑做个备份；

### 三、安装Command Line Tools
***

为什么需要将这个独立出来？哎，没办法啊。。。在Mac上很多事情都靠他，离了它，估计我就没法好好生活好好学习好好过日子了！！！（夸张夸张！）

这个的安装，我是直接下载了XCode和Command Line Tools之后安装的。用App Store下载Xcode，那个速度我实在是等不了。

### 四、安装Java
***

由于平时需要使用Weka，当然还有那个什么，那个什么~~~反正很多啦，都是需要Java支持的，像Matlab！嘿嘿。。。所以去安装一个Java吧，虽然我很不喜欢它！

### 五、配置shell
***

Mac默认使用的时bash，表示不喜欢。在对电脑进行各种软件安装配置之前，必须把Terminal搞成我喜欢的样子，嘿嘿~~

1. 切换shell到`zsh`：`chsh -s /bin/zsh`；
2. 首先安装`Homebrew`，执行下面的命令即可：

    ```
    ruby <(curl -fsSkL raw.github.com/mxcl/homebrew/go)
    ```

3. 安装`wget`：`brew install wget`
4. 安装`oh-my-zsh`：`wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh`
5. 修改配置文件`.zshrc`；
6. 修改主题文件，主题文件的目录为`~/.oh-my-zsh/themes`，找到自己使用的主题，进行修改即可。

### 六、R语言相关
***

每天都在使用R语言，离了这个可真是活不了！下载好了R 和 RStudio之后，直接安装就好了。不过，这边可能会出现一个问题，就是在Mac上可能会出现encoding之类的问题，这个时候就需要设置一下，打开终端运行以下的命令即可：

```
defaults write org.R-project.R force.LANG en_US.UTF-8
```

安装好之后，将自己常用的包下载一下就OK了！

### 七、Python
***

除了R语言，应该算是这货用的最多，所以安装好了R之后，就开始来弄它了。

Mac是自带Python的，10.9自带的版本是Python 2.7.5，我一般使用的是Python 2.7.6，所以首先需要更新一个Python。以前我使用的软件包管理系统是Macports，不过现在已经叛逃到了Homebrew了！

1. 执行`brew install python`就可以下载安装最新版的Python了。不过安装好了之后，还是用不了的。因为Mac还是会用自带的那个Python。这个我一般就是将Homebrew的软件包目录加入PATH中，并且将该软件包目录的位置放置于其它目录的上方。

    可以使用`sudo vi etc/paths`打开系统的PATH，然后在里面的第一行添加Homebrew的软件包安装目录`\usr\local\bin`，第二行添加为`\usr\local\sbin`，其实只要再`\usr\bin`的上方就行了。

2. 安装好这些之后，可以使用Python自带的`easy_install`安装`pip`，即：`easy_install pip`；
3. 使用`pip`安装需要的python库：`pip install numpy`等等。我一般安装的是库有：`numpy, scipy, matplotlib, ipython, scikit-learn`等。

### 八、Sublime Text & TextMate
***

我一般使用的文本编辑器就是上面两个，ST3常用，TM用的稍微少一些。配置的时候，ST3稍微麻烦一些，TM则简单地多，只要点点点就可以了。

#### Sublime Text 3

1. 安装好ST之后先安装Package Control，打开view -> show console，在console中输入代码。可以到[这里](https://sublime.wbond.net/installation)去查看安装的最新代码(区分ST2以及ST3)；

2. 配置安装主题Flatland（我的最爱），使用`shift + cmd + P`打开Package Control，输入`install package`，return之后等待一下。在弹出的窗口中输入`Flatland`，安装即可。安装完成后，打开Prefereces -> Settings - User，添加配置：

    ```
    "color_scheme": "Packages/Theme - Flatland/Flatland Monokai.tmTheme",
    "theme": "Flatland Dark.sublime-theme"
    ```

    当然还可以对这个主题进行其它配置，可以自行Google；

3. 修改字体，还是在Setting-User中，添加：

    ```
    "font_face": "menlo",
    "font_size": 13
    ```

4. 安装一些常用的包：`ConvertToUTF8`, `Enhanced-R`, `SublimeLinter`, `SublimeREPL`, `OmniMarkupPreviewer`, `Markdown Extended`, `Jedi - Python autocompeltion`, `Alignment`, `BracketHighlighter`, `SendText`, `SideBarEnhancements`, `TrailingSpaces`等等。
5. 对有些需要配置的包配置一下，其实我也就配置了跟R语言有关的包，以及R语言在ST的快捷键。

#### TextMate

这个配置起来比较容易，只需要在Preferences -> Bundles下面选择需要安装的包就可以了。我安装了一些我常用的包，然后将主题更换成了`Made of Code`，将`show command output`修改成了`Right of text view`。

### 九、安装其它的软件
***

完成上面的安装，基本上就能用了，但是我需要在R中使用Knitr和Sweave，所以我得安装MacTex，顺便还装了Lyx。

安装好了MacTex之后，我就安装了其它一些平时会用的软件，像Octave、Weka、MySQL等等！

剩下的就是常用软件了，什么Dropbox、Evernote、iWork等等！！！iWork那个下载速度很是蛋疼啊~~~

### 总结
***

因为是隔了好几天才动笔写这个记录的，所以应该记录之中不免会有些遗漏！当然，中间或多或少肯定也存在一些问题，欢迎指正！
