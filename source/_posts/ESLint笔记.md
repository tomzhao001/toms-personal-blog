---
title: ESLint笔记
categories: 
- [Javascript, ESLint]
tags: 
- ESLint
- NodeJS
- Javascript
---

# ESLint 概述

ESLint可以说是JS的一种CheckStyle，在VS Code里面直接有插件，他会帮你把有violation的地方直接画出来，特别人性化。

使用`npm install -g eslint`之后
就可以在CLI里面的项目根目录下面通过`eslint --init`来初始化你的rule。

会有一些选项供选择，可以直接使用Google，Amazon或者Airbnb的已有配置，也可以自定义rule。
我个人偏向于自己自定义，然后保存成JSON格式，在不同的项目里可以有不同的rule，而且在项目根目录的`.eslintrc.json`中可以手动配置。

# ESLint 使用中遇到的问题

## 1. 对于一些node的内置变量，eslint会划红线表示`no-undef`

前面提到过，因为eslint是面向JS的，所以node中特有的变量不会被识别。
解决方法就是，在`.eslintrc.json`中添加`env`的配置：

```
"env": {
    "node": true
},
```



参考链接：https://segmentfault.com/q/1010000004904525

## 2. 如何Disable代码的Check

有的时候不可避免的有些代码不能通过eslint的check，这个时候就需要disable这个功能了，只要一行注释就可以解决：
`const temp = '00'; // eslint-disable-line no-unused-vars`

在`eslint-disable-line`后面哪个，是eslint check的错误信息，复制黏贴上去就可以啦。

那么，有时候要diable大量的代码怎么办呢？
可以参考下面的官方文档，是关于如何global取消一个violation的check的：
https://eslint.org/docs/rules/no-undef