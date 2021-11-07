---
layout: post
title:  "【Java】指定端口运行 war 包"
date:   2018-06-17 17:29:36
categories: Java
---

最近在调研使用 Jenkins 持续集成软件，拿到了一个 war 文件，运行 war 文件有两种方式：
1. 把 war 文件放到 tomcat 的 webapps 文件夹中，运行 tomcat ，由 tomcat 自动解压运行 war 文件中的程序；
2. 不用 tomcat，cmd 进入 war 文件所在目录，直接执行以下命名运行 war 中的程序；
> java -jar 文件名.war

今天主要想说的就是这第二种方式。

我们调用如下命令后，war 程序就开始运行了。

> java -jar jenkins.war


![](https://upload-images.jianshu.io/upload_images/782269-5ecc9b103f081d54.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后我们在浏览器输入“localhost:8080”，就可以使用 Jenkins了。
![](https://upload-images.jianshu.io/upload_images/782269-49daa147ec4611d0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里就有一个问题，端口号的问题，我们大家都知道，tomcat 默认端口号也是 8080，我们刚刚运行的 Jenkins 也用的是 8080（如果我们不得不用 tomcat，但又不想用 tomcat 运行我们当前的 Jenkins.war 时），这里很明显就冲突了。

如果我们能指定 Jenkins 运行的端口，避免它运行时使用 8080 端口，那么 Jenkins 和 tomcat 就会和平共处了。

事实上 Java 给我们提供运行 war 时指定端口的命令，我们可以借助 help 来查看一些扩展命令。

> 输入 java -jar jenkins.war --help

![](https://upload-images.jianshu.io/upload_images/782269-a3335877112670a2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们可以看到下边有一个 --httpPort 选项可以指定端口号。

接下来，我们修改指令，自定义端口号，比如我们想指定端口号为9999。

> java -jar jenkins.war --httpPort=9999

![](https://upload-images.jianshu.io/upload_images/782269-23de19aeb74c40ff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

修改浏览器访问地址：localhost:9999，达到了我们想要的结果。

![](https://upload-images.jianshu.io/upload_images/782269-9c9cf143edc08bd1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



