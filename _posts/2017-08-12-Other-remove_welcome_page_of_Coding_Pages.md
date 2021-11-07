---
layout: post
title:  "【其他】去掉Coding Pages的欢迎页之Hosted by Coding Pages，我的是Hexo的Next主题"
date:   2017/8/12 11:30:06
categories: Other
---
首先必须把Coding Pages升级为银牌会员（免费），其实就是补全个人资料罢了。

然后在Page服务中才会出现Hosted by Coding Pages设置项，如下图。

![](http://upload-images.jianshu.io/upload_images/782269-21aebab627ea9e08.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

有两种方式来去掉欢迎页，一种是文字，一种是图片，我一看图片是300*300，感觉有点大，并且是方形的，不知道放哪比较好，最后还是选择了文字，就是下边这一行代码。

```
<p>Hosted by <a href="https://pages.coding.me" style="font-weight: bold">Coding Pages</a></p>
```
结合Hexo的Next主题，我心中理想的效果是这样，与原有的3块配合，上边两块，下边两块：

![](http://upload-images.jianshu.io/upload_images/782269-8828915a3c85b63b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

但是，如果用官方建议的p标签，效果立马就丑哭了。

![](http://upload-images.jianshu.io/upload_images/782269-195b14b3d7b2a317.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

p标签带的行间距（不知道叫法对不对）太暴力了，空了一行，好丑。果断放弃p标签，改成span标签，“强力驱动”和“主题”两个字之间的竖线也挺好看的，copy一下，最后效果就是这样，自我感觉还好，就是不知道Coding官方能不能审核通过，希望我这么优美的布局能打动他们的审核。

![](http://upload-images.jianshu.io/upload_images/782269-61ffd300da333f65.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

最后把修改的footer代码贴出来，其实我只加了3行，另外，这个footer文件的位置在：

> 你的Hexo文件夹\themes\next\layout\ _partials\footer.swig

![](http://upload-images.jianshu.io/upload_images/782269-1c126719690ce818.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

补充：
2017/08/14：一个工作日过去了，我来到Coding Pages设置页，看到审核通过了，还挺快的，好开心。

![](http://upload-images.jianshu.io/upload_images/782269-f21be3393bf0caa1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
