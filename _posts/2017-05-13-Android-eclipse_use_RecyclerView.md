---
layout: post
title:  "【Android】eclipse中使用RecyclerView"
date:   2017/5/13 11:02:42
categories: Android
---

这篇文章其实没什么太大的使用价值，有点开历史倒车，因为现在Android大都用Studio开发了嘛，权当是记录一下曾经的经历吧。

eclipse中使用RecyclerView有两种方法，一种是直接导jar包，另一种是依赖类库。

> 先把丑说在前头，强烈建议使用第二种方法，虽然麻烦点，但是更稳定，bug少。

现在详细说下每种方法。

**第一种方法是导入jar包。**

这个jar文件其实我们每个人都有，就在sdk中，按照下图的路径一级一级找，就能找到recyclerview-v7这个文件夹，里面有好多版本，但是很遗憾只有最低版本里面的jar文件可以用，对，就是21.0.0这个版本，这是recyclerview的第一个版本，不太完美，有bug（滑动冲突），不过如果你只是用来写个demo练习下RecyclerView的基本功能，那没啥大问题。

![](http://upload-images.jianshu.io/upload_images/782269-1a00c37120b1acb9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

进入21.0.0这个文件夹，解压第一个arr文件，里面有个classes.jar的文件，这就是我们需要的recyclerview的jar文件，可以改名的，改完名放入libs文件，剩下的事我就不多说了，你们都懂。

![](http://upload-images.jianshu.io/upload_images/782269-58448a0fcc178579.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**接下来说下第二种方法。**

![](http://upload-images.jianshu.io/upload_images/782269-cf7f0db12dfe56c5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

找到上图这个文件夹，把这个文件夹以类库方式导入进eclipse，要以复制的方法哈。


![](http://upload-images.jianshu.io/upload_images/782269-0e521874cadc88f3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后看下图。


![](http://upload-images.jianshu.io/upload_images/782269-845e43316888d714.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这样一做后，别的项目就是引用recyclerview的类库了。

![](http://upload-images.jianshu.io/upload_images/782269-424ba4c67ada55c7.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

本文完。