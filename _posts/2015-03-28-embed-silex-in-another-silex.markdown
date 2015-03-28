---
layout: post
title:  "把silex嵌入到另一个silex中"
date:   2015-03-28 15:40:45
description: silex嵌入另一个silex
permalink: post/embed-silex-in-another-silex
disqus:
  id: embed-silex-in-another-silex
categories:
- blog
- php
- silex
---

今天来点儿黑科技～如何在silex框架里嵌入另一个框架（silex本身）

由于silex的核心是基于symfony的事件驱动的http-kernel组件，所以这个看似复杂的任务就相当轻松的完成了，只需要在所有的任务都执行之后，返回Response对象即可，记住，不能调用send方法，因为这个方法真的会把内容写入到输出中，具体做了什么可以去看 `vendor/symfony/http-foundation/Symfony/Component/HttpFoundation/Response.php : public function send()`，所以我们的修改也就异常简单了，首先，需要制作一个无论怎样都要返回个Response对象的框架～，继承Silex\Application，然后重写它的run方法：

```php
<?php
namespace Rswork\Silex;

use Symfony\Component\HttpFoundation\Request;
use Silex\Application as BaseApplication;

class Application extends BaseApplication
{
   /**
     * Handles the request and delivers the response.
     *
     * @param Request|null $request Request to process
     */
    public function run(Request $request = null)
    {
        if (null === $request) {
            $request = Request::createFromGlobals();
        }

        $response = $this->handle($request);
        $this->terminate($request, $response);

        return $response;
    }
}
```

这个内嵌的silex变成了run之后只返回Response对象提供处理，而且异常等它都会自动管理，始终会返回个Response～（symfony的http-kernel实在了的，这样的结构甚至可以你随便套多少层，但是session什么的共同资源还是会容易冲突）。

写个测试的东西尝试下，看看我们的理解对不对：

```php
index.php

<?php

require_once "vendor/autoload.php";
require_once "Application.php";

$app = new Silex\Application();

Symfony\Component\Debug\Debug::enable();

$app['debug'] = true;

$app->get('/{url}', function(){
    $subapp = new Rswork\Silex\Application();
    $subapp['debug'] = true;
    $subapp->get('/', function(){return 'hello you!<br/>welcome to homepage!';});
    $subapp->get('/hello/you', function(){return 'hello you!';});

    try {
        return $subapp->run();
    } catch( \Exception $e ) {
        throw $e;
    }
})->assert('url', '.*+');

$app->run();

```

然后方便起见，直接用php内置的server， `php -S localhost:7000`，然后打开浏览器访问`http://localhost:7000`这个地址，然后再访问`http://localhost:7000/hello/you`～
