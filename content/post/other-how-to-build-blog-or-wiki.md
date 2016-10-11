---
date: 2015-05-26
categories: 折腾记
title: 使用Jekyll来搭建blog
tags: 折腾
---
**update-2016-01-01: 本博客已改为hexo搭建**

### **写在前面**
这篇文章就当作我创建博客的一个简单的记录。

### **一、 github账号申请**

**1>** 进入[https://github.com](https://github.com)，没注册过的进入首页就是注册界面，填写注册信息并提交后，需进入自己注册邮箱，github会发送一份验证邮件到你的邮箱，只有通过邮箱验证才能正常的使用github上的服务。当然，github上也有付费服务，比如能够创建私人仓库，不过免费用户的所能使用的服务已经很齐全了。
	
<!-- more -->

**2>** 创建仓库(create new repository)，在用户小头像的右侧有一个符号“＋”，点击它就能进入创建仓库的页面，仓库命名为yourusername.github.io注意`yourusername`改为你自己的用户名，点击Create repository就创建了一个空的仓库

### **二、msysgit的安装与配置**

**1>** 进入[http://msysgit.github.io/](http://msysgit.github.io/)下载msysgit，安装过程一路next即可，安装成功后桌面多了一个git bash的图标，双击后进入bash窗口，之后许多操作都会在这个窗口进行。

**2>** 在C:\Git\etc\git-completion.bash文件结尾添加

```
alias ls='ls --show-control-chars --color=auto'
```

使得在Git Bash中输入ls命令，可以正常显示中文文件名。
新版本的msysgit中似乎只有这一个乱码问题，其他地方的乱码问题本人还没遇到，如果你遇到了可以参考别人分享的[git乱码解决方案汇总](https://gist.github.com/vkyii/1079783)

### **三、 jekyll的安装**

**1>** jekyll和ruby的关系密切，总之jekyll不能没ruby，因此还需要安装ruby环境

进入[http://rubyinstaller.org/downloads/](http://rubyinstaller.org/downloads/)根据自己的系统选择64-bit或32-bit的可执行文件，在rubyinstallers一栏下载ruby，在DEVELOPMENT KIT一栏下载devkit，比如`DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe`和`Ruby 2.2.2 (x64)`这两个文件，分别进行安装后将ruby安装目录中的bin添加到系统环境变量中比如我的`C:\Ruby200-x64\bin`,接着在命令行窗口输入ruby -v显示ruby版本号说明安装成功

**2>** 初始化ruby

```
ruby dk.rb init
```

**3>** 在Devkit安装目录下配置config.yml,在尾部添加ruby的安装目录如`C:\Ruby200-x64`

**4>** 安装development kit工具命令集

```
ruby dk.rb install
```

**5>** 在git bash中输入gem install jekyll进行安装，并用jekyll -v检查安装是否成功

### **四、 创建博客**

**1> 博客生成** 

在git-bash中输入

```
cd c:/myrepository
jekyll new myblog
```

此时myrepository目录下就多了一个目录myblog，其目录结构如下

```
/myblog 

    |--　index.html
 　 |--　_config.yml
 　 |--　_layouts
 　 |　　　|--　default.html 
　　　     |--  page.html
 　 |--　_posts
 　 |　　　|--　2015-01-01-hello-world.html
 　 |--  css
 　 |      |--  main.css
 　　　　  |--  syntax.css
    |--  _site  

```
用jekyll生成的是一个静态的博客网站，myblog下的就是整个博客的内容

**2> 动手写文章之前** 

到这一步其实可以开始写文章了，必须按`yyyy-MM-dd-post-name.md`或`yyyy-MM-dd-post-name.markdown`的格式新建文件，如`2015-01-01-hello-world.md`，而在开始写内容前，必须填写YAML头信息，这样该文件才能被Jekyll进行处理。头信息必须在文件的开始部分，并且需要按照 YAML 的格式写在两行三虚线之间。如下最基本的头信息

```
---
layout: post
title: Hello World
---
```

接下才是内容的输入，当然写之前最好掌握markdown的语法

**3> 本地访问博客** 

在gitbash中定位到博客主目录，输入

```
jekyll serve
```

则会在_site中生成一系列的网站的文件并且启动服务，这时在浏览器中访问`localhost:4000`就能看到自己的博客了

**4> 博客配置**

因为我们还没有对一些配置文件进行修改，所以访问的博客名字并不是自己的，比如修改`_config.yml`

```
name: DroidCat's blog     //换成你博客的名字
markdown: redcarpet       //设置markdown的渲染引擎
highlighter: pygments     //设置语法高亮的解析引擎
```

当然可以进行更多的设置，这要看个人需求
比如你可能觉得生成的blog页面太丑太单调了，你可修改css目录下的main.css，你觉得页面的布局也不符合你的要求，那你可以在_layouts下修改文件或创建新的布局文件，layout是在md文件头信息进行设置的

### **五、使用git**

可能你会觉得写的文章只能在本地看不过瘾，想要发布到网上让别人浏览，跟别人一起讨论，分享成果，广交良友，博客之所以称之为博客，道理也正是如此吧。那么git和github就不会让你失望了，我们可以将整个工作目录先提交本地的git仓库，再和远程仓库建立连接，然后推送到远程仓库中，远程仓库就是我们当开始建立的`yourname.github.io`，虽然github上的单个仓库容量限制是1GB，但事实上纯文字的博客1GB够写个好几年，需要图片的地方可以引用大型网站的图片链接，比如新浪。

**1> git的全局配置**

```
git config --global user.name  example
git config --global user.email example@example.com
```

**2> 继续在git-bash输入**

```
cd c:/myrepository/myblog
git init
```
完成仓库的初始化
这时在myblog目录下就生成了一个.git文件夹
继续输入

```
git add .
git commit -m "hello github"
```

完成一次提交，内容只是提交给了本地仓库，
此时还不能将内容推送到远程仓库，因为还没有添加与本地仓库关联的远程仓库，输入`git remote`没有任何输出说明还没有远程仓库与之关联
需要使用如下命令使本地仓库与`yourusername.github.io`这一仓库进行关联

```
git remote add origin https://github.com/yourusername/yourusername.git.github.io.git
```

完成关联后继续输入命令进行推送

```
git push origin master
```

将本地仓库的内容推送到远程仓库的master分支上，期间会提示输入用户名和密码，输入完成稍等片刻，在浏览器访问yourusername.github.io就能显示自己的博客网站了。

**3> 设置无密码ssh安全连接github**

生成ssh密钥

```
ssh-keygen -t rsa -C "youremail@email.com"
```

查看密钥

```
cat ~/.ssh/id_rsa.pub
```

复制上面得到的公钥内容，登陆github在settings的ssh keys中粘贴key，title随便填

验证key

```
ssh -T git@github.com
```

显示如下内容说明验证成功

    Hi droidroidcat! You've successfully authenticated, but GitHub does not provide shell access.


接着本地添加密钥

先打开ssh agent:

```
eval `ssh-agent -s`
```

继续运行下面的

```
ssh-add C:/Users/username/.ssh/id_rsa
```

成功添加后使用git push命令就不再需要输入用户名和密码这么繁琐，并且更加安全。

### **六、使用markdown**

按`四、 创建博客`的第2点创建以md或markdown为后缀的文件，接着书写内容前需先熟悉markdown语法，可参考[Markdown语法说明（简体中文版）](http://www.appinn.com/markdown/)

**1> 标题**

在文字前加符号#，如下效果
    
    # 一级标题
    ## 二级标题
    ### 三级标题
    #### 四级标题
    ##### 五级标题
    ###### 六级标题

![标题效果](http://ww3.sinaimg.cn/mw690/b5405c76gw1esh81en3r1j205g07yglw.jpg)

**2> 斜体和粗体**

使用 * 和 ** 表示斜体和粗体。
示例：
这是 *斜体*，这是 **粗体**。

**3> 列表**

在多行需要以列表进行呈现时，可在每行文字前加`-`或`*`表示无序列表，每行按数字顺序`1.` `2.` `3.`添加则为有序列表，注意点好与其后面需要间隔空格，效果如下

        - 第一行
        - 第二行
        - 第三行
- 第一行
- 第二行
- 第三行

再如

    1. 第一行
    2. 第二行
    3. 第三行
1. 第一行
2. 第二行
3. 第三行

**4> 引用**

文字前加`>`这一符号

    >这是一条引用
>这是一条引用

再如

    >这是一段引用
    >这是一段引用
    >这是一段引用
    >这是一段引用
    >这是一段引用
>这是一段引用
>这是一段引用
>这是一段引用
>这是一段引用
>这是一段引用

**5> 图片与链接**

插入链接与插入图片的语法相似，区别在于一个`!`号
图片为：`![这是图片](http://ww3.sinaimg.cn/mw600/90eef340gw1f8kyq5puyej20jg0nsjsi.jpg)`添加后直接显示图片
效果如下：

![](http://ww3.sinaimg.cn/mw600/90eef340gw1f8kyq5puyej20jg0nsjsi.jpg)

链接为：`[这是链接](http://baidu.com)`
[这是链接](http://baidu.com)

**6> 行内代码块**

使用 \`代码` 表示行内代码块。

示例：
让我们来好好聊聊 `人生`。

**7> 代码块**

使用四个缩进空格表示代码块，需要保证上一行为空行
示例：

    明天有空吗

更多的markdown语法需要查找相关的资料，今天就先写到这里吧
                                                   




