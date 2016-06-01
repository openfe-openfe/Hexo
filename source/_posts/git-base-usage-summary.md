---
title: Git基础用法总结
comments: true
date: 2016-04-17 15:08:47
update: 2016-04-17 15:08:47
categories: Git
tags: ['Git']
---

最近在忙着面试找工作，一直没更新博客。今天周末抽时间把Git相关的一些知识及用法整理总结一下。

Git是目前世界上最先进的分布式版本控制系统，没有之一，对，没有之一。著名的同性交友网站-Github，使用的就是Git存储。无数的开源项目在Github上汇聚，由此可知Git的威力。

<!-- more -->

## 一、Git简介

Git是一个分布式的版本控制系统，与集中式的版本控制系统不同的是，每个人都工作在通过克隆建立的本地版本库中。也就是说每个人都拥有一个完整的版本库，查看提交日志、提交、创建里程碑和分支、合并分支、回退等所有操作都直接在本地完成而不需要网络连接。

对于Git仓库来说，每个人都有一个独立完整的仓库，所谓的远程仓库或是服务器仓库其实也是一个仓库，只不过这台主机24小时运行，它是一个稳定的仓库，供他人克隆、推送，也从服务器仓库中拉取别人的提交。

## 二、Git安装

Git是Linux之父Linus的第二个伟大的作品，它最早是在Linux上开发的，被用来管理Linux核心的源代码。后来慢慢地有人将其移植到了Unix、Windows、Max OS等操作系统中。

想要使用Git，第一步当然是安装。

### 2.1 Linux安装

我以Ubuntu系统为例，使用下面的命令可以查看该系统是否安装了Git。

```
$ git
The program 'git' is currently not installed. You can install it by typing:
sudo apt-get install git
```

如果如上所示，则说明没有安装Git，根据提示使用下面的命令安装Git。

```
$ sudo apt-get install git
```

有一些教程的安装命令是：`sudo apt-get install git-core`，那是因为以前有个软件叫GIT(GNU Interactive Tools)，所以Git只能叫`git-core`，不过现在由于Git太有名。GNU Interactive Tools已经改名为`gnuit`，`git-core`也正式改名为`git`。所以**老一点**的教程或者系统是使用`sudo apt-get install git-core`。

### 2.2 Windows上安装

