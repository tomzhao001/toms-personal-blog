---
title: Nexus搭建本地NPM仓库
categories: 
- [DevOps, Nexus]
tags: 
- DevOps
- Nexus
---

在nodejs的开发中，难免会想要去开发一点可重用的包，来方便别人和自己。在NPM官网上有非常详细的关于如何把包上传至NPM仓库的。不过毕竟有些有缺陷或者未完成的包，不好意思上传到公网，或者是公司内部项目要放到公司的私有仓库中。

所以，这里，我们主要关注如何在本地搭建Nexus的NPM仓库。

第一步肯定是上Nexus的Github，看readme文档：https://github.com/sonatype/docker-nexus3

非常简单，一句话`$ docker run -d -p 8081:8081 --name nexus sonatype/nexus3`

这就是docker的魅力所在啊！

等一会儿，直接访问http://localhost:8081，就有啦。

第二步就是创建仓库啦。用`admin` - `admin123`登陆，点击管理 —> Repositories —> Create repository

有三种npm仓库类型，proxy、host和group

首先proxy就是字面意思，可以设置一个代理仓库，我们的私有仓库肯定也是要链接公网npm的，不然都不能下载新的包。所以一般proxy会设置成 [https://registory.npmjs.org](https://registory.npmjs.org/)

这个host才是真正本地的私有仓库，proxy下载下来的，以及你自己上传的，都是放到这里。

我们可以看到远端proxy和本地host是两个仓库，频繁切换会很不方便，这就有了group！顾名思义，就是把proxy和host结合起来。这样就可以放心地把npm config里面的registry改成npm-group了！

创建好这三个之后，就设置新的registry吧！

```
npm config set registry http://localhost:8081/repository/npm-group/
```

这个url可以在你新建的仓库的属性中查看。

到此，这个npm的仓库已经可以用了，但是我们还不能把自己的包放到上面去，我们还有两步！

首先，开启Realms，点击管理 —> Realms —> npm Bearer Token Realm 把这个放到右边Active去，就好了。

这样还不行，因为这样你`npm publish`的时候，其实是提交到npm-group上，这样是会报错的。

最后一步，在项目中的package.json文件中，添加这个属性：

```
"publishConfig": {
    "registry": "http://localhost:8081/repository/npm-host/"
  },
```

指定传到host上面去，就好啦！

参考链接：

https://exchange.sonatype.com/details?extension=4789672387

https://zhuanlan.zhihu.com/p/35907412