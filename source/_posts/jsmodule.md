---
title: 深入 JavaScript 模块化模式详解
layout: post
date: 2016-11-29 14:42:55
categories: javascript
tags: [javascript]
keywords: javascript
description: 模块化是 JavaScript 中很常见的模式，这个模式已经被广泛的理解并接受了但是它仍然有一些没有引起足够重视的高级用法。在这篇文章中，我们回顾模块化的基础知识并覆盖一些需要引起足够注意力的高级主题。

---
## 译文：

模块化是 JavaScript 中很常见的模式，这个模式已经被广泛的理解并接受了，但是它仍然有一些没有引起足够重视的高级用法。
在这篇文章中，我会回顾模块化的基础知识并覆盖一些需要引起足够注意力的高级主题，其中甚至有一个是我自己原创的。

## 基础知识

我们从一些简单的回顾开始，这些知识在 Eric Miraglia（YUI 作者）在几年前写了关于 [JavaScript 模块化的博客](http://yuiblog.com/blog/2007/06/12/module-pattern/) 之后就人尽皆知了。如果你已经非常熟悉 JavaScript 模块化，那么请直接跳至“高级模式”部分。

### 匿名闭包

这是 JavaScript 模块化得以实现的基础，也是 JavaScript 最佳的特性，没有之一。我们只需要创建一个匿名函数，并立刻执行它，
这个匿名函数中运行的所有的代码就会存活于一个闭包之中，并在应用程序的生命周期内提供私密性和状态。



```
(function () {
    // 所有的变量和函数都只属于本作用域
	// 但仍然维护着对全局作用域的访问
}());
```


请注意那个包围在匿名函数外的`( )`，为了满足 JavaScript 语法的正确性，我们必须添加它，因为以 function 开头的语句总是被认为是函数声明，被 `( )` 括起来以后函数声明则变成了函数表达式。

### 全局导入

JavaScript 有一个特性叫做“隐含全局”。任何时候，只要有被使用的变量名，JavaScript 解释器就会沿着作用域链向上查找一个定义了该变量名的 var 声明。如果在这个过程中没有找到这个变量声明，那这个变量就假定是全局的。而如果这个变量是被赋值而且此时这个变量尚不存在，那么这个变量会被创建为全局变量，这也意味着在匿名闭包中可以很容易的创建全局变量。不过，很不幸，这样会导致难以维护的代码，因为对人来说，很难区分在一个源文件中那些变量是全局的。

幸好，匿名函数为这种情况提供了一个简单的解决方案，我们可以把全局变量作为参数传入匿名函数，从而把他们的导入我们的代码，这样既清晰，相比“默认全局”又可以提高性能，来看一个例子：



```
(function ($, YAHOO) {
	// 现在我们可以在匿名函数中用$访问 jQuery，用 YAHOO 访问全局变量的 YAHOO 了。
}(jQuery, YAHOO));
```



### 模块导出

有时候你声明变量的时候并不想使用全局变量，而是局部变量，这时候你可以通过把变量当做匿名函数的返回值导出来达到目的。这也就是基本的JavaScript 模块模式，来看一个例子：



```
var MODULE = (function () {
	var my = {},
	    privateVariable = 1;

	function privateMethod() {
		// ...
	}

	my.moduleProperty = 1;
	my.moduleMethod = function () {
		// ...
	};

	return my;
}());
```



注意到这里我们声明了一个叫做`MODULE`的全局模块，它有两个公有属性：一个叫做`MODULE.moduleMethod`的函数，和一个叫做`MODULE.moduleProperty`的属性。应该留意的是它利用匿名函数的闭包维护了`私有的内部状态`，而且，利用上面我们所学的全局导入，我们可以很容易的导入我们所需要的全局变量。

## 高级模式

虽然上面所学的知识已经足够应付大部分的情况，但对于 JavaScript 模块模式，我们仍然可以更进一步，创建一些强大的，可扩展的模式。那我们现在通过一个名叫 `MODULE` 的模块来把这些模式过一遍。

### 扩容（Augmentation）

到目前为止我们见到的模块都有一个限制，就是整个模块只能存活于一个源文件中，而任何一个在大型项目中工作过的人都会理解把一个模块分解到多个源文件的价值。还好，我们有一个很漂亮的解决方案叫做 `模块扩容`。首先我们导入模块，然后我们增加一些属性，再重新导出这个模块，从而对上述的 `MODULE` 模块达到扩容的目的：



```
var MODULE = (function (my) {
	my.anotherMethod = function () {
		// 增加新方法
	};

	return my;
}(MODULE));
```



虽然不是必须的，但我们仍然使用了 var 关键字来保持一致性。上面的代码运行以后，我们的模块将会拥有一个新的公有方法叫做 `MODULE.anotherMethod`，这个额外的源文件仍然可以维护它自己的私有内部状态和导入的变量。

### 宽松扩容（Loose Augmentation）

虽然我们上面的例子要求初始的模块必须先被创建，然后才可以进行扩容，但是事实上也不是必须这样做。JavaScript 中为了提高性能，可以异步的加载脚本。我们可以灵活的创建由多个部分组成的模块，并通过宽松扩容来以任何顺序加载它们。要求就是，每个源文件有如下的代码结构：



```
var MODULE = (function (my) {
	// add capabilities...

	return my;
}(MODULE || {}));
```



这个模式中的`var`关键字是必须的，另外注意到，被导入的模块如果尚未存在的话，将会被创建，这意味着你可以使用像[LAB.js](http://labjs.com/)这样的工具来并行的加载这些模块，而通常情况下多个模块同时加载则会被阻塞。

### 紧缩扩容（Tight Augmentation）

虽然宽松扩容很赞，但是它还是在你的模块上加了一些限制，最关键的问题是你无法安全的覆盖模块(Overwrite)属性。另外在模块初始化的时候，你不能使用来自其他源文件的模块（但是在运行时是可以的）。而紧缩扩容指定了一系列加载顺序，并且运行覆盖，下面是一个简单的例子（为前面的 `MODULE` 模块扩容）：


```
var MODULE = (function (my) {
	var old_moduleMethod = my.moduleMethod;

	my.moduleMethod = function () {
		// 方法覆盖，但是仍然通过 old_module.moduleMethod 保持对原有方法的访问
	};

	return my;
}(MODULE));
```



这里我们覆盖了`MODULE.moduleMethod`，但是如果需要的话，我们仍然可以访问被覆盖方法。

### 克隆和继承（Cloning and Inheritance）



```
var MODULE_TWO = (function (old) {
	var my = {},
		key;

	for (key in old) {
		if (old.hasOwnProperty(key)) {
			my[key] = old[key];
		}
	}

	var super_moduleMethod = old.moduleMethod;
	my.moduleMethod = function () {
	    // 覆盖被克隆的方法，并通过 super_moduleMethod 维护对服方法的访问
	};

	return my;
}(MODULE));
```


这个模式可能是最`不灵活`的，虽然它允许一些巧妙的组合(compositions)，但是以付出灵活性为代价。就像我之前所写的，一些作为属性的对象和函数不会被复制，他们仅仅作为一个对象的两个引用而存在。改变其中的一个则会同时改变另外一个。即使我们可以通过递归的克隆来解决这个问题，对函数我们仍然没有更多的办法，除了使用 `eval`。在这篇文章包括这个模式仅仅是为了完整性。

### 跨文件私有状态（Cross-File Private State）

把一个模块分割到多个文件的最大缺陷是每个文件都将会维护它自己的私有状态，并且没有任何途径访问其他文件的私有状态。这个问题有解决方案，下面这个宽松扩容模块（Loose Augmentation Module）的例子可以 `维护跨所有扩容模块的私有状态`。



```
var MODULE = (function (my) {
	var _private = my._private = my._private || {},
		_seal = my._seal = my._seal || function () {
			delete my._private;
			delete my._seal;
			delete my._unseal;
		},
		_unseal = my._unseal = my._unseal || function () {
			my._private = _private;
			my._seal = _seal;
			my._unseal = _unseal;
		};

	// 永久维护了对_private, _seal, 和 _unseal 私有状态的访问

	return my;
}(MODULE || {}));
```



在这里，任何文件都可以在 `_private` 中设置它们的局部私有变量，它们立即对其他文件可见。在这个模块完全加载以后，该应用程序应该调用 `MODULE._seal()`, 它可以阻止模块外部对内部的 `_private` 变量的访问。如果这个模块在整个应用程序的存活期间内被再次扩容，模块内部的方法可以在新文件被加载之前再次调用 `_seal()`，然后在方法被执行之后再次调用 `_unseal()`。这个模式是我今天在工作的时候突然想到的，而且之前我在其他地方都没见过它。我认为它是一个非常有用的模式，而且这个模式也值得单独写一篇文章。

### 子模块（Sub-modules）

我们要讨论的最后一个高级模式其实是最简单的，而且已经有很多创建子模块的很好的用例，其实它就像创建普通的模块一样：



```
MODULE.sub = (function () {
	var my = {};
	// ...

	return my;
}());
```



虽然看起来很简单，但我认为它值得在这里一提。子模块具有普通模块的所有高级特性，包括扩容和私有状态。

## 结论（Conclusions）

大部分 JavaScript 模块的高级模式可以被组合起来，并得到更多的有用的模式。如果是非选不可的话，我提倡组合 `宽松扩容`，`私有状态`，和 `子模块`来设计复杂应用程序这样一种途径，我自己也曾经这样做过。

在这篇文章里我还没有讨论过性能，这里我可以给出一个小小的提示：模块模式对 `提升性能` 很有好处。模块化的代码可以被很好的压缩，从而可以更快的下载，使用 `宽松的扩容(loose augmentation)` 模式使得非阻塞并行下载成为可能，也可以提高下载速度。虽然和其他方法相比，初始化的时间可能略有增加，但是这样的折衷是值得的。并且，只要全局变量都正确导入了，运行时性能应该不会降低，甚至会有所增加，因为局部变量的引用搜索链被模块缩短了。

最后通过一个例子总结一下：一个用来加载自身的子模块，如果模块尚不存在的话就创建一个新的。为了使程序简短，我去掉了私有状态，但是把它加进来也不是什么难事。这个模式允许并发加载一个具有层级关系的，复杂的代码库，这个代码库包括该模块自身，子模块和其他所有的模块。



```
var UTIL = (function (parent, $) {
	var my = parent.ajax = parent.ajax || {};

	my.get = function (url, params, callback) {
		// ok, so I'm cheating a bit :)
		return $.getJSON(url, params, callback);
	};

	// etc...

	return parent;
}(UTIL || {}, jQuery));
```



我希望你会觉得这篇文章所讲述的东西都很有用，也请你在评论处留下你的想法，现在你可以继续去写更好，也更模块化的 JavaScript 代码了。

_这篇文章在[Ajaxian.com](http://ajaxian.com/archives/a-deep-dive-and-analysis-of-the-javascript-module-pattern) 上被加精了，在那里的评论区有更多的讨论，那些评论和本文下面的这些评论都值得一读。_

### 资源

[JavaScript Module Pattern: In-Depth](http://www.adequatelygood.com/JavaScript-Module-Pattern-In-Depth.html)

[JavaScript 模块化的博客](http://yuiblog.com/blog/2007/06/12/module-pattern/)

[jQuery](http://jquery.com/)

[YUI](http://yuilibrary.com/)
