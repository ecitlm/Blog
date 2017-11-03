---
title: 从0开始实现一个NBA赛事直播新闻小程序(含后台接口)
date: 2017-11-03 22:27:28
comments: true
categories: Blog
tags: [小程序,wxapp,javascript]
keywords: 
description:  从零开始实现一个NBA赛事直播新闻小程序切含后台接口的编写,接口来源是分析体育H5端的接口,使用php对接口进行抓取整理,切对接口进行了签名校验，已经实现，但没有用上小程序之中通过，接口整理一开始使用的是ThinkPHP5对接口统一整理的、后面使用PhalAPi对接口文档进行了再次的整理,整理的接口有以下,通过以下接口数据完成一个简单的大气的NBA小程序的开发

---

__写在前面的一段话__
>关于NBA、基于深刻的莫过于高中时代的，买篮球先锋报、用手机2G网络和同学凑在一起看文字直播、还生怕班主任老师发现，
印象中第一次了解NBA是在2006年的英语书上，有一页上面是所有球队的logo以及名字、那时开始慢慢的了解篮球、了解到了休斯顿火箭队大姚、
从此对火箭队情有独钟、时至今日依然对那支22连胜的火箭有太多感情、时至今日绿军三巨头、邓肯、蜗壳都已经退役了,oh~oh~oh好像有点跑题了，还是回到主题、记录花了几天时间写的这个小程序

__话不多少先上源码地址__
小程序GitHub地址: https://github.com/ecitlm/wx-nba
后端接口API地址:可先参照  https://ecitlm.github.io/TP5_Splider/#/?id=tp5_splider 
项目下面的 controller 下的Nba.php类

__部分界面效果体验__

