---
layout: post
title:  "jquery 获取使用namespace绑定的事件"
date:   2014-01-07 15:42:05
description: "jquery的event前段时间了解了下，发现namespace方式绑定的自定义事件超级好用，支持单个触发或者根据事件名称触发一系列绑定在这个事件上的namespace里的handler，前提是，如何判断绑定事件存在，并触发它。"
permalink: post/jquery-get-custom-events-namespace-and-trigger-it
disqus:
  id: jquery-get-custom-events-namespace-and-trigger-it
categories:
- blog
- jquery
---

jquery的event前段时间了解了下，发现namespace方式绑定的自定义事件超级好用，支持单个触发或者根据事件名称触发一系列绑定在这个事件上的namespace里的handler，前提是，如何判断绑定事件存在，并触发它。

经过在Google搜索，在stackoverflow上找到了骨头！详细回答请 [戳这里](http://stackoverflow.com/questions/14072042/how-to-check-if-element-has-click-handler)～

简单讲，就是需要结合jquery 1.8以上版本([1.8 release log](http://blog.jquery.com/2012/08/09/jquery-1-8-released/))，利用 \_data() 方法获取到event列表，并进行进一步的处理。

下面是写的一段测试代码：

```javascript
// add a check string start with something
String.prototype.startWith = function(needle){
    return(this.indexOf(needle) == 0);
}

// set a app object
var app = $('#page');

// add a custom event handler
app.on('slider_preload.before', function(event){
        console.log('before slider pre-load event');
        });

// add another custom event handler
app.on('common.init', function(event){
        console.log('init event handler in common events');
        });

// get events list
var app_events = $._data( app.get(0), 'events' );

// trigger common init event
if( undefined !== app_events && undefined !== app_events.common ){
    app.trigger('common.init');
}

if( undefined !== app_events && undefined !== app_events.slider_preload ) {
    app.trigger('slider_preload');
}
```
