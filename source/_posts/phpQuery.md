---
title: ThinkPHP5 集成扩展 phpQuery用法总结
date: 2017-05-30 14:34:28
comments: true
categories: php
tags: [php,ThinkPHP]
keywords: ThinkPHP5 集成扩展 phpQuery 用法总结
description: 假期闲来无事,宅在家中,突然想到php里面如何去使用操作DOM爬去自己想要的数据返回给前端了,经过一番搜索找到了这个phpQuery扩展,于是打算集成到ThinkPHP5项目中去使用,遂记录之,集成扩展 phpQuery用法总结

---


## 什么是phpQuery？
>[phpQuery](http://code.google.com/p/phpquery/)是一个类似于Jquery，能够在 PHP里面操作 HTML DOM节点的一个扩展库，能够灵活的对页面数据进行采集、爬虫获得自己想要的数据、文件下载地址：http://code.google.com/p/phpquery/


###   phpQuery的使用介绍

>如下代码 使用 `\phpQuery::newDocumentHTML($data)` 得到 HTML 的 DOM 树，就可以像Jquery一样操作HTML节点了
```php
 $filePath = 'http://www.xxxx.com/';
 $data=Http_Spider($filePath);
 \phpQuery::newDocumentHTML($data);
 //\phpQuery::newDocumentFile($data);
 $arr = array();
 
 $list = pq('#picture')->find("a");
 foreach ($list as $li) {
	 $title = pq($li)->attr('title');
	 $url = pq($li)->attr('href');
	 $img = pq($li)->find('img')->attr('src');
	 $tmp = array([
		 'title' => $title,
		 'url' => $url,
		 'img' => $img
		 ]);
      array_push($arr, $tmp);
     }
```
###  载入文档（loading documents）

>加载文档主要通过phpQuery::newDocument来进行操作，其作用是使得phpQuery可以在服务器预先读取到指定的文件或文本内容。
主要的方法包括：

```php
phpQuery::newDocument(html,html,contentType = null)

phpQuery::newDocumentFile(file,file,contentType = null)

phpQuery::newDocumentHTML(html,html,charset = 'utf-8')

phpQuery::newDocumentXHTML(html,html,charset = 'utf-8')

phpQuery::newDocumentXML(html,html,charset = 'utf-8')

phpQuery::newDocumentPHP(html,html,contentType = null)

phpQuery::newDocumentFileHTML(file,file,charset = 'utf-8')

phpQuery::newDocumentFileXHTML(file,file,charset = 'utf-8')

phpQuery::newDocumentFileXML(file,file,charset = 'utf-8')

phpQuery::newDocumentFilePHP(file,file,contentType) 
 
```


## 将phpQuery扩展集成到 ThinkPHP5 
>我们下载phpQuery包 然后将包复制到 Thinkphp项目的根目录 vendor（第三方类库目录）文件夹里面，如下
 ![图片](https://dn-coding-net-production-pp.qbox.me/695499db-0daf-46cf-a8c1-ba2e0216b8e5.png) 
 
然后还有一个文件 `QueryList`，QueryList作为一个类方法使用，在这个类方法里面引入扩展，
```php
Vendor('phpQuery.phpQuery');
class QueryList{
}
```

接下来就是如何在控制器方法里面使用这个扩展

```php
include('QueryList.php');

 public function splider()
    {
     $filePath = 'http://www.xxx.com/';
      $data=Http_Spider($filePath);
      \phpQuery::newDocumentHTML($data);

    }
```
接下来就可以像JQuery，一样舒服的使用了