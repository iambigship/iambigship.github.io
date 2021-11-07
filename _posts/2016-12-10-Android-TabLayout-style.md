---
layout: post
title:  "【Android】修改TabLayout中标签文字的样式"
date:   2016/12/10 13:03:42
categories: Android
---

![](http://upload-images.jianshu.io/upload_images/782269-e2c1998d67d48663?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们在用TabLayout和ViewPager做可以滑动的标签和碎片时，标签的样式一般只能设置文字颜色、滑块的颜色和厚度值

![](http://upload-images.jianshu.io/upload_images/782269-46ad2845abd38a7f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果我想设置文字的大小和加粗，在这必须用到

```
app:tabTextAppearance="@style/CustomTabLayoutTextAppearance"
```
完整代码就是这样：

![](http://upload-images.jianshu.io/upload_images/782269-a04bf06da72a2a2f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个style需要自己在styles.xml中自己定义，我在里边写了字体的大小、加粗、黑色（刚开始没写颜色直接给我展示了白色，尴尬）

![](http://upload-images.jianshu.io/upload_images/782269-25906c2937fd440c?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)