在[Git官网](https://git-scm.com/downloads)下载Windows版本的安装软件。根据你的电脑系统选择32位或者是64位。下载完成之后。双击运行，根据引导进行安装。

安装完成之后，你可以在开始菜单中找到“Git”->"Git Bash"，打开“Git Bash”之后如果出现一个类似命令行窗口的东西，则说明安装成功。如下图所示：

![Git Bash](http://7xrnl9.com1.z0.glb.clouddn.com/image%2Fgit%2Fgit-bash-win.png)



### 2.3 Mac OS X 上安装

> 等我买了Mac笔记本再更新...心疼的摸一把自己的钱包。

### 2.4 安装后的配置

安装完成之后可以使用下面的命令查看当前Git的版本。

```
$ git version
git version 1.9.1
```

Git安装完成之后需要进行一些简单的设置。

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

`--global`这个参数的意思就是当前机器中的所有仓库都是用这个配置。

有时候你在使用Git的过程中可能会出现`warning: LF will be replaced by CRLF`。这是因为Windows中的换行符为 CRLF， 而在linux下的换行符为：LF。所以有时候你在执行`git add .`时，系统会提示：LF 将被转换成 CRLF。使用下面的命令可以禁用自动转换。

```
$ git config --global core.autocrlf false
```

## 三、Git用法

### 3.1 创建版本库

创建版本库非常的简单，选择一个合适的地方，使用下面的命令创建版本库。

```
$ mkdir TestGit # 创建文件夹
$ cd TestGit # 进入文件夹
$ git init # 初始化成Git版本库
Initialized empty Git repository in /home/kylin/Repositories/TestGit/.git/
```
至此一个空的Git仓库就创建好了，就是这么的简单。在这个目录下你会看到一个`.git`目录。这个目录就是Git用来跟踪管理版本库的。所以千万不要修改删除这个文件夹中的文件。因为这是隐藏目录，所以如果你看不见可以使用命令`ls -a`来查看。

### 3.2 Git基本命令

当创建好了一个版本库之后，我们就可以在版本库中进行版本控制了。所以我们需要了解一下Git的一些基本命令。

> * git add # 将工作区的修改提交到暂存区
> * git commit # 将暂存区的修改提交到当前分支
> * git status # 查看当前仓库的状态
> * git diff # 查看修改
> * git log # 查看提交历史
> * git reset # 回退到某一个版本
> * git reflog # 查看历史命令，类似与Linux中的history

### 3.3 工作区和暂存区

上面说到了工作区和暂存区的概念，想要用好Git，那么了解Git中的工作区、暂存区之间的关系是很重要的。先来看一张图。

![工作区和暂存区](http://7xrnl9.com1.z0.glb.clouddn.com/image%2Fgit%2Fgit-repository.png)

工作区：在你的电脑中可以看见的Git仓库的那个目录，如之前我创建的`TestGit`目录。那文件夹就是一个工作区。当我们往版本库提交的时候有两个步骤：

> 1. git add # 这一步就是将工作区中修改添加到暂存区（stage）中。
> 2. git commit # 这一步其实就是将暂存区中的修改添加到当前的分支中。

所以我们在提交的时候一般都是这样：

```
$ git add . # 将工作区中所有修改提交到暂存区，"."点的含义就是所有修改
$ git commit -m "first commit" # 将暂存区的所有修改提交到当前分支，-m参数是命名当前的提交
```

### 3.4 其他命令使用

当我们修改了版本库中的一些文件之后，可以使用`git status`命令来查看都有那些修改。

![git-status](http://7xrnl9.com1.z0.glb.clouddn.com/image%2Fgit%2Fgit-status.png)

当你想要查看某个文件修改那些具体的内容，可以使用`git diff <filename>`命令来查看。

![git-diff](http://7xrnl9.com1.z0.glb.clouddn.com/image%2Fgit%2Fgit-diff.png)

当你的版本库有了很多次提交之后，你想查看以往的提交记录的话可以使用`git log`来查看历史提交记录。

![git-log](http://7xrnl9.com1.z0.glb.clouddn.com/image%2Fgit%2Fgit-log.png)

如果你想查看两次提交版本之间有什么修改的话，也可以使用`git diff`来查看。

![git-diff-2](http://7xrnl9.com1.z0.glb.clouddn.com/image%2Fgit%2Fgit-diff-2.png)

如果你想要回到之前的版本的话，可以使用`git reset`命令。

![git-reset](http://7xrnl9.com1.z0.glb.clouddn.com/image%2Fgit%2Fgit-reset.png)

如上图所示，先使用`git log`查看历史版本记录，可以看到有两个。先使用`git reset --hard 51fca33`命令让版本回退到第一次提交的时候。再使用`git log`命令查看一下当前的版本发现只有一个了，这时候版本成功回退到第一次提交的时候。

那么如果我此时又想让版本回退到第二次提交的时候怎么办呢？此时只有一个版本啊。这时使用`git reflog`可以查看历史操作记录。只需要使用命令`git reset`回退到之前的操作就可以了。如下图所示：

![git-reflog](http://7xrnl9.com1.z0.glb.clouddn.com/image%2Fgit%2Fgit-reflog.png)

在上面的图中可以看到，commit_id是`51fca33c34eec36c787faef180ccf2d3e5017462`一大长串。但是我的命令中只使用了`51fca33`前面一小段。因为Git会根据你的输入自动去寻找相应的版本号，所以并不需要输入完整的ID,但是注意也不要输入太短。

掌握了上面这些命令的话，基本上Git就可以很顺畅的使用了。当然在实际应用中，我们还需要学习远程仓库和分支的使用。

## 四、远程仓库

Git是分布式版本控制系统，同一个仓库，可以同步到很多不同的主机中，那么如何同步呢？这个时候只需要使用Git中克隆命令就可以从别的仓库中复制一份一样的到你的版本库中。非常的方便。

首先需要使用克隆命令从远程服务器克隆版本库到本地，使用下面的命令进行克隆。

```
$ git clone <url> # url是远程版本库的地址
```

使用`git remote -v`查看远程版本库的信息。如下图所示，前面的origin是远程库的别名，后面的地址是远程库的具体地址。使用`git remote remove <别名>`可以删除一个远程仓库。

![git-remote-remove](http://7xrnl9.com1.z0.glb.clouddn.com/image%2Fgit%2Fgit-remote-remove.png)

当我们使用`git clone <url>`命令克隆远程仓库的时候，已经自动添加了此远程仓库，别名为`origin`。如果你是在本地`git init`自己手动创建的版本库，想要和远程仓库进行关联，需要使用命令`git remote add <remote-name> <url>`先添加一个远程仓库。添加完成之后可以使用`git remote -v`查看。

![git-remote-add](http://7xrnl9.com1.z0.glb.clouddn.com/image%2Fgit%2Fgit-remote-add.png)

当然，也许你还想要添加其他的远程仓库，只需要在添加其他远程仓库的时候保证别名不一致就可以了。`origin`是默认的别名。

当你添加了远程仓库之后，本地仓库就可以与远程仓库进行关联以及操作了。如果你想把本地仓库中的修改提交到远程仓库，可以使用命令`git push <remote-name> <branch>`提交到远程仓库。

![git-push](http://7xrnl9.com1.z0.glb.clouddn.com/image%2Fgit%2Fgit-push.png)

如果远程仓库中的文件有所改动，那么可以使用命令`git pull <remote-name> <branch>`下载远程仓库中的修改并进行快速合并。如果有与本地文件相冲突的内容，在命令行中会有提示，根据提示进行手动消除冲突就可以了。这样的话就可以让自己的本地库与远程库保持一致了。

## 五、分支管理

Git中分支的使用非常快捷方便，相对于SVN来说，速度快了不知道多少。熟练的使用分支是团队协作开发的基础。

### 5.1 创建与合并分支

在Git中分支的创建、切换、删除都十分的快捷方便。主要命令如下：

> * git branch # 查看所有本地分支
> * git branch <new-branch> # 创建一个新的分支
> * git checkout <branch> # 切换到指定分支
> * git branch -d <branch> # 删除一个分支
> * git merge <branch> # 将指定的分支合并到当前分支

![git-base-branch](http://7xrnl9.com1.z0.glb.clouddn.com/image%2Fgit%2Fgit-base-branch.png)

上图显示了分支的查看、创建、创建、切换、删除这几种操作。使用`git branch`可以查看本地所有分支，并且分支前面有一个`*`号的是当前分支。

在团队的实际开发中，`master`主分支应该是最稳定的，用来发布新版本。`dev`分支是开发分支，在这个分支中进行开发，每一个程序员应当有自己的分支，如我在开发中需要创建我自己的分支`kylin`，我写的程序全部提交到`kylin`分支中，当完成了某一个模块，再将`kylin`分支的内容合并到`dev`中。这是团队开发中分支的基本管理，这里用到了分支合并，请看下图。

![git-merge](http://7xrnl9.com1.z0.glb.clouddn.com/image%2Fgit%2Fgit-merge.png)

在上图中，我先切换到`dev`开发分支，修改了`README.md`文件之后，将修改提交到`dev`分支。然后切换到`master`分支，将`dev`分支使用命令`git merge <branch>`合并到`master`主分支中。这就是分支合并的基本用法。

### 5.2 团队协作

在团队协作中，我们需要了解分支管理。团队协作中的分支管理应该如同下图所示。

![git-branch-group](http://7xrnl9.com1.z0.glb.clouddn.com/image%2Fgit%2Fgit-brach-group.png)

> 1. `master`主分支应该是最稳定的，主要用来发布新版本。
> 2. `dev`分支是开发分支，这是最不稳定的，程序员们都在这上面干活，只有当版本发布的时候，才将此分支合并到`master`主分支上。
> 3. 每一个程序员应当有自己的分支，当完成某一块的内容的时候，往`dev`分支合并即可。

当程序出现了BUG的时候，你可以在`master`或是`dev`分支创建一个临时分支`issue-001`，在这个分支修复BUG，修复完成之后将此分支合并到主分支，然后删除BUG分支即可。

## 总结

Git版本控制器真心是非常非常好用，在团队开发中可以很好的提高工作效率，真心是团队开发必备。本篇博客由于篇幅的原因，不能把Git的知识讲解的特别详细。仅将平时最常用的知识及命令整理总结了一下，如果有说的不好的地方还请指出来，我将及时改正。

如果有小伙伴想要更加深入的了解Git的使用及其他的内容的话，在这里给大家推荐一些比较好的教程。

1. [廖雪峰的Git教程（非常推荐）](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
2. [Git权威指南](http://pan.baidu.com/s/1o7LWJbW)
3. [Pro Git 中文版本](http://pan.baidu.com/s/1nuG9erR)

最后在附上一张**Git命令速查表**。

![git-command](http://7xrnl9.com1.z0.glb.clouddn.com/image%2Fgit%2FGit.jpg)
