---
layout: post
title:  "【Studio】Android Studio导入项目时的遇到问题（Re-download dependencies and sync project (requires network)）"
date:   2017/9/22 20:30:06
categories: Studio
---

最近在做分享，做到微信分享时，下载了微信分享的Demo，可是在导入Android Studio编译时，却遇到了一个奇怪的问题，如图：


![](http://upload-images.jianshu.io/upload_images/782269-33a292c8871eea11.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

奇怪了，我已经按照 [【Studio】Android Studio如何最快速、顺利导入其他项目](http://blog.csdn.net/yingpaixiaochuan/article/details/53190931) 一一做了修改了，但还是出现了问题，这太不科学了；并且我在点击Studio的同步按钮时，Studio根本没有联网搜索gradle的意思，直接秒弹这个错误窗口，看来应该是哪个地方有硬伤，我得仔细找找。

接下来翻遍了自己感觉可能出现问题的所有地方，中途还找度娘和谷哥了，但也没有找到解决办法，大部分文章都是说gradle的问题，可我该改的地方都改了呀，真是束手无策，在我不知道第几次翻到 **gradle-wrapper.properties** 文件时。

![](http://upload-images.jianshu.io/upload_images/782269-30787a2dac197be1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我感觉哪里有点不对，拿了一个正常项目的 **gradle-wrapper.properties** 作了下对比，发现了点猫腻，原来前边部分有点不一样，我以前都只关注后边的版本号了，压根都没注意到前边这个网址。

![](http://upload-images.jianshu.io/upload_images/782269-07c50ae2c8dc0a80.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

剩下的事就顺理成章了，我把前半部分进行了替换，然后点击同步，顺利通过编译，最后成功运行。