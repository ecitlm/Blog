---
title: 微信小程序相关
date: 2016-12-11 20:34:28
comments: true
categories: Blog
tags: [share]
keywords: 
description: 微信小程序的一些相关笔记。

---

## 一、通过事件进行参数传递


> 什么是事件?

1.这里是列表文本事件是视图层到逻辑层的通讯方式。

2.这里是列表文本事件可以将用户的行为反馈到逻辑层进行处理。

3.这里是列表文本事件可以绑定在组件上，当达到触发事件，就会执行逻辑层中对应的事件处理函数。
这里是列表文本 事件对象可以携带额外信息，如id, dataset, touches。

```html

 <view id="tapTest" data-hi="MINA" bindtap="tapName"> Click me! </view>
 
```


```javascript
Page({
  tapName: function(event) {
    console.log(event.target) 
  }
})


```


```javascript
  //事件处理函数
  bindViewTap: function(e) {

    var id=e.target.id;
    console.log(e.target.id);
      
   // this.data.id=id,
    console.log(id);

    wx.navigateTo({
      url: '../logs/logs?title='+id
    })
  },

```

可以看到 dataset 里面就是我们设置的data-hi="MINA"的值了。

现在我们来看下刚刚我们写的， 首先 bindtap,以bind开头的就是要给他绑定个事件，

这个事件的名字就是“=”号后面的数值就是绑定的事件名称，需要在 逻辑【js】层定义上。 

然后就是传值了，注意到的朋友可以看到 我们这里写了data-hi 和我们平时写js的传值是同一个定义方法。

这个data-*就对应事件的属性target里的dataset 值。
这里我们需要调用的话就是 event.target.dataset.hi就能取到data-hi所对应的值。








## 二、navigator 跳转url传参


```html

 <view class="btn-area">
  <navigator url="navigate?title=navigate" hover-class="navigator-hover">跳转到新页面</navigator>
  <navigator url="redirect?title=redirect" redirect hover-class="other-navigator-hover">在当前页打开</navigator>
</view>

```

>跳转页面的接收代码

*.js 跳到新页面之后在onload里面直接接收参数，接收方法也就是 e.[参数值]

```javascript

Page({
  data: {
    logs: [],
    title:""
  },
  onLoad: function (e) {
        console.log(e)
    this.setData({
       title: e.title 
    })
  }
})

```


















