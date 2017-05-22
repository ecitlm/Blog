---
title: Node.js+Express+MySQL 操作数据库小记
date: 2017-05-21 13:04:28
comments: true
categories: Node
tags: [node,javascript]
keywords: 
description: 随著前端届技术大百花齐放, node已俨然成为前端开发中最具牛X的,里程碑式的技术之一,了掌握 node.js,已是每个前端程序员必备的技能了,近期接触 node.js操作 MySQL 数据库,进一步的加深对 node.js 的认识,随笔记录之
---

## 一、node MySQL 的安装


1.进入项目命令行安装,我这里使用的是 cnpm 安装。
```
cnpm install mysql 

```

 ![node mysql](https://dn-coding-net-production-pp.qbox.me/a4b9cf89-08e9-4cc7-a8f0-850959fc5ca8.png) 

2.在安装成功 MySQL 之后就是编写相关MySQL配置了,我写了一个mysql.js文件,node.js的mysql驱动

```
//mysql.js
var mysql = require('mysql'); //调用MySQL模块
//创建一个connection
var connection = mysql.createConnection({
    host: '127.0.0.1', //主机
    user: 'root',     //数据库用户名
    password: '123456',     //数据库密码
    port: '3306',       
    database: 'tpcms', //数据库名称
    charset: 'UTF8_GENERAL_CI' //数据库编码
});

module.exports = connection  

```
这样一个简单的数据库连接驱动就写好了,当然需要填写正确相关配置 数据库用户名、密码、数据库名称,这里要注意的是 charset 编码的问题,一开始我没有添加,返现返回回来的数据是乱码的,将 配置 exports,再其他文件就可以导入使用了


3.我现在再新建一个select.js select 操作查询数据库的文件

```
//select.js
const express = require('express');
const http = require('http');
const app = express()
var router = express.Router();
const connection = require('./sql');//导入mysq配置文件

//创建一个connection连接
connection.connect(function(err) {
    if (err) {
        console.log('[query] - :' + err);
        return;
    }
    console.log('[connection connect]  succeed!'); //如果连接成功 控制台输出 success 了
});


app.get('/', function(req, res) {
    var res = res;
    var req = req;

    //执行SQL语句,这里是一条简单的MySQL查询语句
    var sql = "select description, title,content,time from tp_post";
    connection.query(sql, function(err, rows, fields) {
        if (err) {
            console.log('[query] - :' + err);
            return;
        }
        console.log(rows)
        res.send(rows)  //这里在页面上输出数据
        console.log('The solution is: ', rows[0].solution);
    });
})
 

 module.exports = app
```
好的,select.js 文件编写好了，expor t导出，在 app.js 入口文件使用


4.编写配置好app.js入口文件了
```
const express = require('express');
const http = require('http');
const app = express()
var router = express.Router();

//配置路由 这样访问localhost:3000/select就能访问的接口了
app.use('/select', require('./api/select'))
app.use(router);
app.listen(3000);
console.log(3000);
```
ok,现在在命令行启动项目了,进入项目目录,执行 node app,项目监听的3000端口
```
node app
```
我这里是出现 启动成功,也就是输出了select.js文件里面的这一句
```
console.log('[connection connect]  succeed!'); //如果连接成功 控制台输出success了
```
![图片](https://dn-coding-net-production-pp.qbox.me/2ea0b5ff-fba4-40c0-9557-6f8bc6c784ff.png) 
运行成功了，我在浏览器上输入 访问地址 localhost:3000/mysql,在页面上返回的就是以下json 数据了

![json](https://dn-coding-net-production-pp.qbox.me/1c79ba48-7245-4f81-9a2d-8d1352974647.png) 


到这里为止，一个简单的node.js+Express 操作MySQL就完成了，一个小demo作为自己的总结记录。




文献参看
http://www.oschina.net/translate/node-mysql-tutorial?utm_source=tuicool&utm_medium=referral
















