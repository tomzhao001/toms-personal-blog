---
title: JS Coding 小技巧
categories: 
- [Javascript, CodeTips]
tags:
- Javascript
- CodeTips
---

在看别人的代码的时候，特别是各种源码，会发现一下让人醍醐灌顶的写法，为了防止看了就忘，看到的时候就记下来吧，嗯！

1. 怎么表示一个数number是2的次方，比如8，16，64等

   ```
   int number = xxxxxx
   int t = number - 1;
   if (number & t == 0) {
       // This number is a power of 2
   }
   ```

2. 在JS里面，有一种判断方式可以不用`if`，而使用`&&`，下面是一个promise的例子：

   ```
   new Promise((resolve, reject) => {
       for (let i = 0; i < 100; i++) {
           i === 99 && resolve();
       }
   });
   ```

   通过这种短路原则，可以实现类似`if`的功能。

   类似的，`||`这个或的符号也可以用作默认的赋值。

   ```
   let keepAliveDelay = sockopts.keepAliveDelay || 0;
   ```

   当然也不能忘了否这个符号`!`。看到一种用法是表示一个变量是boolean类型。

   因为JS是非常弱类型的语言，没到runtime的时候，每个变量的类型都不知道，就给查代码和调试造成一点麻烦，但是`!!`这个符号可以表示boolean类型，可以说是很贴心了。

   ```
   let keepAlive = !!sockopts.keepAlive;
   ```