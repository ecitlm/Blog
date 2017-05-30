---
title: Linux 下安装配置node开发环境及搭建express项目小记
date: 2017-05-28 14:06:28
comments: true
categories: Node
tags: [node,javascript]
keywords: linux 安装配置node开发环境及搭建express项目小记
description: 端午假期,作为一个程序猿,还是继续写代码咯,折腾一下 在Linux下面安装 Node.js,Node.js已经成为非常广泛的Javascript运行环境,由于开发需要,我也在服务器上面部署了Node.js 开发环境

---



### 1. Node.js 包下载
先到Node.js 中文网看一下 下载包、选择对应的下载包下载 ,
 ![图片](https://dn-coding-net-production-pp.qbox.me/3abd3ebc-53b1-4c38-bc54-1eb978e60761.png) 


Linux服务器上面 是CentOS 7.2的版本 ，我这里查看一下系统信息如下：
```powershell
[root~]# cat /etc/redhat-release
CentOS Linux release 7.2.1511 (Core) 
[root~]# uname -r
3.10.0-514.6.2.el7.x86_64
[root~]# 
```
 ![图片](https://dn-coding-net-production-pp.qbox.me/a10091ed-6885-406a-861c-4b75e04108fc.png) 

我下载的是 
https://npm.taobao.org/mirrors/node/v6.10.3/node-v6.10.3-linux-x64.tar.xz
我是将包下载安装在
/usr/local/src

```powershell
 # cd /usr/local/src/
 # wget https://npm.taobao.org/mirrors/node/v6.10.3/node-v6.10.3-linux-x64.tar.xz
 # unzip  node-v6.10.3-linux-x64.tar.xz
```
开始下载安装包 下载可能需要一段时间，下载完之后，就是解压了
```powershell
# unzip  node-v6.10.3-linux-x64.tar.xz
```



### 2. 修改配置文件
>下载解压完成之后就是修改配置文件了，修改 Node 的环境变量

编辑文件  vi /etc/profile、进入etc目录找到 ，编辑profile，在文件在文件最下面修改添加如下代码
```
 # node
 export NODE_HOME=/usr/local/src/node-v6.10.3-linux-x64
 export PATH=$PATH:$NODE_HOME/bin  
 export NODE_PATH=$NODE_HOME/lib/node_modules 
```
修改后保存，在命令行输入：source /etc/profile，让配置文件生效,现在可以再命令行测试一下node 是否安装成功了
```powershell
[root@ /]# node -v
v6.10.3
[root@ /]# npm -v
3.10.10
```
提示node 6.10.3版本,npm为3.10.10版本，ok，现在 Node已经是安装好了，


### 3. 安装 express 
首先需要全局安装express，
npm install -g express-generator #需先安装express-generator
npm install -g express
```powershell
[root@ /]# npm install -g express-generator 
/usr/local/src/node-v6.10.3-linux-x64/bin/express -> /usr/local/src/node-v6.10.3-linux-x64/lib/node_modules/express-generator/bin/express-cli.js
/usr/local/src/node-v6.10.3-linux-x64/lib
└── express-generator@4.15.0 

[root@ /]# npm install -g express
/usr/local/src/node-v6.10.3-linux-x64/lib
└── express@4.15.3 

[root@iZ28zgf2eiiZ /]# 
```
 ![图片](https://dn-coding-net-production-pp.qbox.me/8751b9a1-9953-46b3-9d09-fb723f220e36.png) 
执行安装好了,可以查看一下express 的版本了,4.15.0
```powershell
[root@ /]# express --version
4.15.0
[root@ /]# 
```

### 4. 创建 express 项目
>现在可以使用express命令行创建项目了,
```powershell
[root@i node_web]# express first_express
```
我是进入到了我指定的node_web项目文件下main 执行命令行之后如下目录结构
 ![图片](https://dn-coding-net-production-pp.qbox.me/e8c1a928-a689-4a5c-b479-e2ca57f43721.png) 
按照提示说的进行安装相关依赖等
```
install dependencies:
     $ cd first_express && npm install
   run the app:
     $ DEBUG=first-express:* npm start
```

然后就可以访问你的express项目了，默认端口是3000


















