---
title: NodeJS笔记
categories: 
- [Javascript, NodeJS]
tags: 
- Javascript
- NodeJS
---

## `process.cwd()`, `./`, `__dirname` 和 `__filename` 的区别

- `__dirname`: 总是返回被执行的 js 所在文件夹的绝对路径
- `__filename`: 总是返回被执行的 js 的绝对路径
- `process.cwd()`: 总是返回运行 node 命令时所在的文件夹的绝对路径
- `./`： 不使用require时候，`./`与`process.cwd()`一样，使用require时候，与`__dirname`一样

参考链接与例子： https://github.com/jawil/blog/issues/18

## Promise all

Promise.all 里面传入的Promise数组，不是一个个按顺序运行的，而是并发运行的，互相没有dependencies

参考链接与例子： https://blog.csdn.net/wf19930209/article/details/79350060

如果需要一个个的运行，可以使用 async.series(funcArray, (err, results) => {…});

文档：https://caolan.github.io/async/docs.html#series