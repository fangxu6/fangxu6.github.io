---
title: My First Post
date: 2018-04-11 10:24:04
tags: hexo
description: hexo编写blog教程
---

【Hexo搭建独立博客全纪录】（三）使用Hexo搭建博客
发表于 2017-05-12 | 分类于 前端工具 | 阅读次数 2038 | 阅读次数 2190

每周一篇的频率更新博客，今天【Hexo搭建独立博客全纪录】系列终于进入正题：使用Hexo搭建博客，我搭建博客是跟着网上的教程一步一步做的，在此基础上结合官方文档，构成本文的内容。

参考资料：

    使用hexo+github搭建免费个人博客详细教程-liuxianan：http://blog.liuxianan.com/build-blog-website-by-hexo-github.html
    Hexo官方使用文档：https://hexo.io/zh-cn/docs/
    NexT 官方使用文档：http://theme-next.iissnan.com/

前言

使用github pages服务搭建博客的好处有：

    全是静态文件，访问速度快；
    免费方便，不用花一分钱就可以搭建一个自由的个人博客，不需要服务器不需要后台；
    可以随意绑定自己的域名，不仔细看的话根本看不出来你的网站是基于github的；
    数据绝对安全，基于github的版本管理，想恢复到哪个历史版本都行；
    博客内容可以轻松打包、转移、发布到其它平台；
    等等；

准备工作

在开始一切之前，你必须已经：

    有一个github账号，没有的话去注册一个；
    安装了node.js、npm，并了解相关基础知识；
    安装了git for windows（或者其它git客户端），安装过程在【Hexo搭建独立博客全纪录】（一）使用Git和Github中已讲过

本文所使用的环境：

    Windows7
    node.js@6.9.4 —— git命令行输入node -v
    git@2.8.1.windows.1 —— git命令行输入git version
    hexo@3.3.1 —— git命令行输入hexo -v或hexo version

搭建github博客
创建仓库

新建一个名为你的用户名.github.io的仓库，比如说，如果你的github用户名是test，那么你就新建test.github.io的仓库（必须是你的用户名，其它名称无效），将来你的网站访问地址就是 http://test.github.io 了，是不是很方便？

由此可见，每一个github账户最多只能创建一个这样可以直接使用域名访问的仓库。

几个注意的地方：

    注册的邮箱一定要验证，否则不会成功；
    仓库名字必须是：username.github.io，其中username是你的用户名；

创建成功后，以后你的博客所有代码都是放在这个仓库里啦。
配置SSH key（配置过的请跳过）

这一步骤在【Hexo搭建独立博客全纪录】（一）使用Git和Github4.1和4.2中也已讲过，如果在前面已经配置过SSH key，则可以跳过此步骤。

为什么要配置这个呢？因为你提交代码肯定要拥有你的github权限才可以，但是直接使用用户名和密码太不安全了，所以我们使用ssh key来解决本地和服务器的连接问题。
创建SSH Key

查看是否已经有ssh密钥：
打开用户主目录”C:\Users\Administrator.hp-PC”，
看看有没有.ssh目录。

    如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件。如果有，可以直接跳至下一小节“Github中添加SSH Key”

    如果已经有ssh密钥，想要重新生成ssh密钥，需要清理原有ssh密钥：

    $ mkdir key_backup
    $ cp id_rsa* key_backup
    $ rm id_rsa*

    如果没有，打开命令行，输入命令ssh-keygen -t rsa –C “youremail@example.com”。此处的邮箱地址，你可以输入自己的邮箱地址。在回车中会提示你输入一个密码，这个密码会在你提交项目时使用，如果为空的话提交项目时则不用输入。这个设置是防止别人往你的项目里提交内容。

    由于我本地此前运行过一次，所以本地有，如下所示：

".ssh"

id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。由于之前使用Github客户端，因此还有github_rsa和github_rsa.pub两个文件。known_hosts文件如果没有暂时不管。

验证是否连接成功，连接成功显示Hi baoyuzhang! You've successfully authenticated, but GitHub does not provide shell access.。如下：
"验证是否连接成功"
Github中添加SSH Key

登录github，点击个人头像打开”settings”，再打开”SSH and GPG keys”页面，然后点击”New SSH Key”，填上任意title，在”Key”文本框里黏贴id_rsa.pub文件的内容，点击 Add Key，你就应该可以看到已经添加的key。如下：
"Github中添加SSH Key"
使用hexo博客框架

