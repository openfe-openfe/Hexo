---
title: Android实践系列之项目基础配置
comments: true
date: 2016-07-14 13:28:39
update: 2016-07-14 13:28:39
categories: Android
tags: ['Android', '项目实践']
---

最近想要写一个Android的实践开发项目，一来可以把自己学习的新技术、新知识加以实践；二来可以将自己以往的技术点加以整理，总结，优化；三来希望可以与更多的人分享，交流Android知识与技术。

这个【Android实践系列】将会作为一个系列去写，其中将会涉及到我以往的技术总结，还将会把我学习的新技术应用到项目中。

<!-- more -->

## 一、项目介绍

* 项目名：AndroidDemo
* 开发环境：Android Studio 2.1.2
* 最小SDK版本：9
* 目标SDK版本：23
* Github地址：[https://github.com/dkylin/AndroidDemo](https://github.com/dkylin/AndroidDemo)

## 二、基础配置

在进行项目的开发之前，需要先加入一些第三方开源库的依赖。接下来将会一一进行介绍。

### 2.1 [RxJava](https://github.com/ReactiveX/RxJava)

想要添加RxJava到你的项目中，只需要在文件`app/build.gradle`中添加如下依赖：

```
dependencies {
    // RxJava
    compile 'io.reactivex:rxjava:1.1.3'
    compile 'io.reactivex:rxandroid:1.1.0'
}

```

添加RxAndroid是为了Android中的线程问题。添加完毕之后点击`Sync Now`同步一下就OK了。

RxJava的本质可以概括为`异步`两个字。那么同为异步，它的优势又有哪些呢？也是两个字：`简洁`。这里的简洁不是指代码的简洁，而是逻辑上的简洁。如果想知道更多关于RxJava的信息，请自行查找资料，在此不再赘述。

* RxJava的Github地址:[https://github.com/ReactiveX/RxJava](https://github.com/ReactiveX/RxJava)
* 推荐一篇介绍RxJava的文章：[给 Android 开发者的 RxJava 详解](http://gank.io/post/560e15be2dca930e00da1083)。

### 2.2 [Retrofit](http://square.github.io/retrofit/)

如果你后台的接口符合RESTful API规范，那么使用RxJava + Retrofit 完全可以打出成吨的伤害，简直不要太酸爽。

将Retrofit添加到项目中只需要在`app/build.gradle`加入如下依赖：

```
dependencies {
    // Retrofit & adapter-rxjava & converter-gson(三个版本需要相同)
    compile 'com.squareup.retrofit2:retrofit:2.0.2'
    compile 'com.squareup.retrofit2:converter-gson:2.0.2'
    compile 'com.squareup.retrofit2:adapter-rxjava:2.0.2'
    compile 'com.google.code.gson:gson:2.6.2'
}
```

在添加Retrofit开源库的同时需要添加adapter-rxjava与RxJava结合，添加gson库的支持，用于解析json数据。Retrofit 2.0已经将OkHttp集成进去了，所以就不需要单独导入OkHttp了。

* Retrofit官网：[http://square.github.io/retrofit/](http://square.github.io/retrofit/)
* Github地址：[https://github.com/square/retrofit](https://github.com/square/retrofit)
* 推荐文章：[RxJava与Retrofit结合的最佳实践](http://gank.io/post/56e80c2c677659311bed9841)

### 2.3 [Glide](https://github.com/bumptech/glide)

Glide是一个图片加载库，一个使用非常简单的图片加载库，使用方便，功能强大。

将Glide添加到项目中只需要在`app/build.gradle`加入如下依赖：

```
dependencies {
    // Glide图片加载库
    compile 'com.github.bumptech.glide:glide:3.7.0'
}
```

* Github地址：[https://github.com/bumptech/glide](https://github.com/bumptech/glide)
* 推荐文章：[Picasso vs Imageloader vs Fresco vs Glide](http://stackoverflow.com/questions/29363321/picasso-v-s-imageloader-v-s-fresco-vs-glide)

### 2.4 [ButterKnife](https://github.com/JakeWharton/butterknife)

ButterKnife是使用注解的方式帮助开发者绑定View组件，让开发者不需要再写一大串的findViewById。配合Android studio插件[android-butterknife-zelezny](https://github.com/avast/android-butterknife-zelezny)使用简直不要太棒！不多说，自己去感受！！！

将Glide添加到项目中首先需要在项目级的`build.gradle`中添加：

```
buildscript {
  repositories {
 	jcenter()
  }
 dependencies {
   	// 添加APT，用于自动生成代码
   	classpath 'com.neenbedankt.gradle.plugins:android-apt:1.8'
  }
}
```

然后在`app/build.gradle`加入如下依赖：

```
apply plugin: 'android-apt'

android {
  ...
}
dependencies {
  // ButterKnife
  compile 'com.jakewharton:butterknife:8.2.1'
  apt 'com.jakewharton:butterknife-compiler:8.2.1'
}
```

* Github地址：[https://github.com/JakeWharton/butterknife](https://github.com/JakeWharton/butterknife)

### 2.5 [Logger](https://github.com/orhanobut/logger)

Logger是一个非常好用的日志工具。使用也非常简单，在此不再赘述。在`app/build.gradle`加入如下依赖：

```
dependencies {
    // Logger
    compile 'com.orhanobut:logger:1.15'
}
```

* Github地址：[https://github.com/orhanobut/logger](https://github.com/orhanobut/logger)

### 2.6 [LeakCanary](https://github.com/square/leakcanary)

LeakCanary是很常用的一个内存泄漏检测工具，使用非常简单，可以很好的检测出应用的内存泄漏。在`app/build.gradle`加入如下依赖：

```
dependencies {
    // LeakCanary内存泄漏检测
    debugCompile 'com.squareup.leakcanary:leakcanary-android:1.4-beta2'
    releaseCompile 'com.squareup.leakcanary:leakcanary-android-no-op:1.4-beta2'
    testCompile 'com.squareup.leakcanary:leakcanary-android-no-op:1.4-beta2'
}
```

Github地址：[https://github.com/square/leakcanary](https://github.com/square/leakcanary)

## 结语

本文仅是介绍了项目中所使用的一些开源库以及简单的介绍。在后面的文章将会对这些开源库的使用进行详细的介绍。