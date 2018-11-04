title: Xcode6为什么干掉pch（Precompile Prefix Header）以及如何添加pch文件

date: 2015-03-24 14:43:14

categories: iOS

tags: [xcode6,pch]

------

一直在用xcode6开发，但发现下载的一些项目都是在xcode5上创建的，所以一直没注意到，xcode6竟然干掉pch文件了。

为什么xcode6没有自动创建pch文件呢？

简单地看：我们在写项目的时候，大部分宏定义，头文件导入都在这里，Xcode6去掉Precompile Prefix Header的主要原因可能在于Prefix Header大大的增加了Build的时间。没有了Prefix Header之后就要通过手动@import来手动导入头文件了，在失去了编程便利性的同时也降低了Build的时间。

## 如何在Xcode6中添加pch（Precompile Prefix Header）？

1，Command+N，打开新建文件窗口：ios->other->PCH file，创建一个pch文件：“工程名-Prefix.pch”：

![图片1](https://blog.flyada.com/images/QQ20150324-1.png)

2，将building setting中的precompile header选项的路径添加“$(SRCROOT)/项目名称/pch文件名”（例如：$(SRCROOT)/xx/xx-Prefix.pch）

![图片2](https://blog.flyada.com/images/QQ20150324-2.png)

可以了，编译一下程序，如果有错误检查一下添加的路径是否正确。

3，将Precompile Prefix Header为YES，预编译后的pch文件会被缓存起来，可以提高编译速度

![图片3](https://blog.flyada.com/images/QQ20150324-3.png)