<center>
![](https://user-gold-cdn.xitu.io/2017/11/3/6e5575af9cdfe080304e7239cff40e89)![](https://user-gold-cdn.xitu.io/2017/11/3/9891d424f55687c0ecdf8356baef72e3)
</center>

## 关于接口API
>接口来源是分析腾*体育H5端 的接口，使用php对接口进行抓取整理、切对接口进行了签名校验，已经实现，但没有用上小程序之中通过，接口整理一开始使用的是`ThinkPHP5`对接口统一整理的、后面使用`PhalAPi`对接口文档进行了再次的整理，整理的接口有以下、通过以下接口数据完成一个简单的大气的NBA小程序的开发、目前小程序正处于上架申请中。

* 每日赛事直播列表接口
* 球赛直播实时详情接口
* 实时数据统计接口
* 球队进本信息接口
* 球队球员阵容名单接口
* 球员基本信息赛季数据接口
* 30只球队排名数据接口
* 篮球快讯新闻列表接口
* 新闻详情接口
* 新闻评论数据接口
在线接口系统地址 https://wxapp.it919.cn/wx/listAllApis.php
![图片](https://user-gold-cdn.xitu.io/2017/11/3/80f7e486ab3c0dce0e936cb8b9e8e05e)

## 小程序界面
  > 界面整体有十几个、包含以上接口对应的UI界面、以下界面属于应用的截图界面
  
<center>
<img src="https://user-gold-cdn.xitu.io/2017/11/3/d4d8e33f8bb69d74a618f57bbb3f07d8" width=300 /><img src="https://user-gold-cdn.xitu.io/2017/11/3/1e78f1707b1518d311f8630a8dbac160" width=300 /><img src="https://user-gold-cdn.xitu.io/2017/11/3/12646461f61d7f8e9f938f1b998d24cc" width=300 /><img src="https://user-gold-cdn.xitu.io/2017/11/3/45db5be8620ced658e35a4adc4bd65c3" width=300 /><img src="https://user-gold-cdn.xitu.io/2017/11/3/d264a8aafcccc8266d9d55f2a1fb54aa" width=300 /><img src="https://user-gold-cdn.xitu.io/2017/11/3/0ff0aea2847dc1b0a41ce0d22341ff36" width=300 /><img src="https://user-gold-cdn.xitu.io/2017/11/3/703963b267cbb71871a4ee936dc47a75" width=300 /><img src="https://user-gold-cdn.xitu.io/2017/11/3/2fe5707340136c88a82c0f17b032e0b8" width=300 /><img src="https://user-gold-cdn.xitu.io/2017/11/3/455fceb7966e6aed5b6b85638da18fab" width=300 /><img src="https://user-gold-cdn.xitu.io/2017/11/3/07a43e2416a473d3312409add56588a4" width=300 />
</center>

## 项目目录结构
> 项目目录结构如下
![](https://user-gold-cdn.xitu.io/2017/11/3/59d316149c0cbf6b6cc547cf2524fbfe)

### 网络请求的封装
网络请求使用小程序的 `wx.request`+ `promise`+`bluebird` 对接口请求方法进行封装,一些列出的代码属于项目的核心代码
`utils`目录下的`fetch.js`文件所对应的方法
```javascript
const Promise = require("./bluebird"); //为了兼容问题
/**
 * 网络请求API接口
 * @param  {String} api    api 根地址
 * @param  {String} path   请求路径
 * @param  {Objece} params 参数
 */
module.exports = function(api, path, params) {
    wx.showLoading({
        title: "加载中"
    });
    console.log(`${api}/${path}`);
    console.log(params);
    return new Promise((resolve, reject) => {
        wx.request({
            url: `${api}/${path}`,
            data: Object.assign({}, params), //如果这里需要合并签名时间戳参数时候可以这么写
            header: { "Content-Type": "json" },
            success: function(res) {
                resolve(res);
                wx.hideLoading();
            },
            fail: function(err) {
                wx.hideLoading();
                reject(err);
            }
        });
    });
};
```

所有接口的请求放在`api.js`之中
```javascript
const fetch = require("./fetch");
const API_DOMAIN = "https://api.xxx.cn/api";

/**
 * @param  {String} 接口地址   
 * @param  {Objece} params 接口参数参数
 */
function fetchApi(api, params) {
    return fetch(API_DOMAIN, api, params);
}

//NBA比赛直播
function nab_schedule(params) {
    return fetchApi("Nba/schedule", params).then(res => res.data);
}

//直播室信息
function live_detail(params) {
    return fetchApi("Nba/live_detail", params).then(res => res.data);
}

//直播内容
function live_content(params) {
    return fetchApi("Nba/live_content", params).then(res => res.data);
}

//球员技术统计
function technical_statistics(params) {
    return fetchApi("Nba/technical_statistics", params).then(res => res.data);
}
//球员详情
function player_detail(params) {
    return fetchApi("Nba/player_detail", params).then(res => res.data);
}

//联盟排名
function team_rank(params) {
    return fetchApi("Nba/team_rank", params).then(res => res.data);
}

//球队信息
function team_info(params) {
    return fetchApi("Nba/team_info", params).then(res => res.data);
}

//球队阵容
function Lineup(params) {
    return fetchApi("Nba/Lineup", params).then(res => res.data);
}

//新闻详情
function news_info(params) {
    return fetchApi("Nba/news_info", params).then(res => res.data);
}
//NBA 新闻快讯
function news_list(params) {
    return fetchApi("Nba/new_list", params).then(res => res.data);
}
//NBA新闻评论
function news_comments(params) {
    return fetchApi("Nba/news_comments", params).then(res => res.data);
}

//关于我
function website(params) {
    return fetchApi("Nba/website", params).then(res => res.data);
}
module.exports = {
    nab_schedule,
    live_detail,
    live_content,
    technical_statistics,
    player_detail,
    team_rank,
    team_info,
    Lineup,
    news_info,
    news_comments,
    news_list,
    website
};

```
## 数据渲染问题
>在对请求到的接口数据渲染的过程之中并没有遇到什么大的问题，页面布局上的事情也就没什么可讲的了，比较麻烦的事情是需要对接口返回的每个字段进行分析所对应的显示问题，这个再记录一下赛事直播界面的数据、新闻详情的数据渲染解析HTML的问题。

### 页面布局
小程序页面布局使用的单位是`rpx`,对应设计稿`750px`是最舒服的、rpx可以根据屏幕宽度进行自适应。规定屏幕宽为750rpx。如在 iPhone6 上，屏幕宽度为375px，共有750个物理像素，则750rpx = 375px = 750物理像素，1rpx = 0.5px = 1物理像素。

| 设备        | rpx换算px (屏幕宽度/750 | px换算rpx (750/屏幕宽度)  |
| ------------- |:-------------:| -----:|
| iPhone5      | 1rpx = 0.42px  | 1px = 2.34rpx|
| iPhone6     | 1rpx = 0.5px     |   1px = 2rpx |

### 数据绑定渲染wxml页面
```javascript
var app = getApp();
Page({
    data: {
        list: [],
        footer: 0  //footer 底部导航栏切换高亮所要用到的值

    },
    onLoad: function() {
        this.nab_schedule("") //初始化数据
    },
    //ajax 列表请求
    nab_schedule: function(param) {
        var that = this;
        var params = {
            date: param
        };
        app.api.nab_schedule(params)
            .then(res => {
                that.setData({
                    list: res.data.data
                });
            })
            .catch(e => {
                console.error(e)
            });

    },

    //选择日期变化请求数据
    selectDate: function(e) {
        this.nab_schedule(e.target.dataset.time);
    },
    //  点击日期组件确定事件  
    bindDateChange: function(e) {
        this.nab_schedule(e.detail.value);
    }
})
```
新闻详情页面渲染使用到了`wxParse`，能搞方便的解决渲染`HTML`转`wxml`的问题
 模板页面用`import`导入、渲染HTML
```html
 <import src="../wxParse/wxParse.wxml" />
  <view class="wxParse">
    <template is="wxParse" data="{{wxParseData:article.nodes}}" />
  </view>
</view>
```
在接口请求成功时候对`res.body`进行一个操作处理，使用起来也比较简单
```javascript
 onLoad: function (e) {

    var data = {
      docid: e.docid || "D230QPOC0005877U"
    }
    var that = this;
    app.api.news_info(data)
      .then(res => {
        console.log(res);
        that.setData({
          item: res.data
        });
        that.news_comments(data);
        if(res.data && res.data.img.length!=0){
           var replaceStr = "<img src=" + (res.data.img[0])['src'] + ">";
           res.data.body = res.data.body.replace("<!--IMG#0-->", replaceStr);
        }
        WxParse.wxParse('article', 'html', res.data.body, that, 5);
       
      })
      .catch(e => {
        console.error(e)
        var article = "文章已经删除";
        WxParse.wxParse('article', 'html', article, that, 5);
      });
  }
```
### 图片大图预览功能实现
演示：
![](https://user-gold-cdn.xitu.io/2017/11/3/0ebf3852d977ba72168cae5cd1fb3f3f)

小程序内置的图片查看放大组件`wx.previewImage`，使用该方法可以实现图片放大预览效果功能
```javascript
wx.previewImage({
      current: url, // 当前显示图片的http链接
      urls: [url] // 需要预览的图片http链接列表
    })
```


## 总结
>小程序一直处于不温不火中、在笔者这这篇归档时、小程序已经开通内嵌网页功能、整体来说小程序还是很容易上手的、重要得多是多看文档，查找相关资料、仅以此文章记录开发体验、小应用还会持续更新、有感兴趣的小伙伴欢迎交流、源代码托管在`GitHub`











