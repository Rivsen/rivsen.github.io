---
layout: post
title:  "使用GitHub、Composer、Packagist管理公开的PHP包（Step By Step）"
date:   2015-03-15 14:00:45
description: 如何在packagist上发布php包
permalink: post/how-to-publish-package-to-packagist-using-github-and-composer-step-by-step
disqus:
  id: how-to-publish-package-to-packagist-using-github-and-composer-step-by-step
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

再来添加一个readme，要不然打开github仓库主页的时候，总会提示让你创建一个readme，有readme其实对其他小伙伴能快速了解这个包的功能有很大作用。

```text
# Hello World Package for PHP Composer #

This is a hello world package for php composer beginners tutorial.
```



### 3. 推送代码 ###

到此为止我们已经完成了仓库的初始化：初始化composer描述文件，编写readme文档，接下来需要把代码推送到GitHub。

```bash
$ git add ./
$ git commit -m 'init hello world package'
$ git push origin master
```

最后一步需要加`origin master`参数的原因是空仓库是没有分支的，所以我们需要强制推送本地的master到远端的master，在这之后可以直接用`git push`命令推送而不需要加后面的参数了。

![推送到GitHub]({{ site.baseurl }}/uploads/2015/0314/github-03.png)

### 4. 发布到packagist.org ###

访问Packagist主页，确认自己已经登录，然后点击右上角大大的`Submit Package`，然后填入我们创建的仓库的地址，点击Check，然后没问题，再点击Submit。

![发布到Packagist01]({{ site.baseurl }}/uploads/2015/0314/packagist-01.png)
![发布到Packagist02]({{ site.baseurl }}/uploads/2015/0314/packagist-02.png)

这个时候我们的包已经可以提供给小伙伴们安装啦！恭喜！撒花！

那么世界上一般事情都不会这么顺利，尤其是，其实我们前面也没做什么的情况下。

提交之后，会自动跳转到我们发布的hello-world包的详情页面，可以看到上面有红红的一行警告，仔细阅读它。

所以我们知道了，还需要去配置GitHub和Packagist之间的自动更新钩子，根据向导，复制自己的packagist的api token，然后去GitHub配置好仓库的钩子服务，然后点击服务名称后面的笔图标，进去之后点Test service。如果services列表里的packagist前面是绿色的对钩，说明成功啦第一步啦！

接着我们再去packagist的hello-world包详情页面刷新，红红的警告没有啦！（如果这里还有，那么说明在GitHub创建的service填写的资料有错误，第一个是username，不是email地址，第二个是packagist api token，一定不要搞错，第三个不需要填）。

### 5. 测试安装hello-world包 ###

```bash
$ cd ~/work/github/test
$ composer require rivsen/hello-world dev-master
```

查看下test目录里，是不是已经有了我们的hello-world包了～

到这里我们竟然一行PHP都没写呢！

### 6. 添加示例代码 ###

接下来终于到可以写PHP啦！

首先编辑我们hello-world包仓库代码里的`composer.json`(别改成test里面的了哈)，加入autoload配置。

```json
{
    "name": "rivsen/hello-world",
    "description": "this is a hello world repo for composer.",
    "license": "MIT",
    "authors": [
        {
            "name": "Rivsen Tan",
            "email": "rivsen1003@gmail.com"
        }
    ],
    "autoload": {
        "psr-4": { "Rivsen\\Demo\\": "src" }
    },
    "require": {}
}
```

这里我们添加了autoload属性，并且是什么`psr-4`，这里我需要说一下，PSR-X是php-fig发布的一系列规范中的一个自动加载规范，如果想要深入了解它的其他规则，请阅读[PHP-FIG](http://www.php-fig.org/)。添加的配置代表我们定义了一个命名空间的起始目录，比如src目录里有一个Hello类文件(类名必须和文件名一致)，那么要想通过autoload访问它，必须把命名空间写成`namespace Rivsen\Demo;`。

那么我们去创建它吧！

```bash
$ cd ~/work/github/hello-world/
$ mkdir src
$ cd src/
$ touch Hello.php
```

```php
<?php
namespace Rivsen\Demo;

class Hello
{
    private $name;

    public function __construct( $name = 'World' )
    {
        $this->name = $name;
    }

    public function getName()
    {
        return $this->name;
    }

    public function hello()
    {
        return 'Hello '.$this->name.'!';
    }
}
```

在提交代码之前，我们需要自己先测试一遍我们的代码是否有问题。

```bash
$ cd ~/work/github/hello-world
$ composer install
```

然后创建一个测试php文件，引入autoload，并且实例化一个我们的Hello类。

```bash
$ touch test.php
```

```php
<?php
require_once "vendor/autoload.php";

$hello = new Rivsen\Demo\Hello();
echo $hello->hello();

echo "\n";
$hiGirl = new Rivsen\Demo\Hello('My Goddess');
echo $hiGirl->hello();
```

OK，能看到你和你的女神打招呼就说明~~女神光环加持~~成功啦！

然后需要推送这些新代码到GitHub，因为我们配置了钩子服务，所以在推送之后不久，GitHub会通知Packagist仓库有更新，然后小伙伴们就轻松的拿到你的更新的代码啦！

```bash
$ git add .
$ git commit -m 'upload test script for Hello class'
$ git push
```

## 包版本管理 ##

至此我们已经可以把自己的代码发布到线上了，但是有一个问题，别人的包都有版本号，而我们的安装需要手动指定`dev-master`要不然composer会说找不到stable版本，这里我们就需要引入版本和分支概念了，composer包的版本是来自于git的分支和tag，分支代表dev版本(除master外)，tag代表stable版本，因为正常来说，大家都是这样管理项目的版本的，所以直接无痛模式切换，自由方便。下面我们就来模拟一下如何发布轻量级的版本（相对使用私钥签署tag并发布的过程）。

```bash
$ cd ~/work/github/hello-world
$ git branch
```

我们会看到绿色的master前面有个星号，说明当前工作分支是master，那么对应的composer包的版本就是dev-master，是不是有些熟悉了。接下来我们创建一个0.1分支，作为我们0.1版本的迭代分支，并基于它提交一些代码，然后推送。

```bash
$ git checkout branch -b 0.1
```

比如我们编辑readme，添加使用示例（虽然我们在0.1分支开发和提交，但是我们一定要在特性或者bug修复的工作完成之后，合并回master）。


```bash
$ git add .
$ git commit -m 'update readme, add Hello class demo'
$ git push origin 0.1
```

因为我们创建的0.1分支在远端是不存在的，所以要指定推送到远端的0.1，GitHub会自动创建一个0.1分支，和master是一样的。这个时候我们再看看我们的hello-world包的版本，使用`composer show rivsen/hello-world`命令查看。

```text
name     : rivsen/hello-world
descrip. : this is a hello world repo for composer.
keywords : 
versions : dev-master, 0.1.x-dev
type     : library
...
```

但是只有dev是不够的，很多线上的版本依赖的都是stable，肯定不会让一个dev包上去的，这个时候，我们觉得可以发布一个稳定版本了，那么在0.1分支上，我们发布一个0.1.0版本。

```bash
$ git tag 0.1.0
$ git push --tags
```

推送成功之后，用composer命令查看一下版本信息，是不是有了一个0.1.0的stable版本了呢！到此为止，基本的发布流程就讲完啦！

有什么意见或建议，可以创建一个[issue](https://github.com/Rivsen/hello-world/issues/new)给我，我会及时回复的！

hello-world 包资源地址： https://github.com/Rivsen/hello-world
composer 包名称： rivsen/hello-world
