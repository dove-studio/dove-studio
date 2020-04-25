---
abbrlink: ca4af638
---
# 注册GitHub账号

# 创建GitHub仓库

创建git仓库时候，仓库的名称有格式要求，例如我的GitHub仓库用户名是dove-studio，那么我创建的仓库名称就是[dove-studio.github.io](http://dove-studio.github.io)。 

# 下载node.js

[node.js](https://nodejs.org/en/download/) 主要用于安装hexo，npm开头的命令都依赖于Node.js 
如果出现以下提示,则代表你的Node.js没有安装或者还未生效，如果已经安装了则重启电脑. 
bash: nmp: command not found

# 安装git

Windows版本的git下载地址为[https://git-for-windows.github.io/](https://git-for-windows.github.io/) 
一步一步默认安装好了

Git for Windows. 国内直接从官网下载比较困难，需要翻墙。这里提供一个国内的下载站，方便网友下载
[Git for Windows](https://npm.taobao.org/mirrors/git-for-windows/)

# 安装hexo

[官方中文文档](https://hexo.io/zh-cn/docs/index.html)

让我们先了解了解 的命令解释 
$ hexo g #完整命令为hexo generate，用于生成静态文件 
$ hexo s#完整命令为hexo server，用于启动服务器，主要用来本地预览 
$ hexo d #完整命令为hexo deploy，用于将本地文件发布到github上 
$ hexo n #完整命令为hexo new，用于新建一篇文章

npm全称Node Package Manager，是node.js的模块依赖管理工具。由于npm的源在国外，所以国内用户使用起来各种不方便。下面整理出了一部分国内优秀的npm
镜像资源，国内用户可以选择使用。

淘宝npm镜像
搜索地址：http://npm.taobao.org/
registry地址：http://registry.npm.taobao.org/

cnpmjs镜像
搜索地址：http://cnpmjs.org/
registry地址：http://r.cnpmjs.org/

有很多方法来配置npm的registry地址，下面根据不同情境列出几种比较常用的方法。以淘宝npm镜像举例：
1.临时使用
npm --registry https://registry.npm.taobao.org install express
2.持久使用
npm config set registry https://registry.npm.taobao.org
// 配置后可通过下面方式来验证是否成功
npm config get registry
// 或npm info express
3.通过cnpm使用
npm install -g cnpm --registry=https://registry.npm.taobao.org
// 使用cnpm install expresstall express


（1）鼠标右键任意地方，选择Git Bash，使用以下命令安装hexo

npm  install  hexo-cli -g
npm  install  hexo-deployer-git  --save
1
2
提示：如果后面出现“ERROR Deployer not found: git“这样的提示错误，则执行此步骤第二个命令 
（2）创建放置博客文件的文件夹：hexo文件夹，我放置在E盘，E:\hexo,最好不要在中文路径下面，会出现莫名其妙的错误，我们都懂的。 
注意：以后进行hexo操作都要进入到此文件夹中 
（3）进入E:\hexo文件夹，鼠标右键选择“Git Bash”,执行以下命令，初始化hexo，这时候会在该文件夹中创建网站所需要的文件

hexo init
1
（4）安装依赖包

npm install
1
会在D:\Hexo目录中安装 node_modules 
（5）生成静态文件

hexo g
1
（6）此时最基本的网站已经搭建好了，只不过是在本地放着，我们可以先预览一下 
在git中执行

hexo s
1
这时候，会在本地开启一个http服务，监听4000端口，我们在浏览器访问http://127.0.0.1:4000

六、部署本地文件到github
本地文件要发布到github上面，则需要使本地git和互联网上的通信。怎么通信呢？ 
有两种方法，https和ssh公钥方式。使用https的话需要每次输入密码，不过配置起来简单。常用的是ssh公钥的方式。 
先来在本地初始化一下git设置吧

本机git初始设置
1、设置姓名和邮箱地址
终端输入：任意目录中都可以

$ git config  --global  user.name  "your name"  
$ git config  --global  user.email  your_email@youremail.com
1
2
我用的是我的用户名和邮箱设置的，设置完成会在~/.gitconfig文件中生成如下配置 


2、本地生成ssh key
ssh-keygen -t rsa -C 13253641509@qq.com
1
和在linux生成的ssh一样，也可以生成的秘钥时候设置私钥密码

3、把本机生成的公开的秘钥添加到github中
cat ~/.ssh/id_rsa.pub
1
将公钥复制下来

4、将公钥放在github上。
打开github登陆后，点击自己头像下箭头，找到settings（设置），在左边栏目里面选择SSH keys,然后点击Add SSH key，随便填一个Title，把上面把cat出来的内容全部添加到key文本框里，然后点击add key。 


添加成功后，创建github时用到的邮箱会收到GitHub发的一个”A new public key was add to your account”的邮件。

5、使用私人密钥与GitHub进行认证和通信
ssh -T git@github.com
1


将hexo和github进行关联
1、编辑E：\hexo下的_config.yml文件，在_config.yml最下方，添加如下配置

$ cd e:/hexo
$ vi _config.yml

deploy:
  type: git
  repository: git@github.com:thinkerwalker/thinkerwalker.github.io.git
  branch: master
1
2
3
4
5
6
7
8
9
说明： 
(1)hexo的配置文件中任何’:’后面都是带一个空格的。 
(2)repository: 后面的地址其实在github上有生成。新建仓库时候，会有生成如下图 

(3)这里发现还有HTTPS，其实HTTPS也可以和本地进行通信，只不过通信时候，每次要求输入GitHub账号密码。

2、将本地文件同步到GitHub
$ hexo g
$ hexo d
1
2
此时，我们的博客已经搭建起来，并发布到Github上了，这时可以登陆自己的Github查看代码是否已经推送到对应Repository，在浏览器访问http://thinkerwalker.github.io就能看到自己的博客了。第一次访问地址，可能访问不了，您可以在几分钟后进行访问，一般不超过10分钟。

七、发表一篇文章
1、在Git Bash执行命令： 
$ hexo new “firstblog” 
2、这时候会在E:\hexo\source_post文件夹中，生成一个firstblog.md文件，这时候，我们可以在里面编辑内容。

3、可以先在本地预览一下生成的博客 
hexo g 
hexo s 
4、访问http://127.0.0.1:4000 
5、同步到GitHub上 
hexo d 
6、访问自己的网址即可查看，https://thinkerwalker.github.io

注意：首页中，默认是把整篇文章显示出来，怎么在首页显示文章的缩略呢？ 
在想要显示缩略文章的内容中，缩略内容的后面添加如下命令 
<!--more--> 
这样这段代码前面部分，将以缩略形式显示到首页，后面的内容将不会显示。

八、绑定自己的域名.
1、在source文件中新建一个CNAME文件，然后里面添加自己的网站域名，如walker123.cn 
可以在setting中查看是否发布成功，默认发布到http://walker123.cn上面了，可以在下面添加自定义主机名。 
 
2、到自己的域名解析中添加CNAME值，主机记录www,解析到username.github.io.(注意最后面有点，我用的腾讯云，如果没有加点，保存时候自动加上了点)，为了让只输入一个二级域名也可以访问到，加上自己的域名记录输入@或者walker123.cn.(cn后面有点)，如下图 

3、使用hexo g，hexo d进行部署 
4、等十分钟左右，用自己域名访问下试试

九、主题安装
在 Hexo 中有两份主要的配置文件，其名称都是 _config.yml。 其中，一份位于站点根目录下，主要包含 Hexo 本身的配置，称为站点配置文件；另一份位于主题目录下，这份配置由主题作者提供，主要用于配置主题相关的选项，称为主题配置文件。

hexo3.0的默认配置主题是landscape

1、下载hexo主题
hexo的官方主题可以到这里https://hexo.io/themes/下载。

发现这里面的主题都还挺漂亮https://www.zhihu.com/question/24422335 
http://www.jianshu.com/p/b6c694c2e58e

此处下载著名的NexT 版本，版本说明https://github.com/iissnan/hexo-theme-next/releases 
下载地址是https://github.com/iissnan/hexo-theme-next/archive/v5.1.1.zip，这里是最新版本。

2、安装主题
1、只需要将主题文件拷贝至站点目录的 themes 目录下，这里是主题存放的文件夹。 然后修改下配置文件即可。下载完成后解压放入E:\hexo\themes文件夹中 
2、修改站点配置文件 
vi e:/hexo/_config.yml 
搜索theme，将后面的修改为想要的theme， 
此处设置为：theme: hexo-theme-next-5.1.1 注意分号后面有空格 
3、hexo clean（切换主题之后、验证之前， 我们最好使用 hexo clean 来清除 Hexo 的缓存） 
4、hexo s本机预览一下配置的主题，若要开启调试模式，则加上–debug 
http://127.0.0.1:4000 
看到类似下面的即证明安装成功，后面将要更改一些主题的设定，包括个性化以及集成第三方服务 
 
5、将代码上传到GitHub 
hexo d

十、基于next主题的配置
主题说明文档http://theme-next.iissnan.com/getting-started.html

选择 Scheme
Scheme 是 NexT 提供的一种特性，借助于 Scheme，NexT 为你提供多种不同的外观。同时，几乎所有的配置都可以 在 Scheme 之间共用。

Scheme 的切换通过更改 主题配置文件，搜索 scheme 关键字。 你会看到有三行 scheme 的配置，将你需用启用的 scheme 前面注释 # 去除即可,从这三个中选择一个去掉#注释 
目前 NexT 支持三种 Scheme他们是： 
(1)Muse - 默认 Scheme，这是 NexT 最初的版本，黑白主调，大量留白 
 
(2)Mist 
 
(3)Pisces - 双栏 Scheme 


设置语言
编辑 站点配置文件， 将 language 设置成你所需要的语言。建议明确设置你所需要的语言，例如选用简体中文，配置如下： 
vi e:/hexo/_config.yml 
language: zh-Hans 
修改了配置文件后，重新执行一次hexo s重读一下配置文件

设置头像
编辑 主题配置文件， 修改字段 avatar， 值设置成头像的链接地址。其中，头像的链接地址可以是： 
（1）互联网上的地址http://example.com/touxiang.png 
（2）站点内的地址，放置在 source/images/ 目录下 ，如果没有此目录则创建即可，配置为： 
avatar: /images/touxiang.png 
(3)将图片复制到/images/目录中,我这里是E:\hexo\source\images目录，public目录中的文件则为hexo要上传到GitHub仓库上的文件

设置 作者昵称
编辑 站点配置 文件， 设置 author 为你的昵称。 
站点描述 
编辑 站点配置文件， 设置 description 字段为你的站点描述。 
站点描述可以是你喜欢的一句签名:)

站点分类
比如此处要在菜单上添加一个linux分组。

1、主题配置文件中添加菜单
搜索menu，在其下面添加Linux项，并指定目录，我写的是 
Linux: /categories/Linux 
如下图： 


2、编辑language文件
主题文件中每一个menu项，都需要使用翻译文件将它翻译一下才能正常显示，否则将显示为如下图所示： 
 
找到主题中的languages目录下的zh-Hans.yml,这里和前面设置的语言有关，我们前面设置的是zh-Hans，因此这里需要修改此文件，格式是“menu项: 翻译内容” 
如下所示，

vim e:/hexo/themes/hexo-theme-next-5.1.1/languages/zh-Hans.yml
1


3、博客文件设置
在每篇博客的头部说明的地方 
添加如下设置 
categories: Linux 
 
我们将看到如下效果： 


提醒：hexo目录中的scaffolds目录中的文件是模板文件，_post文件内容就是每次hexo new “filename”生成的文件的头部

4、添加分类小图标
 
如上图，在菜单图标开启的情况下，如果菜单项与菜单未匹配（没有设置或者无效的 Font Awesome 图标名字） 的情况下，NexT 将会使用 ？作为图标。

设定菜单项的图标，对应的字段是 menu_icons。 此设定格式是 item name: icon name，其中 item name 与上一步所配置的菜单名字对应，icon name 是 Font Awesome 图标的 名字。而 enable 可用于控制是否显示图标，你可以设置成 false 来去掉图标。 
(1)找到图标 
在http://www.fontawesome.com.cn/faicons/中找到图标的名字，找到linux图标的名字是“linux” 
(2)编辑主题配置文件 
找到munu_icons: 在下面添加item: value 
如下图：

 
(3)重新生成文件

hexo clean
hexo g
hexo s预览
hexo d上传到GitHub
1
2
3
4
站点标签
 
怎么显示这些标签呢？ 
在每篇博客的开头出加入 
(1)tags: Linux 
或者： 
(2)tags : 
- 标签1 
- 标签2 
每一行一个标签，也就是说每篇博客可以有多个标签 
如图： 


网页title处添加小图标


1、准备小图标.ico文件
可以在线转换图像文件，为ico文件，百度在线ico文件转换。 
我使用的在线转换网址是https://www.ico.la/，很方便，选择图片后，选择一下生成的ico文件尺寸，将直接下载到本地。我选择的尺寸是32x32.

2、编辑主题配置文件
favicon: 指定图标 
通常所有的网站的title图标，名字都叫做favicon.ico 
这里的默认主题配置文件中如下： 
favicon: /favicon.ico

3、复制文件到指定目录
此处的favicon.ico文件是放在/下面，因此需要将ico文件放入到source目录，这里是E:\hexo\source中.如果将ico文件放上去，刷新没有出来，很可能是浏览器有缓存的原因，换一个浏览器即可

 
主题说明文档http://theme-next.iissnan.com/getting-started.html 
其他更多主题配置请看next说明文档，比如添加评论，浏览量等功能。^_^
