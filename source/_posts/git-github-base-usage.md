---
title: Git系列之Github基础设置及使用详解
date: 2016-07-06 16:00:14
update: 2016-07-06 16:00:14
comments: true
categories: Git
tags: ['Git','Github']
---

![Github-Mascot](http://7xrnl9.com1.z0.glb.clouddn.com/image/git/github1.jpg)

在上一篇博客[**Git系列之初识Git与Github**](http://dkylin.com/archives/2016/git-github-base.html)中详细介绍了Git以及Github分别是什么，以及两者之间的关系。接下来本系列教程将带你一步一步的学习Git以及Github的详细用法。本篇博客将教你如何加入Github，并进行初步的配置及使用。

<!-- more -->

## 一、加入Github

[**Github**](https://github.com)的优秀在[上一篇](http://dkylin.com/archives/2016/git-github-base.html)博客中已经说得很详细了，如果想了解的话请自行去看上一篇博客：[**Git系列之初识Git与Github**](http://dkylin.com/archives/2016/git-github-base.html)。下面请跟着我的脚步一起加入Github大军吧，**Come On**。

### 1.1 注册账号

想要加入Github，首先你需要注册Github的账号，打开[Github网站主页:https://github.com](https://github.com)，然后你会看到下面的注册界面。

![Github-Register-step-1](http://7xrnl9.com1.z0.glb.clouddn.com/image/git/github-register.png)

根据提示依次填入：你的用户名、你的E-mail地址、你的账号密码。输入完成之后点击`Sign up for Github`按钮注册即可。注意你的用户名和E-mail必须是没有被注册过的才可以。如果提示错误请换个用户名或者E-mail再重新注册即可。

> 这里有个小小的提示，很多人在注册账号的时候可能为了方便就乱输用户名，比如：sadasdsdfg120。这种用户名难以记忆并且看着很不舒服。所以建议使用你的英文名，或者有意义的单词。而且建议你的其他平台都使用相同的名字，这样的话辨识度会比较高，你可以打开[我的博客-关于](http://dkylin.com/about/)界面，观察我各个平台的用户名，除了被人抢先注册的，其他的我都用了容易记忆并且统一的名字。

这一步成功之后，会跳转到注册的第二步，如下图所示。这时Github已经向你的邮箱中发送了一封验证邮件，进入你的邮箱点击验证即可。

![Github-Register-step-2](http://7xrnl9.com1.z0.glb.clouddn.com/image/git/github-register-2.png)

如上图所示，这一步主要是让你选择一个个人计划，我们直接使用默认的免费的就可以了。下面有一个选择框是提示你可以在下一步创建一个organization(组织)的。这个可以不选择，直接点击`Finish sign up`按钮完成注册即可。完成之后会出现下面的界面。

![Github-Register-Finish](http://7xrnl9.com1.z0.glb.clouddn.com/image/git/github-register-finish.png)

看到这个就说明你已经注册成功了。你可以选择`Read the guide`按钮去看一下Github官方为新人写的引导文档，或者点击`Start a project`去创建一个开始一个项目，创建一个仓库。再或者点击界面右上角你的头像->`Your profiles`进入你的主页。

### 1.2 Github界面介绍

到此我们已经成功注册了Github账号，以后就可以轻松+愉快的使用Github了。那么新人可能会对Github比较陌生，对界面中的各种功能、按钮不清楚。那么接下来我会对此一一解释。先来看张图：

![Github-Profiles](http://7xrnl9.com1.z0.glb.clouddn.com/image/git/github-profiles-init.png)

这是新注册用户的界面，在图中我标记的比较详细，下面一一解释(下面的编号对应图中的编号)。

1. 进入Github主页的按钮;
2. 搜索框，你可以在这里搜索一些开源项目;
3. 你的头像，可以在设置里面进行设置;
4. 你的昵称(账号名称)，你可以在设置中设置你的昵称;
5. 提示用户添加一个类似于个人描述或个性签名的东西;
6. 这是你加入Github的时间;
7. 这有三个数据，Followers:追随(关注)你的人;Starred:你Star(点赞)的项目;Following:你追随(关注)的人;
8. Pull requests:简单地说就是别人向你的仓库提交合并请求，后面会详细说;
9. Issues:这就是别人对你的项目提的问题;
10. Gist:代码片段，后面会详细说;
11. 这是系统给你的提示消息，提示你去编辑自己的资料;
12. 你每天的贡献度;
13. 你的项目仓库;
14. 你平时活动，或是动态，比如在哪个项目做了提交等;
15. 这是你每天向Github提交的贡献的分布图;
16. 由于这是新账户，还没有贡献，所以贴出了一个官方对于贡献的解释，帮助新手理解;
17. 这是一个创建新仓库或新组织的按钮，点击按钮会弹出菜单，下面会详细讲解;
18. 这是跟用户相关的一个按钮，点击之后也会有一个跟用户相关的菜单，下面会详细讲解。

## 二、Github的配置

到这里，我们已经创建好了一个Github账户，并且详细的了解了Github个人主页中的布局以及详细信息。那么现在我们来学习一下如何对Github进行一些简单的设置。

点击`1.2章节图片`中的`编号18`的按钮，然后选择菜单下方的`Settings`进入你的`设置界面`。

![Personal Settings](http://7xrnl9.com1.z0.glb.clouddn.com/image/git/personal_settings.png)

### 2.1 Profile - 个人简介

进入设置界面之后，首先引入眼帘的是`Profile(个人简介)`页面。在页面右侧是具体需要设置的一些信息。设置内容如下：

1. Profile picture：头像设置，点击`Upload new picture`按钮进行头像上传更新即可;
2. Name：你的昵称或姓名，有别于登录用户名，这仅用来显示，类似于QQ昵称;
3. Public email：设置公开邮箱，可以选择你注册的邮箱，也可以不设置;
4. Bio：个人简介，Tell a little about yourself.也可以@一些人或组织;
5. URL：个人主页地址，或其他网络地址;
6. Company：你的公司;
7. Location：你的位置或地址。

设置完上面的信息之后，点击`Upload profile`按钮更新个人信息。

在下面还有三个小块：
1. Contributions：关于Contribution(贡献)的设置，可以设置把私人仓库的贡献显示在我的主页，然而我们用的都是免费的账户，是没有私人仓库的。所以可以无视。
2. Github Developer Program：如果你想开发一些应用、工具之类的集成到Github，可以加入Github开发组。
3. Jobs profile：勾选`Available for hire`选项，表示你是可被招聘的，有工作会联系你。

### 2.2 Account - 账号设置

1. Change password：修改你的登录密码，如果忘记密码可以点击`I forgot my password`重置;
2. Change username：修改你的用户名，这是可用于登录的用户名，而不是之前的昵称;
3. Delete account：删除账号，请不要作死去尝试。

### 2.3 Emails

Github需要你设置一个Primary的邮箱(主邮箱)，以便给你发送一些通知。Github会默认将你注册时的邮箱作为主邮箱，如果你想修改的话可以添加一个邮箱，然后设置为主邮箱。

Github默认邮箱都是public，即公开的，你可以勾选`Keep my email address private`选项修改为private(私有的)。不过如果你想使用私有邮箱进行命令行操作的话就需要在Git中设置你的邮箱。具体的操作方法官方给了链接，可以点击查看。

Email preferences：邮件偏好设置，第一个选项是发送所有邮件给我，包括我没有订阅的。第二个选项是仅发送跟我账号相关的和我订阅的邮件。

### 2.4 Notifications - 通知

这里主要是设置Github给你发送通知或者消息的方式。

How you receive notifications

- Automatic watching：当你获得了一个仓库的push权限时，会自动发送通知给你。在下方可以设置自动关注。
- Participating：如果你参与的项目有新的消息，或者有人@你了就会发消息给你，可以选择邮件或者网页通知两种方式。
- Watching：如果你关注的项目有更新的话会通知你，可以选择邮件或者网页通知两种方式。

Notification email

- Primary email address：设置主邮箱，通知将发送到这个邮箱;
- Settings：设置需要通知你的事件。

### 2.5 其他设置

新手使用Github基本上设置上面的内容就可以了，下面的设置暂时不会用到。后面的教程中会在用到的时候进行详细的讲解。下面对其他的设置进行简单的介绍：

1. Billing：如果想要更好的服务，可以在这里花钱购买相应的服务。
2. SSH and GPG keys：设置SSH跟GPG密钥的地方，如果你想用SSH工具连接到你的Github的话，需要在这里设置你的SSH公钥，这个会在后面的教程中详细说明。
3. Security：顾名思义，安全方面的设置，可以在里面开启双重认证，查看当前那些主机登录了你的Github账号，查看登录记录。
5. OAuth applications：这是用来查看你的Github绑定了那些第三方的应用的。
6. Personal access tokens：如果你想使用Github API进行一些开发的话，需要在这里生成API Token。
7. Repositories：对仓库的一些设置。
8. Organizations：对组织的一些设置。
9. Saved replies：被保存的回复在这里可以看到。

## 三、Github的基本使用

创建完账号并且进行了基本的设置之后，我们就可以愉快的使用Github进行代码管理了。下面来看一下Github的基本使用方法。

### 3.1 创建仓库

点击1.2章节图片中的`编号17`的按钮，可以看到有三个选项：New repository(创建仓库)、Import repository(导入仓库)和New organization(创建组织)。点击`New repository`选项创建一个新的仓库。

![Create New Repository](http://7xrnl9.com1.z0.glb.clouddn.com/image/git/Create_new_repository.png)

如上图所示是创建新仓库的界面，输入仓库名称，然后系统会检测是否重名，如果已经有重名的仓库则无法创建，需要修改仓库名才可。仓库名输入完成之后，可以在下面输入对仓库的描述。

在描述的下方可以选择仓库是公开的还是私有的，但是对于免费账户来说是没有私有仓库的。所以一般都是默认公开。如果需要私有仓库，则可以在2.5章节中的`Billing`设置中付费开启私有仓库的权限。

然后是`Initialize this repository with a README`选项，默认是不勾选的，不勾选的状态下将会创建一个空仓库。如果勾选的话则会创建一个仓库，仓库中会有一个`README.md`文件。当你配置好之后，点击按钮`Create repository`即可创建一个新的仓库。

![New Repository](http://7xrnl9.com1.z0.glb.clouddn.com/image/git/new_repository.png)

在上图中我们可以看到一个仓库的完整界面，在这个界面中有很多的功能，在此就不一一说明了，后面会专门写一篇文章来详细介绍这些功能。

### 3.2 文件操作

![File Operation](http://7xrnl9.com1.z0.glb.clouddn.com/image/git/File_operation.png)

在界面中间，你可以看到如上图所示的操作面板，主要功能如下：
1. Create new file：创建新文件，点击按钮之后可以创建一个新文件并进行文件编辑，编辑完成后保存提交就可以把新文件保存在仓库中了。
2. Upload files：上传文件，点击按钮之后进入上传文件的界面，选择你想要上传的文件即可将文件上传到仓库中。
3. Find file：寻找文件，当你的仓库中有很多文件的时候，你可以使用这个功能快速的查找文件。
4. Clone or download：克隆或下载仓库，点击按钮之后可以弹出一个面板，可以复制仓库地址进行克隆，或者选择`Download ZIP`，系统将把仓库中所有文件打包成ZIP压缩文件并下载到本地。

## 结语

本篇文章详细的介绍了如何加入Github，并对Github进行基础的设置，最后介绍了一些基础的操作。

由于文章篇幅有限，而Github又是如此的博大精深，所以在本篇文章中可能无法面面俱到。在之后的系列文章中，我会对Github(包括Git)进行更加全面，更加深入的介绍。

> 如果你在阅读本文中遇到了理解不了的地方或者认为有错误的地方，请在下方评论，我会及时进行解答或修改。