---
title: 移动端rem适配方案
date: 2017-03-11 20:34:28
comments: true
categories: Blog
tags: [css,javascript]
keywords: 
description: 移动手机端界面写了挺多,倒没有认真想过适配方案的问题,今天记录一下移动端适配方案-媒体查询方案,淘宝手淘移动端JS适配方案

---

## 选择 rem 做移动端适配的原理


> 什么是 rem 呢? `rem` 是在W3C上面的解析——“font size of the root element”,它是根据根元素'html'的大小变化而变化的,HTML默认的`font-szie`为`16px`


##媒体查询方式适配
>媒体查询方式适配就是根据 `media screen`大小的变化浏览器来自动选择对应的适配代码,将html设置10px
```
html{font-size:10px}
@media screen and (min-width:321px) and (max-width:375px){html{font-size:11px}}
@media screen and (min-width:376px) and (max-width:414px){html{font-size:12px}}
@media screen and (min-width:415px) and (max-width:639px){html{font-size:15px}}
@media screen and (min-width:640px) and (max-width:719px){html{font-size:20px}}
@media screen and (min-width:720px) and (max-width:749px){html{font-size:22.5px}}
@media screen and (min-width:750px) and (max-width:799px){html{font-size:23.5px}}
@media screen and (min-width:800px){html{font-size:25px}}

```

##根据JavaScript来改变根字体的大小

```
;(function(designWidth, maxWidth) {
    var doc = document,
        win = window;
    var docEl = doc.documentElement;
    var tid;
    var rootItem, rootStyle;

    function refreshRem() {
        var width = docEl.getBoundingClientRect().width;
        var dpr = window.devicePixelRatio || 1;
        docEl.setAttribute('data-dpr', dpr);
        if (!maxWidth) {
            maxWidth = 750;
        };
        if (width > maxWidth) {
            width = maxWidth;
        }
        //与淘宝做法不同，直接采用简单的rem换算方法1rem=100px
        var rem = width * 100 / designWidth;
        //兼容UC开始
        rootStyle = "html{font-size:" + rem + 'px !important}';
        rootItem = document.getElementById('rootsize') || document.createElement("style");
        if (!document.getElementById('rootsize')) {
            document.getElementsByTagName("head")[0].appendChild(rootItem);
            rootItem.id = 'rootsize';
        }
        if (rootItem.styleSheet) {
            rootItem.styleSheet.disabled || (rootItem.styleSheet.cssText = rootStyle)
        } else {
            try {
                rootItem.innerHTML = rootStyle
            } catch (f) {
                rootItem.innerText = rootStyle
            }
        }
        //兼容UC结束
        docEl.style.fontSize = rem + "px";
    };
    refreshRem();

    win.addEventListener("resize", function() {
        clearTimeout(tid); //防止执行两次
        tid = setTimeout(refreshRem, 300);
    }, false);

    win.addEventListener("pageshow", function(e) {
        if (e.persisted) { // 浏览器后退的时候重新计算
            clearTimeout(tid);
            tid = setTimeout(refreshRem, 300);
        }
    }, false);

    if (doc.readyState === "complete") {
        doc.body.style.fontSize = "16px";
    } else {
        doc.addEventListener("DOMContentLoaded", function(e) {
            doc.body.style.fontSize = "16px";

        }, false);
    }
})(750, 768);
```
上面核心的几行代码
```
var width = docEl.getBoundingClientRect().width; //获取移动设备的宽度
var rem = width * 100 / designWidth;  //rem的值等于(设备的宽度)x100/(设计稿宽度)
docEl.style.fontSize = rem + "px";   //设置html的字号为第二行的值
```
假如设计稿的大小实际是750px的设计稿,计算出来根字体为50px,Iphone 实际是375px的宽度,计算就是7.5rem的宽度

阿里团队开源的一个库
https://github.com/amfe/lib-flexible
















