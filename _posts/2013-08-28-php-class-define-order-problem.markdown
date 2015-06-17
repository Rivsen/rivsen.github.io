---
layout: post
title:  "php的一个类定义先后顺序的问题"
date:   2013-11-20 06:07:15
description: "php的一个类定义先后顺序的问题"
permalink: post/php-class-define-order-problem
disqus:
  id: php-class-define-order-problem
categories:
- blog
- php
---

今天在玩 symfony 的 event-dispatcher 的时候遇到了个奇怪的问题：
我定义的类无法找到，它继承了Event，可是去掉继承语句却能正常调用。

```php
<?php

namespace
{
    require_once "vendor/autoload.php";
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Symfony\Component\EventDispatcher\Event;

    $a = new Test\a();
    var_dump($a->getName());
}

namespace Test
{
    use Symfony\Component\EventDispatcher\Event;

    class a extends Event
    {
        public function getName()
        {
            return 'ger';
        }
    }
}
```

后来在qq群、irc上提问了，然后回复说我应该先定义这个namespace再使用，尤其是在单个文件中。

本来php平时用的时候是可以先调用后定义的，不过貌似php还没做到那么完整。于是我找到了一些有意思的博客，其中一个就是类的继承问题，其实这个类未定义和命名空间无关，而是和类的定义顺序有关，相关链接：[点这里](http://blog.leezhong.com/tech/2011/06/15/be-careful-with-php-extends.html) 。

```php
<?php

class A extends B {} class B extends C {} class C {}
```

上边这段代码会提示类B未找到，但是把B和C的定义顺序调换，就可以正常运行了。

```php
<?php

class A extends B {} class C {} class B extends C {}
```

上边的博客中给了个php.net上的链接，感兴趣的可以去看看： [extends](http://php.net/manual/en/keyword.extends.php) 。
