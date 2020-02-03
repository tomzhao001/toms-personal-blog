---
title: Babel笔记
categories:
- [Javascript, Babel]
tags: 
- Javascript
- Babel
---

# Babel 概述

一句话，Babel就是用来把代码降级成ES2015，来兼容各种环境和浏览器。
所以，Babel其实就是一个代码转换器（code converter）。
官网文档： https://babeljs.io/docs/en/

怎么会了解到Babel的呢？其实就是因为这个错误：

```
import { log4js } from 'log4js';
^^^^^^

SyntaxError: Unexpected token import
	...
	...
```



因为在用ES6，而且主要是ES6的Airbnb的编码规范中明确写明推荐使用`import {} from xxx`而不是`require('xxx')`，但是用上了之后就遇到了这个错误。
*Airbnb ES6编码规范： https://github.com/yuche/javascript/blob/master/README.md#table-of-contents*

Node本身已经支持部分ES6语法，但是`import export`，以及`async await`(Node 8 已经支持)等一些语法，我们还是无法使用。

Babel就可以解决这个问题。
既然`import`这个关键字在Node中不支持，那么怎么办呢？对，转换！
就和TypeScript一样，为了JS代码的规范和强类型，最后还是编译成JS输出执行。
Babel在这里扮演相同的角色，在一下高版本的ES支持的语法，或者是TypeScript的强类型语法，原生JS不支持的东西，Babel几乎都可以编译成普通的可以使用的JS文件。
十分强大！！

# Babel Presets

Babel能够转换各种JS代码，靠的就是各种插件plugin，每个JS版本的转换，都需要不同的插件，毕竟JS规则不同嘛。
详细参考： https://babeljs.io/docs/en/

那么，每次加这么多plugin是不是很麻烦，就有了preset，顾名思义就是事先定好的类似模板的东西，装好preset，就自动有各种plugin了，就可以转换啦！

常见的有

```
@babel/preset-env
@babel/preset-flow
@babel/preset-react
@babel/preset-typescript
```



具体用法可参见： https://babeljs.io/docs/en/presets

不过大都只需要`npm install --save-dev`就行啦，然后配置一下`.babelrc`什么的。

# Babel ESLint