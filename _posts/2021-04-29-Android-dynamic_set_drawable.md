---
layout: post
title:  "【Android】代码中动态设置 drawable 背景"
date:   2021-04-29 10:50:36
categories: Android
---

如图这样的效果，我们平时直接可以在 xml 写死。

![](https://img-blog.csdnimg.cn/20210429153536966.png)

写法很简单。
```xml
<?xml version="1.0" encoding="utf-8"?><!--白色圆角线条背景图-->
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <corners android:radius="15dp" />
    <stroke
        android:width="1dp"
        android:color="@color/white" />
</shape>
```
但是如果它的样式会动态变化。

![](https://img-blog.csdnimg.cn/20210429154008862.png)

一种还好，我们可以再写一个 xml 文件。

但是如果有好几种，再一一写一个对应的 xml 文件就有点被动了。

这个时候，我们可以在 java 或 kotlin 代码中去设置，这里我用 kotlin 写一下。

```kotlin
//dp1 是 1dp 对应的像素值，单位为 float
var tvBg = GradientDrawable().apply {
     shape = GradientDrawable.RECTANGLE//样式，矩形
     cornerRadius = dp1 * 16f//圆角
     setStroke(dp1, ContextCompat.getColor(act, R.color.white)//边框的大小和颜色
}
tv_btn_reserve.background = tvBg
```
是不是很简单呀。
