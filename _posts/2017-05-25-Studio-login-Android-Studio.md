---
layout: post
title:  "【Studio】登录Android Studio"
date:   2017/5/25 20:16:42
categories: Studio
---

Android Studio界面的右上角有个小人头像，可是我几次都登录不了。

![](http://upload-images.jianshu.io/upload_images/782269-b77c1320fc62463a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


点击登录后，跳转到网页，提示要登录guGou帐号，我也登录了，但是回到Android Studio，界面却什么也没有发生。试过几次后，也就放弃了。

过几天又看到那个小人头像，心里总是不爽。

我已经f q了呀，怎么登录不了。

上网找了找，发现这种问题。（这是链接：[http://blog.csdn.net/xx326664162/article/details/51966351](http://blog.csdn.net/xx326664162/article/details/51966351)）

错误信息隐藏的比较深，我一直没发现：

> Access is allowed from event dispatch thread only.: Access is allowed from event dispatch thread only.

这种错误的解决方法是：**将Android Studio的HTTP proxy设置为f q所使用的监听代理**。

我用的是Lantern，地址是：localhost:51670/?1， 也就是**127.0.0.1:51670/?1**
![](http://upload-images.jianshu.io/upload_images/782269-86f74c2cd395d77d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Android Studio设置代理的界面是这样的，Host name输入127.0.0.1，那Port number输入啥，总不能输入51670/?1吧，/？肯定不行，只输入51670试试，结果失败。


![](http://upload-images.jianshu.io/upload_images/782269-53a8227784405a99.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


那么Lantern的Port number是啥呢？

Lantern可以设置的，以前我竟然没发现。

第一步，连上Lantern，就会弹出一个窗口；

![](http://upload-images.jianshu.io/upload_images/782269-1a347064bc442ee1.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


接下来：

![](http://upload-images.jianshu.io/upload_images/782269-17ce6a017cb0ea3f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后：

![](http://upload-images.jianshu.io/upload_images/782269-e0a8dc8acaf83a1d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

就要到了，点高级设置：

![](http://upload-images.jianshu.io/upload_images/782269-43b11bc1a406179b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

nao，就是这个51672，输入到Android Studio的Post number中，这下再登录，就成功了。


![](http://upload-images.jianshu.io/upload_images/782269-defd40460b2d4bb1.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

每种代理的端口都不尽相同，这个举一反三，找到对应的端口就可以登录上Android Studio了。