Hexo官方使用文档：https://hexo.io/zh-cn/docs/

本文使用的命令和官方文档不太一样，但意思相同，达到的效果也相同。初学者建议跟着本文一步一不做。
hexo简介
a.hexo是什么

Hexo是一个简单、快速、强大的基于 Github Pages 的博客框架，支持Markdown格式，有众多优秀插件和主题。

github: https://github.com/hexojs/hexo

Hexo中文官网： https://hexo.io/zh-cn/
b.hexo原理

由于github pages存放的都是静态文件，博客存放的不只是文章内容，还有文章列表、分类、标签、翻页等动态内容，假如每次写完一篇文章都要手动更新博文目录和相关链接信息，相信谁都会疯掉，所以hexo所做的就是将这些md文件都放在本地，每次写完文章后调用写好的命令来批量完成相关页面的生成，然后再将有改动的页面提交到github。
安装
a.注意事项

安装之前先来说几个注意事项：

    很多命令既可以用Windows的cmd来完成，也可以使用git bash来完成，但是部分命令会有一些问题，为避免不必要的问题，建议全部使用git bash来执行；
    hexo不同版本差别比较大，网上很多文章的配置信息都是基于2.x的，所以注意不要被误导；
    hexo有2种_config.yml文件，一个是根目录下的全局的_config.yml，一个是各个theme下的_config.yml。为了描述方便，在以下说明中，将前者称为 站点配置文件， 后者称为 主题配置文件。

b.安装Hexo

在git bush输入以下命令：

$ npm install -g hexo

初始化
a.初始化

在电脑的某个地方新建一个名为hexo的文件夹（名字可以随便取），比如我的是E:\web\github\test\hexo，由于这个文件夹将来就作为你存放博客代码的地方，所以最好不要随便放。


hexo会自动下载一些文件到这个目录，包括node_modules，目录结构如下图：

.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes

b.生成静态文件

执行以上命令之后，hexo就会在public文件夹生成相关html文件，这些文件将来都是要提交到github上你的用户名.github.io的仓库上去的：

c.本地预览

hexo s是开启本地预览服务，打开浏览器访问 http://localhost:4000 即可看到内容，Ctrl+C停止本地预览。

若浏览器一直在转圈但是就是加载不出来，或执行hexo s命令出现以下提示：

这一般情况下是因为4000端口占用的缘故，因为4000这个端口太常见了，解决端口冲突问题请参考这篇文章：

http://blog.liuxianan.com/windows-port-bind.html

第一次初始化的时候hexo已经帮我们写了一篇名为 Hello World 的文章，默认的主题比较丑，打开时就是这个样子：

更改主题

既然默认主题很丑，那我们别的不做，首先来替换一个好看点的主题。

    官方主题：官方提供的各种主题

    有哪些好看的 Hexo 主题? - 知乎

这里我推荐使用使用量排名第一的主题NexT。更换NexT主题可以参考NexT 官方使用文档，点击开始使用一步一步操作。下面做一个示范，并且下周写的优化设置的文章，也是基于NexT主题，可以跟着我一步一步来做。
a.下载主题

b.启用主题

打开根目录下站点配置文件_config.yml， 找到 theme 字段，并将其值更改为 next。

到此，NexT 主题安装完成。下一步我们将验证主题是否正确启用。
c.验证主题

在切换主题之后、验证之前， 我们最好使用 hexo clean 来清除 Hexo 的缓存，以免出现一些莫名其妙的问题。然后重新执行hexo g来重新生成。

使用hexo s本地预览，如果你的博客变为下图的样子，则安装NexT主题成功：

上传到github
a.配置站点配置文件

打开根目录下站点配置文件_config.yml，配置有关deploy的部分：

b.安装插件

此时，直接使用hexo d部署到github，将出现如下错误：

这是因为需要安装如下插件：

c.部署到github

其它命令不确定，hexo d部署这个命令一定要用git bash，否则会提示Permission denied (publickey).

中间好长的安装过程，然后安装成功：

这时，打开https://你的用户名.github.io/，出现下图，则表示部署到github成功：

再打开github仓库，已经有了内容：

基本配置

