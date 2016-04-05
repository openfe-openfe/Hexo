---
title: "使用Hexo搭建个人博客"
date: 2016-03-08 09:13:28
tags: ['Hexo','Git']
updated: 2016-03-15 15:11:28
comments: true
categories: Hexo
---

![ Hexo | center | 500*0 ](http://7xrnl9.com1.z0.glb.clouddn.com/Hexo.png)

最近想搭建一个独立的个人博客，可以将自己在工作学习中的一些知识及经验记录下来。不断积累知识，不断总结经验，让自己可以不断的进步、成长。

目前搭建独立的个人博客有很多种方式，你可以选择购买主机搭建动态博客 - [WordPress](https://wordpress.org)等，也可以使用[Github Pages](https://pages.github.com)来搭建一个静态的个人博客。我选择了现在很热门的[Github Pages](https://pages.github.com) + [Hexo](https://hexo.io) 的方式来搭建独立的个人博客。

## 前期准备

### 相关网站

在搭建个人博客的过程中，你可能会使用到下面几个网站。在这几个网站中都有相应的官方文档及教程。如果官方文档不能满足你，那么请[Google](https://www.google.com)。

[Github 官网](https://github.com)
[Github Pages](https://pages.github.com)
[Hexo 官网](https://hexo.io)
[Node.js 官网](https://nodejs.org)
[Git 官网](http://git-scm.com)

### 相关的Hexo博客搭建教程

[使用GitHub和Hexo搭建免费静态Blog](http://wsgzao.github.io/post/hexo-guide)
[hexo搭建静态博客以及优化](http://code.wileam.com/build-a-hexo-blog-and-optimize)
[一步步在GitHub上创建博客主页](http://www.pchou.info/web-build/2013/01/03/build-github-blog-page-01.html)

## 搭建Git环境

### 注册Github

进入[Github 网站](https://github.com)，按照提示进行注册，然后登录。

![ Github Sign up | 300*0 ](http://7xrnl9.com1.z0.glb.clouddn.com/Github_signup.png)

登录完成之后，在你的主页点击图标[ New Repository ](https://github.com/new)创建一个新的版本库，因为我们是使用[ Github Pages ](https://pages.github.com)去搭建我们的静态博客，所以版本库的名称应该是你的用户名+.github.io。如：我的用户名是：dkylin，那么版本库的名字应该是：[ dkylin.github.io ](https://github.com/dkylin/dkylin.github.io)，这个是一定不能出错的。因为之后你将要访问的你的博客地址就是：[ https://dkylin.github.io ](https://dkylin.github.io)。

![ Github Create Repo | 500*0 ](http://7xrnl9.com1.z0.glb.clouddn.com/Github_createRepo.png)

至此，Github账号创建完成，GIthub Pages 所需要的版本库也创建好了。

### 本地安装Git

进入[ Git 官网](https://github.com)，下载相应的 Git 版本，下载完成之后按照引导安装 Git 。安装完成之后在开始菜单中会有一个 Git Bash 。这是一个类似于Liunx的终端，在里面可以模拟Linux下的终端进行操作。

![ Git Local | 300*0 ](http://7xrnl9.com1.z0.glb.clouddn.com/Git_local.png)

### 配置SSH

打开 Git Bash ，执行下面的命令生成 SSH 访问私钥及公钥。

```powershell
$ ssh-keygen -t rsa -C "email@email.com"
```

![ ssh | 300*0 ](http://7xrnl9.com1.z0.glb.clouddn.com/ssh_rsa.png)

输入命令回车之后会提示你输入一些东西，不用管。一直回车到底就好了。然后你的 `~/.ssh` 文件下就会生成两个文件 `id_rsa` 和 `id_rsa.pub` 。

打开你的 Github -> setting -> SSH Keys 。然后点击 `New SSH Key` 创建一个新的SSH Key。`Title` 可以用你的计算机名，可以用以区分。将文件 `id_rsa.pub` 中的所以内容复制粘贴到 `Key` 下面。然后使用下面的命令测试是否可以连接上 Github 。

```powershell
$ ssh -T git@github.com
```

如果出现下图所示内容则证明连接成功。

![ ssh -T](http://7xrnl9.com1.z0.glb.clouddn.com/ssh-T.png)
 
## 安装Hexo

Hexo的安装在其[官方文档](https://hexo.io/zh-cn/docs/)中有很详细的说明。下面将简单介绍Hexo的安装。

### 安装前提

- 安装[ Node.js ](https://nodejs.org)，请进入Node.js 的官网下载安装。
- 安装[ Git ](http://git-scm.com)，前面已经说明，不再赘述。

### 安装Hexo
上面两个工具安装完整之后，打开 ` Git Bash ` ，只需要使用npm即可完成Hexo的安装。

```powershell
$ npm install -g hexo-cli
```

安装Hexo完成之后，执行下面的命令，Hexo将会在你制定的文件夹中新建所需要的文件。

```powershell
$ hexo init <folder>
$ cd <folder>
$ npm install
```

新建完成后，文件夹下的目录如下：
```powershell
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

- **_config.yml** 文件是网站的配置文件，可以在其中配置网站的大部分参数。
- **package.json** 文件是应用程序的信息。
- **source** 是资源文件夹，是用来存放用户资源的地方。
- **themes** 是[主题](https://hexo.io/themes)文件夹，Hexo会根据主题来生成不同的静态页面。
- **scaffolds**是模板件夹，当新建文章的时候，Hexo会根据模板来建立文件。

### 修改主题

我使用的是NexT主题，下面只介绍怎么安装这种主题，其他主题可以在Hexo Themes、github里面寻找。

先进入你的Hexo文件夹。然后使用下面的命令`clone`下`NexT`主题。
```powershell
$ git clone https://github.com/iissnan/hexo-theme-next.git themes/next
```
然后打开Hexo主文件夹下的**_config.yml**，修改其中的`theme` 属性。`theme:` 后面要加空格。

```
theme: next
```

### 本地查看调试
 
```powershell
$ hexo g #生成静态页面，生成的内容在public文件夹下
$ hexo s #启动本地服务，进行文章预览调试。hexo s --debug 命令可以用来调试
```

### 部署到Github Pages

先使用下面的命令对Git进行初始配置。
```powershell
$ git config --global user.name "your name"
$ git config --global user.email "email@email.com"
```

然后打开Hexo主文件夹下的**_config.yml**，设置其中的`deploy` 参数，详细请查看Hexo官方文档中[部署部分](https://hexo.io/zh-cn/docs/deployment.html)。

我的设置如下所示：

```powershell
deploy:
  type: git 
  repo: ssh://git@github.com/dkylin/dkylin.github.io.git
  branch: master
```

git地址建议使用SSH地址。在上面的参数设置好了之后，使用下面的命令安装[ hexo-deployer-git ](https://github.com/hexojs/hexo-deployer-git)插件，只有安装了插件之后才可以部署到Github Pages。

```powershell
$ npm install hexo-deployer-git --save
```

安装完插件之后使用下面的命令进行部署：

```powershell
$ hexo g #生成静态文件
$ hexo d #部署到github 
```

还有一个更快捷的命令：

```powershell
$ hexo d -g #在部署前先生成
```

### NexT 主题拓展

[ NexT 文档 ](http://theme-next.iissnan.com/) - NexT的详细配置可以在这里查看。

[ NexT Github 地址 ](https://github.com/iissnan/hexo-theme-next) - 想要二次开发，可以Fork一下。

## Hexo常用命令

下面仅列出几种常用的命令。更详细的命令请查看[Hexo官方文档](https://hexo.io/zh-cn/docs/commands.html)。

```powershell
$ hexo clean #清理之前生成的内容，即public文件
$ hexo g #生成静态文件
$ hexo d #部署
$ hexo s #启动本地服务，可以通过http://localhost:4000查看
$ hexo s --debug #使用debug模式启动服务
```
