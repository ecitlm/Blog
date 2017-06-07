---
title: php设置跨域问题,允许指定域名能够跨域
date: 2017-05-25 20:34:28
comments: true
categories: php
tags: [php]
keywords:
description: 这段时间使用php写了一些接口来提供给Vue.js 项目学习开发使用,写好的接口只想自己指定的域名下能够访问,所以需要怎么去做呢?

---


### php设置跨域问题
>这段时间使用php写了一些接口来提供给Vue.js 项目学习开发使用，写好的接口只想自己指定的域名下能够访问、所以需要怎么去做呢？

####  为什么会跨域、造成跨域的原因是什么

浏览器的同源策略是浏览器为安全性考虑实施的非常重要的安全策略，从一个域上加载的脚本就不能去访问另外一个域下的文档，所以就会出现下面提示不能跨域问题
```
XMLHttpRequest cannot load https://api.it919.cn/public/index.php/api/News/new_detail?postid=CM5VQ6UE0001899N. 
The 'Access-Control-Allow-Origin' header has a value 'https://code.it919.cn' that is not equal to the supplied origin. Origin 'http://192.168.1.2:800' is therefore not allowed access.
```

1、允许单个域名访问，也就是指定某个域名能跨域请求php接口
```
header('content-type:application:json;charset=utf8');  
header('Access-Control-Allow-Origin:https://code.it919.cn');
header('Access-Control-Allow-Methods:*');  
header('Access-Control-Allow-Headers:x-requested-with,content-type'); 
```
主要设置的还是
header('Access-Control-Allow-Origin:`https://code.it919.cn`');将其设置为自己跨域的域名，如果允许全部访问的话 设置 `*`
```
header('Access-Control-Allow-Origin:*');
```
ok这样就能够 访问接口数据不跨域了

2、允许多个域名能够跨域访问接口数据
指定多个域名（https://code.it919.cn、http://www.it919.cn）跨域访问，这样的话就配置一个数组包含允许跨域的域名载这里面
```
$allow_origin = array(  
    'https://code.it919.cn',
    'http://www.it919.cn'  
);  
```

获取需要访问接口数据的域名
```
$origin = isset($_SERVER['HTTP_ORIGIN'])? $_SERVER['HTTP_ORIGIN'] : '';  
```
判断该域名是否是在我们定义好的数组里面
```
if(in_array($origin, $allow_origin)){  
    header('Access-Control-Allow-Origin:'.$origin);       
} 
```