我们自己的博客出现Hello World并不是我们想要的，那么我们来进一步配置，将它真正我们自己的博客。

打开根目录下站点配置文件_config.yml，设置如下内容：

参数 	描述
title 	网站标题
subtitle 	网站副标题
description 	网站描述
author 	您的名字
language 	网站使用的语言
timezone 	网站时区。Hexo 默认使用您电脑的时区。

其中，description主要用于SEO，告诉搜索引擎一个关于您站点的简单描述，通常建议在其中包含您网站的关键词。author参数用于主题显示文章的作者。

配置完成后，重新部署，会发现如下变化：

但文章内容还不是我们自己的呀，下面来写我们自己的博客。
写博客
新建文章

执行hexo new '文章名'，hexo会帮我们在..\hexo\source\_posts下生成相关md文件：

打开这个文件就可以开始写博客了，默认生成如下内容：

当然你也可以直接自己新建md文件，用这个命令的好处是帮我们自动生成了时间。一般完整格式如下：

---
title: postName #文章页面上的显示名称，一般是中文
date: 2013-12-02 15:30:16 #文章生成时间，一般不改，当然也可以任意修改
categories: 默认分类 #分类
tags: [tag1,tag2,tag3] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: 附加一段文章摘要，字数最好在140字以内，会出现在meta的description里面
---
以下是正文

删除hello-world.md，同时删除public文件夹下的hello-world文件夹：

将编写好的文章保存，重新生成和部署就可以啦！

新建页面

假如我新建一个名字为about的页面：

在source文件夹下，将生成about文件夹：

部署后将在public文件夹生成一个新的html页面：hexo\public\about\index.html，通过访问https://用户名.github.io/about/访问这个页面：

你可以通过编辑index.html，添加自己的简历等其他你想加入的内容。但注意，它是新建了一个html页面，不是文章，不会出现在博文目录。

至此，博客的基本功能已经全部实现，下面的高级配置，让你更加方便快捷的写博客！
高级配置

现在，Hexo根目录结构如下图：

.
├── _config.yml
├── db.json
├── package.json
├── public
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes

下面介绍的几个是会经常用到的：
_config.yml

网站配置信息，也就是本文所称的站点配置文件，可以在此配置大部分的参数。基本配置在前面基本已经设置过了。
scaffolds

模版文件夹。新建文章时，Hexo 会根据 scaffold 来建立文件。

Hexo的模板是指新建的markdown文件中默认填充的内容。例如，在使用hexo new 文章名时，默认生成的md文件会包含如下内容：

默认内容是哪里来的呢？就在scaffold/post.md中保存：

假如对每篇博客我都需要添加分类categories，每次都手动添加太麻烦，我希望每次默认生成都有categories:，那么就可以在scaffold/post.md中添加：

保存后，每次新建一篇文章时都会包含post.md中的内容。

当然，你也可以在scaffolds文件夹下创建自己的博客模板，我创建一个名为blog的模板：

通过如下命令调用我创建的blog模板新建文章：

在_posts中生成md文件：

public

该文件夹中的内容将被最终push到github仓库中。
source

资源文件夹是存放用户资源的地方。除_posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件（如刚刚生成的about文件夹）会被拷贝到 public 文件夹。
为github仓库添加readme

既然 source 文件夹中的内容将全部被推送到 public 文件夹，public 文件夹中的内容又将被最终push到github仓库中，那么如果我们想要为github仓库添加readme.md，只要在 source 文件夹中创建就好了：

部署到github，就有readme了，但我们发现，README.md已经被解析为README.html，显示的全是html代码，并不是我们想要的文档格式的内容：

为了解决这个问题，我们回到source文件夹，将README.md重命名为README.MDOWN，再部署到github：

source文件夹中，.md会被解析为html，并放到 public 文件夹被push到github，但.MDWN不会被解析。
themes

主题文件夹，下载的主题都保存在此文件夹下。Hexo 会根据主题来生成静态页面。

最终效果

点击查看最终效果

搭的这个博客只是为了写这篇文章，我真正的博客在这里哟！
Hexo常用命令

常见命令

hexo new "postName" #新建文章
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #部署到GitHub
hexo help  # 查看帮助
hexo version  #查看Hexo的版本

缩写：

hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy

组合命令：

hexo s -g #生成并本地预览
hexo d -g #生成并上传