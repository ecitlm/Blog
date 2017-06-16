---
title: Sublime_text3 实用插件整理
date: 2017-06-16 23:04:28
comments: true
categories: 
tags: [js]
keywords: 
description: 实用插件整理录一些自己在使用sublime时常用的一些插件:cssrem , SublimeServer , FileHeader ,OmniMarkupPreviewer , sublime-jsdocs  , AutoFileName,SublimeText-Nodejs , Sublime-Better-Completion , SublimeAllAutocomplete,JsFormat ,jQuery等等插件扩展介绍

---


###  Sublime text 常用插件

>记录一些自己在使用sublime时常用的一些插件 cssrem 、SublimeServer 、 FileHeader 、OmniMarkupPreviewer 、sublime-jsdocs  、AutoFileName、SublimeText-Nodejs 、Sublime-Better-Completion 、SublimeAllAutocomplete 、JsFormat 、jQuery

__安装插件__

>需要先Package install安装

按Ctrl+`调出console 复制代码运行

```

importurllib.request,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();urllib.request.install_opener(urllib.request.build_opener(urllib.request.ProxyHandler()));open(os.path.join(ipp,pf),'wb').write(urllib.request.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read())

```

#### cssrem

> 一个CSS的px值转rem值的Sublime Text 3自动完成插件。[下载地址](https://github.com/flashlizi/cssrem) https://github.com/flashlizi/cssrem

插件效果如下：

![效果演示图](http://upload-images.jianshu.io/upload_images/1838578-43f835019408ae5d.gif?imageMogr2/auto-orient/strip)

###### 安装

* 下载本项目，比如：git clone https://github.com/flashlizi/cssrem

* 进入packages目录：Sublime Text -> Preferences -> Browse Packages...

* 复制下载的cssrem目录到刚才的packges目录里。

* 重启Sublime Text。

###### 配置参数

参数配置文件：Sublime Text -> Preferences -> Package Settings -> cssrem

* `px_to_rem` - px转rem的单位比例，默认为40。

* `max_rem_fraction_length` - px转rem的小数部分的最大长度。默认为6。

* `available_file_types` - 启用此插件的文件类型。默认为：[".css", ".less", ".sass"]。

#### SublimeServer

>静态WEB服务器：SublimeServer [GitHub地址](https://github.com/learning/SublimeServer)

#### FileHeader

>快速新建文件、并生产头部注释    [GitHub地址](https://github.com/shiyanhui/FileHeader)

![效果演示图](http://upload-images.jianshu.io/upload_images/1838578-a2f3b7b8c148b861.gif?imageMogr2/auto-orient/strip)

#### OmniMarkupPreviewer

>为 Sublime Text 的一款强大插件，支持将标记语言(Markdown仅是其中一种)渲染为 HTML 并在浏览器上实时预览，同时支持导出 HTML 源码文件  [GitHub地址](https://github.com/timonwong/OmniMarkupPreviewer)

#### sublime-jsdocs

> 这个插件可以很好的生成js ,php 等语言函数注释,只需要在函数上面输入/** ,然后按tab 就会自动生成注释 [GitHub地址](https://github.com/spadgos/sublime-jsdocs)

#### AutoFileName

>自动提示路径插件  [GitHub地址](https://github.com/BoundInCode/AutoFileName)

#### SublimeText-Nodejs

> 基于sublime text3的node.js开发环境搭建  [GitHub地址](https://github.com/tanepiper/SublimeText-Nodejs)

#### Sublime-Better-Completion

>支持Javascript、JQuery、Twitter Bootstrap框架、HTML5标签属性提示的插件  [GitHub地址](https://github.com/Pleasurazy/Sublime-Better-Completion)

#### AllAutocomplete

>Sublime Text 默认的 Autocomplete 功能只考虑当前的文件，而 AllAutocomplete 插件会搜索所有打开的文件来寻找匹配的提示词。  [GitHub地址](https://github.com/alienhard/SublimeAllAutocomplete)

![效果演示图](http://upload-images.jianshu.io/upload_images/1838578-b7ea09d7e92c8074.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### JsFormat

>js格式化插件  [GitHub地址](https://github.com/jdc0589/JsFormat)

>使用方法：

1、快捷键：`ctrl+alt+f`

2、先用快捷键打开命令面板 “`ctrl + shift + p`”, 再输入 “`Format: Javascript`” 就可以使用格式化命令

#### jQuery

>jQuery 提示插件



#### ConvertToUTF8
>sublime text本身是不支持中文编码的，所以需要通过安装插件来解决，ConvertToUTF8插件可以实现。


#### SublimeTmpl
>能够很好的新建文件时使用模版的内容了, 目前添加了html/js/css/php/python/ruby的模版. 
```
ctrl+alt+h html
ctrl+alt+j javascript
ctrl+alt+c css
ctrl+alt+p php
ctrl+alt+r ruby
ctrl+alt++shift+p python
```

#### BracketHighlighter
>BracketHighlighter 插件能为Sublime Text提供括号，引号这类高亮功能  [GitHub地址](https://github.com/facelessuser/BracketHighlighter)

1. 在Sublime Text中用package control安装 BracketHighlighter ；

2. 安装完成后，打开Preferences -> package settings -> Bracket Highlighter -> Bracket Settings – User，然后添加如下代码：
```
{
	"bracket_styles": {
		"default": {
			"icon": "dot",
			// "color": "entity.name.class",
			"color": "brackethighlighter.default",
			"style": "highlight"
		},
		"unmatched": {
			"icon": "question",
			"color": "brackethighlighter.unmatched",
			"style": "highlight"
		},
		"curly": {
			"icon": "curly_bracket",
			"color": "brackethighlighter.curly",
			"style": "highlight"
		},
		"round": {
			"icon": "round_bracket",
			"color": "brackethighlighter.round",
			"style": "highlight"
		},
		"square": {
			"icon": "square_bracket",
			"color": "brackethighlighter.square",
			"style": "highlight"
		},
		"angle": {
			"icon": "angle_bracket",
			"color": "brackethighlighter.angle",
			"style": "highlight"
		},
		"tag": {
			"icon": "tag",
			"color": "brackethighlighter.tag",
			"style": "highlight"
		},
		"single_quote": {
			"icon": "single_quote",
			"color": "brackethighlighter.quote",
			"style": "highlight"
		},
		"double_quote": {
			"icon": "double_quote",
			"color": "brackethighlighter.quote",
			"style": "highlight"
		},
		"regex": {
			"icon": "regex",
			"color": "brackethighlighter.quote",
			"style": "outline"
		}
	}
}
```

#### Alignment （代码对齐）
>单和易于使用的插件,使你的代码组织和美观。当您重温代码时候非常有用。
使用方法：选中要调整的行，然后按 `Ctrl+ Alt + A` 一些对应的设置可以参看[配置](https://my.oschina.net/shede333/blog/170536)

#### SideBar Enhancements　　
>这个插件改进了侧边栏，增加了许多功能

#### SublimeLinter
>使用SublimeLinter配置JS,CSS,HTML语法检查 可参看   [配置](https://segmentfault.com/a/1190000004169261)



#### Vue.js 文件代码高亮支持
>[让sublime text3支持Vue语法高亮显示](https://github.com/vuejs/vue-syntax-highlight)

#### sublime 支持PHP语法提示
>https://github.com/benmatselby/sublime-phpcs




#### 主题插件
>自己比较喜欢的主题

__Theme - itg.flat__
https://github.com/itsthatguy/theme-itg-flat

https://github.com/voronianski/oceanic-next-color-scheme

https://github.com/babel/babel-sublime

查看20款sublime [主题](http://www.itbulu.com/20-sublime-themes.html)

[PackageControl官网地址：](https://packagecontrol.io/packages/CodeFormatter)










