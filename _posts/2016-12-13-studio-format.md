---
layout: post
title:  "【Studio】解决格式化时，注释部分没有缩进的问题"
date:   2016/12/13 09:03:42
categories: Studio
---

Android studio默认代码格式化（默认Ctrl+Alt+L），是让注释从每行最左边开始显示，比如这样：

![](http://img.blog.csdn.net/20161115112945647)

<!-- more -->

我个人喜欢注释也要缩进对齐。其实这个需要自己设置，打开studio的设置，依次找

Setting->Code Style->Java->Wrapping and Braces->Keep when reformatting->Comment at first column->取消勾选

图文结合更清晰

![](http://img.blog.csdn.net/20161114171722715)

设置完后，再格式化一下代码（默认Ctrl+Alt+L），再看效果。

![](http://img.blog.csdn.net/20161115113140029)

舒服多了。