---
layout: post
title:  "使用GitHub、Composer、Packagist管理公开的PHP包（Step By Step）"
date:   2015-03-15 14:00:45
description: 如何在packagist上发布php包
permalink: post/how-to-publish-package-to-packagist-using-github-and-composer-step-by-step
categories:
- blog
- composer
- php
- github
- packagist
---

## 什么是Github ##
[GitHub](https://github.com/)是一个用于使用Git版本控制系统项目的共享虚拟主机服务，可以免费托管公开的源代码仓库。在继续阅读之前请先确认是否已经有注册的GitHub帐号，因为GitHub官方提供Packagist的钩子服务。如果对Git还是有所陌生，推荐阅读 Progit（[online](https://git-scm.com/book/zh/v2)/[pdf](https://progit2.s3.amazonaws.com/zh/2015-02-26-6c188/progit-zh.355.pdf)）前七章。

![GitHub 截图]({{ site.baseurl }}/uploads/2015/0314/GitHub.png)

## 什么是Composer ##
[Composer](https://getcomposer.org/)是PHP中的一个依赖管理工具， 它可以让你声明自己项目所依赖的库，然后它将会在项目中为你安装这些库。安装步骤可以到官网阅读[Getting Started](https://getcomposer.org/doc/00-intro.md)。在继续阅读之前，请确认composer已经安装并且可以使用。命令别名是`composer`，所以后续的所有如`composer xxx`均代表是执行composer命令。

![Composer 截图]({{ site.baseurl }}/uploads/2015/0314/Composer.png)

![Composer 命令]({{ site.baseurl }}/uploads/2015/0314/Composer-run.png)

## 什么是Packagist ##
[Packagist](https://packagist.org/)主要提供Composer包发布和索引，默认Composer从Packagist获取资源。在继续阅读之前，请使用你的GitHub帐号登录Packagist。

![Packagist 截图]({{ site.baseurl }}/uploads/2015/0314/Packagist.png)

## 发布流程 ##
前面我们已经准备好了GitHub帐号、Composer程序、Packagist帐号，接下来是如何通过它们发布我们的Composer包。

### 1. 创建GitHub仓库 ###
打开Github主页，点击右上角创建仓库，比如取名叫做hello-world。
![创建仓库01]({{ site.baseurl }}/uploads/2015/0314/github-01.png)
![创建仓库02]({{ site.baseurl }}/uploads/2015/0314/github-02.png)
默认仓库是空的，没有任何代码和分支（git空仓库的特性），用`git clone xxx`克隆到本地。

```bash
$ cd ~/work/github/
$ git clone git@github.com:Rivsen/hello-world.git
```

### 2. 初始化 composer.json ###

```bash
$ cd ~/work/github/hello-world/
$ composer init
```

使用composer自带的初始化命令，创建一个composer.json描述文件。如果想手动编辑，可以去composer官网阅读[相关文档](https://getcomposer.org/doc/04-schema.md)获得帮助。
![初始化composer01]({{ site.baseurl }}/uploads/2015/0314/composer-01.png)
![初始化composer02]({{ site.baseurl }}/uploads/2015/0314/composer-02.png)
![初始化composer03]({{ site.baseurl }}/uploads/2015/0314/composer-03.png)

### 3. 推送代码 ###

### 4. 发布到packagist.org ###


## 包版本管理 ##
