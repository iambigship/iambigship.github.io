---
layout: post
title:  "【Java】把一个对象转化为String字符串"
date:   2016/12/30 13:03:42
categories: Java
---


把一个对象obj转化为字符串，我有三个想法。

1. ` obj + "";  `

2. ` obj.toString(); `

3. ` String.valueOf(obj); `


<!-- more -->

这三种方法分别是我在三个阶段的用法。

最开始只为图省事，直接加上字符串；后来知道Object有一个toString()方法，当然像Integer会重写toString()方法；再后来才知道还有个更严谨的方法String.valueOf(Object obj)。

来看下String.valueOf(Object obj)的源码：


```
public static String valueOf(Object obj) {
  return (obj == null) ? "null" : obj.toString();
}
```

它调用了toString()方法，多了一个判断，避免了空指针；不过当你发现打印出null或者TextView中显示出null，你也就知道obj为空了，不过程序不会拋异常。
