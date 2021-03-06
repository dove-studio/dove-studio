---
title: 'Hexo+Github,搭建属于自己的博客'
categories:
  - Web
tags:
  - Hexo
  - Github
  - Blog
abbrlink: 7448c947
date: 2019-05-11 16:21:44
mp3:
cover:
---

### 背景
写blog虽然经历了N多不同时代的产品，恒久不变的始终是自己无人问津的网站。虽然没几个人看，还是隔断时间就要折腾一下。从最开始的wordpress，到tale，到现在的hexo，网站变得越来越简单，越来越轻量级，这里主要说说hexo的使用。
hexo介绍
主页： https://hexo.io/zh-cn/
主页中有非常详细的介绍，主页中有介绍内容我这里就不赘述了，这里主要说说主页中没有详细说明内容。

hexo 可以理解为是基于node.js制作的一个博客工具，不是我们理解的一个开源的博客系统。其中的差别，有点意思。
hexo 正常来说，不需要部署到我们的服务器上，我们的服务器上保存的，其实是基于在hexo通过markdown编写的文章，然后hexo帮我们生成静态的html页面，然后，将生成的html上传到我们的服务器。简而言之：hexo是个静态页面生成、上传的工具。

下面安装及使用的系统环境为Ubuntu。

### 安装
1 安装nodejs
``` bash
sudo apt-get install nodejs
```
2 安装git
``` bash
sudo apt-get install git
```
3 安装npm
``` bash
sudo apt-get install npm
```
4 使用npm安装Hexo 
``` bash
sudo npm install -g hexo-cli
```

5 下面开始生成blog
``` bash
hexo init blog
cd blog
npm install
hexo generate
hexo server
```
然后可以打开浏览器输入localhost:4000查看本地网站

源码结构
* _config.yml 配置文件
* public      生成的静态文件，这个目录最终会发布到服务器
* scaffolds   一些通用的markdown模板
* source      编写的markdown文件，_drafts草稿文件，_posts发布的文章
* themes      博客的模板

我们正常使用，修改最多的源码是_config.yml文件，不管是博客的基础配置，还是模板，都是修改这个文件。
source是我们日常写文章要用的目录，是我们日常操作的文件夹。
如果针对下载的模板修改，那么就需要操作themes了。hexo是用node.js编写的程序，所以theme的修改也是比较容易的。

### 使用
#### 编写BLOG
官方新编写BLOG，建议的是使用命令：
``` bash
hexo new [layout] "title"
```
其实，你可以直接在_drafts新建markdown文件，就是草稿了，在_posts新建markdown文件，就是发布的文章了。当然，记得在顶部加上相关的描述，效果和命令新建文件，是一样一样的。

#### 模板安装和使用
hexo为我们提供了非常多好看实用的模板，官网提供了很多模板，但是，针对模板的使用，只进行了非常简单的说明，这里以我自己的使用的模板diaspora为例，进行详细的补充。

其实，模板的使用，简单来说，只需要两个步骤：

1 将模板copy到themes目录，如：
``` bash
cd themes
git clone https://github.com/Fechin/hexo-theme-diaspora.git diaspora 
```
2 修改_config.yml中的theme：
```
theme: diaspora
```
所有的模板，共有的操作方式就只有这两个，但是不同的模板，会有很多不同的设置，这里就需要针对模板的使用说明进行修改了。建议通过github找到模板，看下readme。

必要的时候需要更新主题
注意：请在更时主题时备份`_config.yml`配置文件

``` bash
cd themes/diaspora
git pull
```
#### 新建文章模板

``` markdown
title: {{ title }}
date: {{ date }}
categories: 
    - categorie
tags: 
    - tag1
    - tag2
mp3: 
    - http://xxx.xxx.xxx/xxx.mp3
    - http://xxx.xxx.xxx/xxx.mp3
cover: /img/cover.jpg
```

#### 创建分类页
1 新建一个页面，命名为 Categories 。命令如下：
``` bash
hexo new page Categories
```
2 编辑刚新建的页面，将页面的类型设置为 categories
``` markdown
title: Categories
date: 2019-05-11 16:44:48
type: "categories"
```
主题将自动为这个页面显示所有分类。

#### 创建标签页
1 新建一个页面，命名为 Tags 。命令如下：
``` bash
hexo new page Tags
```
2 编辑刚新建的页面，将页面的类型设置为 tags
``` markdown
title: Tags
date: 2019-05-11 16:45:50
type: "tags"
```
主题将自动为这个页面显示所有标签。

#### 主题配置
``` yml
# 头部菜单，title: link
menu:
  Categories: /Categories
  Tags: /Tags
  Github: https://github.com/dove-studio
```

### 发布
现在比较流行的方式，都是将代码发布到git，因为我同时要传到github和自己的博客，所有我的处理方式就是：先将代码传到github，再在自己的服务器获取github代码，服务器上自己部署一个nginx，解析静态网页即可。
1 首先注册github账号，比如dove-studio
https://github.com/

2 然后创建一个新项目：
固定格式：dove-studio.github.io

3 点击设置，创建一个page，选择一个主题，不过没什么用，反正马上就被覆盖掉了，这个时候访问一下链接，应该可以看到效果

4 安装git插件：
``` bash
sudo npm install hexo-deployer-git --save
```

5 git的配置，修改_config.yml文件，使用SSH（非HTTPS）:
``` yml
deploy:
  type: git
  repo: git@github.com:dove-studio/dove-studio.github.io.git
  branch: master
```

6 配置本地的ssh github私钥
* 本地生成公钥私钥
``` bash
ssh-keygen -t rsa -C "dove-studio@qq.com"
```
* 添加公钥到 Github
根据上一步的提示，找到公钥文件（默认为id_rsa.pub），用记事本打开，全选并复制。
登录 Github，右上角 头像 -> Settings —> SSH keys —> Add SSH key。把公钥粘贴到key中，填好title并点击 Add key。
* 配置完成后输入命令进行测试
``` bash
ssh -T git@github.com
```
第一次还需要输入yes确认，等待片刻可看到成功提示。

7 设置您账号的缺省身份标识
``` bash
  git config --global user.email "dove-studio@qq.com"
  git config --global user.name "dove-studio"
```
如果仅在本仓库设置身份标识，则省略 --global 参数。
如果存在多个用户，需要设置配置文件
``` bash
gedit ~/.ssh/config
```
输入以下内容
```
Host github.com
HostName github.com
User git
IdentityFile ~/.ssh/id_dove
```

8 将本地的文章通过hexo官方推荐的方式push到github：
``` bash
hexo deploy
```
这样应该就能在你的github上看到上传的代码了，这时看到的应该是纯静态的一个站点。
既然代码已经上传了，那么只要在我的服务器配置好github和ssh私钥，就能顺利的pull代码了，当然，还需要写个crontab定时来拉取，不需要自己登录服务器操作了。

### 常见问题
#### 执行deploy命令了，但是代码未上传？
先执行`hexo generate`命令，生成静态文件了，再执行`hexo deploy`上传，上传是只会上传public中生成的文件。

#### 修改了模板，但是没有生效？
修改了模板以后不生效，建议先`hexo clean`，然后再`hexo generate`。只执行hexo generate，可能模板后者静态文件不会被替换。

#### 预览不太方便？
开发环境提供了实时预览的方式，不需要每次都generate，执行命令：
``` bash
hexo generate -w
```
系统会实时生成新的HTML，非常方便。
