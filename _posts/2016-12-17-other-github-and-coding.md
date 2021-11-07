---
layout: post
title:  "【其他】个人站点从云服务器wordPress到GitHub与Coding双平台的转变"
date:   2016/12/17 13:03:42
categories: other
---

之前用腾讯云服务器提供的第三方wordPress博客平台建了一个个人站点，过程比较容易，操作也很简单，但是对其中的细节不了解，今天早上发现网站无法访问，报404错误，顿时束手元策。

<!-- more -->

首先想到的是给腾讯云服务器提工单，希望他们可以帮我提供一些建议，但是他们表示与他们无关

![](http://i.imgur.com/3ozdlWf.jpg)

***

![](http://i.imgur.com/KEVWu4x.jpg)

只好求助提供第三方wordPress博客平台的公司，试了他们的方法，仍无济于事。

先不说这次的坑能不能填上，要是这次搞好了下次再来个无法访问，那岂不是very悲剧。
为了更稳定的运行，决定另辟蹊径。

之前我曾经利用github pages搞过一个静态的网页，利用clipperL.gethub.io可以访问我的静态页面，当时做的很简单，只有几句话、一个链接和一个图片，只是用来玩的，后来学习了hexo，网页变得好看很多，不过github pages有一个很大的不足，因为服务器在美利坚，访问速度太慢，所以github pages上挂的页面我一直没当做主力。

当时曾想过把github pages的实践复制到腾讯的云服务器上，可以服务器我不想用windows系统，只能用linux系统，在linux上安装Node.js、Hexo，还要配置数据库。目前时间和精力都不允许我这么做，只能找其他的方法。

偶然间我发现了一个叫GitCafe的，网名说这个也可以实现静态页面托管，并且服务器在国内。说干就干，只是现在GitCafe已经被Coding收购具体流程就不细说了，文末我会补上几个详细教程。

最后成功把hexo平台上传至Coding平台，接下来就是域名解析了。经过了这次风波后，我才发现域名解析可以分国内和国外。国内访问Coding，国外访问GitHub。即可以提高国内访问速度，也可以确保当其中一个服务器故障时，不影响网站的正常访问。

***

最后附上如何用 Hexo+GitHub 和 Hexo+Coding 搭建个人博客的链接。

利用GitHub搭建一个你的博客：[http://www.jianshu.com/p/3217ecf4a789/comments/6144953#comment-6144953](http://www.jianshu.com/p/3217ecf4a789/comments/6144953#comment-6144953)

Hexo同时部署在Coding和GitHub上并使用DNSPod分流：[http://www.franktly.com/2016/07/05/Hexo%E5%90%8C%E6%97%B6%E9%83%A8%E7%BD%B2%E5%9C%A8Coding%E5%92%8CGitHub%E4%B8%8A%E5%B9%B6%E4%BD%BF%E7%94%A8DNSPod%E5%88%86%E6%B5%81/](http://www.franktly.com/2016/07/05/Hexo%E5%90%8C%E6%97%B6%E9%83%A8%E7%BD%B2%E5%9C%A8Coding%E5%92%8CGitHub%E4%B8%8A%E5%B9%B6%E4%BD%BF%E7%94%A8DNSPod%E5%88%86%E6%B5%81/)

hexo同时部署到coding(gitcafe)和github：[http://shomy.top/2016/03/03/hexo-in-coding-github/](http://shomy.top/2016/03/03/hexo-in-coding-github/)

Hexo搭建独立博客，托管到Github和Coding上教程：[http://blog.csdn.net/xiaoliuge01/article/details/50997754](http://blog.csdn.net/xiaoliuge01/article/details/50997754)

