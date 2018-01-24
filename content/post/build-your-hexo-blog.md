+++
title = "Hexo博客的搭建"
date = "2016-01-02"
draft = true
categories = ["折腾"]
+++
### 一、前提
需要有Github账号，没有就到[Github.com](https://github.com)进行申请

### 二、开始
创建一个仓库并命名为`username.github.io`，本人的就是`leibnik.github.io`，既是仓库的名称同时也是博客的网址，我们需要做的就是往这个仓库加入html文件，之所以使用Hexo来搭建博客，是因为方便，从MarkDown解析为html再将html文件部署到github，一气呵成。

<!-- more -->

### 三、安装Hexo
Hexo安装前又有两个前提，需要先确定已经安装了下面两个应用程序：

* [Node.js](http://nodejs.org/)
* [Git](https://git-scm.com/)

确认前面的步骤无误后，安装Hexo，只需一行代码

```shell
$ npm install -g hexo-cli
```
### 四、正式搭建
进入指定的路径，执行以下命令，`<folder>`为存放整个博客的目录
```
$ hexo init <folder>
$ cd <folder>
$ npm install
```
完成后，其实我们已经可以在本地先预览我们的网站，执行
```
$ hexo server
```
提示成功后，用浏览器访问地址`http://localhost:4000/`，就能看到我们的静态网站。

### 五、简单介绍
回到我们的博客目录`<folder>`，其目录结构如下：
```
.
├── public               \\ hexo生成的静态网页
├── _config.yml          \\ 网站的配置信息
├── package.json         \\ 应用程序的信息
├── scaffolds		 \\ 存放模板的文件夹，新建文章时用到
├── source               \\ 资源文件夹
|   ├── _drafts
|   └── _posts			 \\ markdown文件
└── themes               \\ 主题文件夹，hexo根据主题来生成html，默认主题landscape
```
接着我们打开`_config.yml`文件瞅一瞅。可以看到有许多类似`key: value`这样的配置信息，都是有着特定的意义的，如下图

|参数|描述|
|---|---|
|title|网站标题|
|subtitle|网站副标题|
|description|网站描述|
|author|作者名称|
|language|网站使用的语言|
|timezone|时区，默认为当前系统的时区|

这里我只介绍一小部分，更多的参考官网：[hexo配置](https://hexo.io/zh-cn/docs/configuration.html)

接着再看下常用的命令，这里给个表格，完整的可以参考[hexo指令](https://hexo.io/zh-cn/docs/commands.html)

指令|用法|含义
---|---|---
init|`hexo init [folder]`|新建一个网站。如果没有设置 folder ，Hexo 默认在目前的文件夹建立网站。
new|`hexo new [layout] <title>`|新建名为`<title>`的文章。
generate|`hexo generate`|生成静态文件，-d或者--deploy：生成后立即部署网站，-w或--watch：监视文件变动
server|`hexo server`|启动本地服务器，默认地址`http://localhost:4000/`
deploy|`hexo deploy`|把网站内容（public里的文件）部署到远程仓库
clean|`hexo clean`|清除缓存文件 (db.json) 和已生成的静态文件 (public)

### 六、开始写作
利用命令来创建文章
```
$ hexo new [layout] <title>
如：
$ hexo new post "Hello world"
```
或者创建后缀名为markdown或md的文件存放到`<folder>/source/_posts`下。

#### 布局
Hexo默认布局有三种分别为：`post`、`page`和`draft`，使用指令创建文章时可以指定布局，不指定则默认为layout为`post`

布局|路径|含义
---|---|---
post|`source/_posts`|存放md文件
page|`source`|分类页则生成categories文件夹，标签页同理
draft|`source/_drafts`|存放草稿

草稿不在网站页面上显示，可通过`hexo publish`转移到`_post`

#### 模板
执行hexo新建文章的命令时，可以指定模板，让hexo根据`scaffolds`里的模板生成文件
```
$hexo new photo "My Gallery"
```
如果hexo在`scaffolds`里找到`photo.md`时就按照该文件来创建新文件，注意模板文件不要与布局同名。

#### Front-matter
在你的`_posts`文件夹下打开初次创建网站默认生成的md文件，就能看到文件开头类似以下的写法：
```
title: Hello World
date: 2013/7/13 20:46:25
---
```
`---`上方就是Front-matter，同样是`key: value`的形式，以下就是预先定义的参数，都有着特定的含义

参数|描述|默认值
---|---|---
layout|布局|--
title|标题|--
date|建立日期|不指定时默认为文件创建日期
updated|更新日期|不指定时默认为文件更新日期
comments|开启文章的评论功能|true
tags|标签|--
categories|分类|--
permalink|覆盖文章网址|--


#### 写作必备的MarkDown语法

书写内容前需先熟悉markdown语法，可参考[Markdown语法说明（简体中文版）](http://www.appinn.com/markdown/)

##### 标题

在文字前加符号#，如下效果
    
    # 一级标题
    ## 二级标题
    ### 三级标题
    #### 四级标题
    ##### 五级标题
    ###### 六级标题

![标题效果](http://ww3.sinaimg.cn/mw690/b5405c76gw1esh81en3r1j205g07yglw.jpg)

##### 斜体和粗体

使用 * 和 ** 表示斜体和粗体。用法：\*斜体\*，\*\*粗体\*\*。
示例：
这是 *斜体*，这是 **粗体**。

##### 列表

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

##### 引用

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

##### 图片与链接

插入链接与插入图片的语法相似，区别在于一个`!`号
图片为：`![这是图片](http://ww2.sinaimg.cn/mw690/b5405c76gw1f33sfvycawj205e05e0sx.jpg)`添加后直接显示图片

效果如下：
![这是图片](http://ww2.sinaimg.cn/mw690/b5405c76gw1f33sfvycawj205e05e0sx.jpg)

链接为：`[这是链接](http://baidu.com)`
[这是链接](http://baidu.com)

##### 行内代码块

使用 \`代码` 表示行内代码块。

示例：
在这光滑的地上 ，`摩擦`。

##### 代码块

使用四个缩进空格表示代码块，需要保证上一行为空行
示例：

    左手右手慢动作

代码块还可以这样写


> \`\`\`  
> 右手左手慢动作重播  
> \`\`\`


注意：**\`**为反单引号，在键盘中数字键“1”的左边

效果如下
```
右手左手慢动作重播
```

##### 代码高亮
针对特定语言产生的高亮效果，比如java，将代码置于 **\`\`\`java** 和 **\`\`\`** 之间

> \`\`\`java  
> public class MainActivity extends AppCompatActivity {  
>     @Override   
>     protected void onCreate(Bundle savedInstanceState) {   
>         super.onCreate(savedInstanceState);   
>         setContentView(R.layout.activity_main);   
>     }   
> }   
> \`\`\`

效果如下：

```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
}
```

再如xml文件，将代码置于 **\`\`\`xml** 和 **\`\`\`** 之间

效果如下：

```xml
<activity android:name=".MainActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN"/>
        <category android:name="android.intent.category.LAUNCHER"/>
    </intent-filter>
</activity>
```

##### 表格
前面好几次用到了表格，所以这里再介绍一下表格的语法好了。写法如下：

	书名|价格|页数                    // 表头
	---|---|---                       // 分割表头与记录
	java编程思想|108元|880            // 第一条记录
	android开发艺术探索|79元|507      // 第二条记录

书名|价格|页数
---|---|---
java编程思想|108元|880
android开发艺术探索|79元|507

更多的markdown语法请自行查找相关资料。

### 七、发布
修改网站配置文件`_config.yml`，添加仓库地址

```
deploy:
  type: git
  repo: https://github.com/leibnik/leibnik.github.io.git  // 仓库地址
  branch: master
```

在网站根目录下执行如下命令

```shell
$ hexo generate	// 生成静态网页
$ hexo deploy 	// 部署到远程仓库
```

执行`hexo deploy`后需要输入账号和密码才能完成部署，如果嫌麻烦可以设置无密码ssh安全连接到github，下面就是步骤，如果不嫌麻烦，那么就此跳过吧 : )

先配置git
```
$ git config --global user.name  example
$ git config --global user.email example@example.com
```
接着生成ssh密钥
```shell
$ ssh-keygen -t rsa -C "youremail@email.com"
```
查看密钥
```shell
$ cat ~/.ssh/id_rsa.pub
```
复制上面得到的公钥内容，登陆github在settings的ssh keys中粘贴key，title随便填

验证key
```shell
$ ssh -T git@github.com
```
显示如下内容说明验证成功

    Hi droidroidcat! You've successfully authenticated, but GitHub does not provide shell access.


接着本地添加密钥

先打开ssh agent:
```shell
$ eval `ssh-agent -s`
```
继续运行下面的
```shell
$ ssh-add C:/Users/username/.ssh/id_rsa
```
到了这里可能还没生效，有可能remote为https形式，要使用ssh就需要ssh形式的remote地址。
如本人博客项目的：
```
https://github.com/leibnik/leibnik.github.io.git    // https
git@github.com:leibnik/leibnik.github.io.git        // ssh
```
修改网站配置文件`_config.yml`

```
deploy:
  type: git
  repo: git@github.com:leibnik/leibnik.github.io.git  // repo为ssh地址
  branch: master
```

成功添加后，每次部署到远程仓库就不再需要输入用户名和密码这么繁琐，并且更加安全。

### 八、修改主题
不打算修改主题可以跳过啦 : )

若需要修改主题，可以到[官网主题市场](https://hexo.io/themes/)挑选一款自己喜欢的主题。

以这款主题为例：[chan](https://github.com/denjones/hexo-theme-chan)

```
git clone https://github.com/denjones/hexo-theme-chan.git themes/chan
```
将主题克隆到themes文件夹下，接着修改网站配置文件`_config.yml`
```
theme: chan
```
theme变量的值就是themes文件夹下主题名称，主题文件夹下有同样名为`_config.yml`的主题配置文件，其配置一般根据不同的主题而定，需要参考主题作者的介绍进行配置，这部分不再深入

### 九、域名绑定
不打算绑定域名的可以跳过 : ) 网站部署到github后，就能以`username.github.io`的形式访问自己的博客啦。

创建`CNAME`文件填写自己的域名，比如本人的
```
droidroid.com
```
放在`source`文件夹下。然后到DNS服务商控制台，CNAME文件里的域名添加解析，记录类型为A，记录值为`192.30.252.153`或者
`192.30.252.154`，可以参考github官网介绍[Configuring A records with your DNS provider](https://help.github.com/articles/setting-up-an-apex-domain/#configuring-a-records-with-your-dns-provider)

然后将网站部署到github，稍等几分钟就能生效啦。你可以通过`droidroid.com`或者`leibnik.github.io`来访问我的博客。

### 十、最后
以上就是搭建Hexo静态博客简单的流程。

朋友，一起开启我们的博客之旅吧！（尬之结语）




