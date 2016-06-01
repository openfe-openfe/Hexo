---
title: 多功能媒体播放器
comments: true
date: 2016-03-21 21:03:50
update: 2016-03-21 21:03:50
categories: 作品集
tags: ['Android','我的作品','播放器','HTML5']
---

在`中科大先进技术研究院-龙芯中科联合实验室`工作期间，前期主要是自我学习，熟悉单位等，在一段时间的熟悉之后，我们开始正式接手项目的开发工作，而我的第一个项目就是`多功能媒体播放器`APP的开发。

<!-- more -->

## 一、项目概述

"多功能媒体播放器"是一款为用户提供视频播放，在线HTML5游戏及电子书阅读的娱乐类App,该项目由本人主导开发，负责客户端70%的功能实现，并且独立完成数据库及服务器端的设计与开发。项目使用了UIL开源库加载图片，视频模块使用Vitamio开源以支持更全面的视频格式，文档模块支持TXT、PDF、EPUB、MOBI四种主流电子书格式。

> * 项目名：多功能媒体播放器
> * 开发语言：Android
> * 运行平台：Android平台
> * 数据库：MySQL
> * 开发工具：Eclipse、MyEclispe 2013
> * 版本控制器：Git

## 二、系统功能

该系统主要分为四个模块：

> 1. 用户模块 - 提供基本用户功能
> 2. 视频模块 - 提供视频播放功能，支持大部分视频格式
> 3. 游戏模块 - 支持HTML5在线游戏
> 4. 文档模块 - 支持TXT、PDF、EPUB和MOBI四种主流格式的电子书

### 2.1 用户模块

> * 用户注册（需短信验证）
> * 用户登录
> * 获取用户信息
> * 修改用户资料
> * 找回密码
> * 修改用户资料
> * 修改用户头像（拍照和本地文件）
> * 获取用户的收藏信息
> * 获取用户的历史记录

### 2.2 视频模块

> * 获取视频列表
> * 视频播放
> * 收藏视频
> * 下拉刷新列表
> * 查看收藏视频
> * 删除收藏视频

### 2.3 游戏模块（HTML5游戏）

> * 获取游戏列表
> * 打开游戏
> * 收藏游戏
> * 下拉刷新列表
> * 查看收藏游戏
> * 删除收藏游戏

### 2.4 文档模块

> * 获取文档列表
> * 打开文档
> * 收藏文档
> * 下拉刷新列表
> * 查看收藏文档
> * 删除收藏文档

## 三、系统设计

### 3.1 数据库设计

> 数据库E-R图：

![E-R图](http://7xrnl9.com1.z0.glb.clouddn.com/image%2Fmulti-media-player%2Fdatabse-E-R.png)

### 3.2 通信接口设计

通信接口格式统一，方便数据的封装以及解析，以用户登录接口（UserLogin）为例：

请求数据：

```
{
	"telephone" : "xxx",
	"token" : "",
	"requestType" : "UserLogin"
	"params" : [{
	    "password" : "xxx"
	}]
}
```

返回数据：

```
{
	"result" : "数字",
	"requestType" : "UserLogin"，
	"content"：[{
	    "id"："";
	    "user_name"："";
	    "passord"："";
	    "nickname"：""
	    ...
	}]
}
```

result的数值及含义：

> * 0：登录成功
> * 1：用户不存在
> * 2：密码错误
> * 3：登录失败

## 四、程序设计

本系统使用了[Android-Universal-Image-Loader](https://github.com/nostra13/Android-Universal-Image-Loader)图片缓存库对图片进行加载。并且在设置中可以清除图片的磁盘缓存以及内存缓存。

本项目的视频、游戏及文档资源均来自于网络，同时也提供了本地资源的支持，在侧滑菜单中有本地文件按钮，点击之后将会检索出手机中的视频及文档资源。

### 4.1 用户模块

在本App中用户无需登录即可正常使用，但是当用户执行收藏等功能时，则需要用户登录之后方可操作。

在用户登录之后，可以在`我的帐号`中查看修改帐号资料及头像。

### 4.2 视频模块

视频模块主要使用了[Vitamio](https://github.com/yixia/VitamioBundle)开源视频解析库对视频进行解析，以支持更多更全面的视频格式。

在视频播放控件中可以使用手势滑动调整手机的亮度以及声音大小。

### 4.3 游戏模块

在游戏模块中，使用WebView加载HTML5在线游戏，在之前的开发中发现不同版本的Android系统对HTML5的支持度不同，导致部分游戏无法打开。于是Google了一下，用网上的方法重写了WebView组件以使其对HTML5支持更全面。

### 4.4 文档模块

目前网络上的电子书格式有很多，本项目的文档模块支持TXT、PDF、EPUB、MOBI四种主流的电子书格式以满足用户的需求。

> 注：本模块由同事开发

## 五、界面展示

> 主界面

![主界面](http://7xrnl9.com1.z0.glb.clouddn.com/image%2Fmulti-media-player%2Fmarket-main.png)

> 左侧菜单

![左侧菜单](http://7xrnl9.com1.z0.glb.clouddn.com/image%2Fmulti-media-player%2Fleft-menu.png)

> 我的账户界面

![我的帐号界面](http://7xrnl9.com1.z0.glb.clouddn.com/image%2Fmulti-media-player%2Faccount.png)

