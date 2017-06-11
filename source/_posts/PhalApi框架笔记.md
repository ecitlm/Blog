---
title: PhapApi框架使用小记
date: 2017-06-11 18:17:28
comments: true
categories: php
tags: [php]
keywords: 
description: PhapApi框架使用小记,PhalApi是一个PHP轻量级开源接口框架，致力于快速开发接口服务。 支持HTTP/SOAP/RPC等协议，可用于搭建接口/微服务/RESTful接口/Web Services

---

##PhalApi框架使用笔记

### 什么是PhapApi
>[PhalApi](https://github.com/phalapi/phalapi/)是一个PHP轻量级开源接口框架，致力于快速开发接口服务。 支持HTTP/SOAP/RPC等协议，可用于搭建接口/微服务/RESTful接口/Web Services，

```
.
│
├── PhalApi         //PhalApi框架，后期可以整包升级
│
│
├── Public          //对外访问目录，建议隐藏PHP实现
│   └── demo        //Demo服务访问入口
│
│
├── Config          //项目接口公共配置，主要有：app.php, sys.php, dbs.php
├── Data            //项目接口公共数据
├── Language        //项目接口公共翻译
├── Runtime         //项目接口运行文件目录，用于存放日记，可软链到别的区
│
│
└── Demo            //应用接口服务，名称自取，可多组
    ├── Api             //接口响应层
    ├── Domain          //接口领域层
    ├── Model           //接口持久层
    └── Tests           //接口单元测试
```


### PhalApi核心思想
>核心思想：DI依赖注入

###PhalApi框架三层结构Api+Domain+Model模式

1.1 Api层
它会通过控制器把请求转发到service层作处理，并将处理结果在页面展示，所以Api更像担当控制器(C)的作用。

1.2 Domain层

Domain层主要负责的是具体的业务实现,如数据获取，一个Domain方法就是一个小的业务具体实现(注意尽量把业务划分得细一点方便通用)

1.3 Model层

数据库操作单独提炼出来统一处理

### PhalApi通用公共函数和 拦截器过滤器

> 公用函数和拦截器

1 公共函数

首先在我们的根目录建立一个文件夹叫做general通用的意思,里面分别有两个文件夹Common(受TP的影响)和Intercept两个文件,我们在里面放入我们自己的文件,当然需要按照正确的规则比如Common_Base等

 然后我们要使用的话当然要注册我们这个general文件作为自带加载文件,然后注册自己需要用的类,如下在入口文件`index.php`进行注入

```
/** ---------------- 通用方法加载 ---------------- **/
//加载项目通用文件
DI()->loader->addDirs('General');
//通用函数基础类
DI()->base = new Common_Base();

```

2 拦截器
PhalApi已经有自带的拦截器，使用时一样需要先注入
 ![PhalApi结构](https://dn-coding-net-production-pp.qbox.me/6b117d2e-ce35-47f8-b084-3c6b15ee4d13.png) 

>加入我们需要对`token`进行校验，一些方法是需要验证，有些方法又是不需要验证 `token`.我们需要怎么实现呢?

可以在项目目录 `Config/app.php`进行定义一个数组，数组包括的是需要验证`token`的类方法名

```
   /**
     * 需要带Token的接口
     */
    apiTokenRules' => array(
        'User.info',
        'User.updateInfo',
    )
```
我们定义好相关需要校验`token`的数组，在 Filter下面的 `SimpleToken` 来进行校验,所有的接口访问都会走改注册了的token校验方法,我们就需要在校验方法里面做过滤拦截,
```
public function check() {
        $service = DI()->request->get('service');
        $app=DI()->config->get('app');
        $app = json_decode(json_encode($app),true);
        $apiTokenRules = $app['apiTokenRules'];
        if (in_array($service,$apiTokenRules)) {
            $allParams = DI()->request->getAll();
            $token = isset($allParams['token']) ? $allParams['token'] : '';
            $user_id = $allParams['user_id'];
            if (empty($user_id)){
                throw new PhalApi_Exception_BadRequest('缺少必要参数user_id');
            }
            $service_token = DI()->cache->get($user_id.'token');
            if (empty($service_token)){
                throw new PhalApi_Exception_BadRequest('请重新登录',99);
            }else{
                if (strcmp($token,$service_token) !== 0){
                    DI()->logger->debug('Wrong Token', array('needToken' => $service_token));
                    throw new PhalApi_Exception_BadRequest('Token错误，请重新登录',99);
                }else{
                    DI()->cache->set($user_id.'token',$service_token,24*60*60);
                }
            }
        }
    }
```


### PhalApi 第三方SDK使用集成方法
>参考大神的OSC@GIT仓库  https://github.com/phalapi/phalapi-library ，仓库有比较多的SDK包 供下载使用 `git clone https://github.com/phalapi/phalapi-library.git`

以短信SMS 容联云短信服务器拓展为例
>配置方式非常简单只需要把拓展下载下来放入Library文件内即可,然后就可以使用如下方法进行实例,文件名称以`Lite.php` 为准,再到根目录进行注入使用,

```
DI()->sms= new SMS_Lite();
```



参看PhapApi
https://github.com/phalapi/phalapi/
https://www.phalapi.net/






