---
layout: post
title:  "PHP PSR-0 Autoloading Standard – 简体中文"
date:   2014-08-30 08:50:45
description: "PHP PSR-0 Autoloading Standard – 简体中文"
permalink: post/php-psr-0-autoliading-stangard-zh-cn
disqus:
  id: php-psr-0-autoliading-stangard-zh-cn
categories:
- blog
- php
---

这个建议标准( [PSR-0](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md) )主要是提供在编写 autoload 时，其文件、类(class)以及命名空间(namespace)在代码中的约定，说白了，就是规范了怎么放(命名空间),类的名称以及其源文件名如何定义，请大家使用有遵循这个规则的框架或系统时，也能够有个最低限度的共用标准。

本篇是翻译自一份繁体中文版的博文，有些字词或者句子为了兼顾“天朝”的名词习惯而有所修改，如果有觉得和原意不符的地方，请提出指正，谢谢。原文链接：[PSR-0 Autoloading Standard](http://blog.mosil.biz/2012/08/psr-0-autoloading-standard/)。

下面将描述一个具有互用性的“自动加载(autoloader)”所需要遵守的条件。

## 必要条件 ##

    1. 一个完全合格的 namespace(命名空间)与class(类)需要符合这样的结构 ```\<Vendor Name>\(<Namespace>\)*<Class Name> ```。
    2. 每个 namespace 需要有一个顶层的命名空间(“提供者名称 Vendor Name”)。
    3. 如果需要的话，每个 namespace 都可以有多个子命名空间。
    4. 当 namespace 如果是从文件系统加载时，其使用的分隔符都要转换成 ```DIRECTORY_SEPARATOR``` 。
    5. 类名称(class name)中，每个下划线(‘_’)都要转换成 ```DIRECTORY_SEPARATOR``` 因为下划线在 namespace 中是没有意义的。
    6. 从文件系统载入的合格 namespace 与 class 一定是“.php”结尾。
    7. Vendors name、namespace 以及 class name 所使用的字母都可以由大小写组成。

## 示例 ##

    1. ```\Doctrine\Common\IsolatedClassLoader``` => ```/path/to/project/lib/vendor/Doctrine/Common/IsolatedClassLoader.php```
    2. ```\Symfony\Core\Request``` => ```/path/to/project/lib/vendor/Symfony/Core/Request.php```
    3. ```\Zend\Acl``` => ```/path/to/project/lib/vendor/Zend/Acl.php```
    4. ```\Zend\Mail\Message``` => ```/path/to/project/lib/vendor/Zend/Mail/Message.php```

## 命名空间与类名称中的下划线 ##

    1. ```\namespace\package\Class_Name``` => ```/path/to/project/lib/vendor/namespace/package/Class/Name.php```
    2. ```\namespace\package_name\Class_name``` => ```/path/to/project/lib/vendor/namespace/package_name/Class/Name.php```

我们设定这个标准，来使自动加载有个基本的共通性。你可以尝试遵循这个标准使用 SplClassLoader 来加载 PHP 5.3 的类。[译注：[RFC:SplClassLoader](https://wiki.php.net/rfc/splclassloader)]

## 范例代码 ##

下面示范如何实践上述标准建议的自动加载规范。

```php
<?php

function autoload($className)
{
    $className = ltrim($className, '\\');
    $fileName  = '';
    $namespace = '';
    if ($lastNsPos = strripos($className, '\\')) {
        $namespace = substr($className, 0, $lastNsPos);
        $className = substr($className, $lastNsPos + 1);
        $fileName  = str_replace('\\', DIRECTORY_SEPARATOR, $namespace) . DIRECTORY_SEPARATOR;
    }
    $fileName .= str_replace('_', DIRECTORY_SEPARATOR, $className) . '.php';

    require $fileName;
}
```

## SplClassLoader 示例 ##

下面这个 gist 链接中是SplClassLoader的范例，若是你有遵循这个标准建议来命名类名的话，你可以使用它来载入自己的类别。这也是目前 PHP5.3 所推荐的类名命名标准。

[https://gist.github.com/221634](https://gist.github.com/221634)

