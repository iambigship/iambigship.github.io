---
layout: post
title:  "【Java】线程安全的单例模式----静态内部类"
date:   2017/3/18 16:55:42
categories: java
---

单例模式作为一种常见的设计模式，在程序中非常常见，主要是为了保证一个类只有一个唯一的对象。

从简单的“饿汉式”、“懒汉式”→利用 synchronized 和 复杂的“双重校验DCL模式”，是一个考虑线程安全的过程（其实静态的饿汉式单例模式也是线程安全的，后文有提到）。

后来有一篇文章上说“双重校验DCL模式”其实并不是线程安全的，我没看懂他说的原因（[原文在此](http://www.javaworld.com/article/2074979/java-concurrency/double-checked-locking--clever--but-broken.html?page=1)），但后来发现了另一种实现线程安全的单例模式，静态内部类方式，代码如下：
```
public class SingletonPattern {

    private SingletonPattern() {
    }

    private static class SingletonPatternHolder {
        private static final SingletonPattern singletonPattern = new SingletonPattern();
    }

    public static SingletonPattern getInstance() {
        return SingletonPatternHolder.singletonPattern;
    }
}
```

刚开始我觉得这种方式挺有意思的，但是不明白为什么要这么写，为此我专门请教了一位Java大神，他今天有空给我解释了原因，我总结一下。

在这个例子中内部类 SingletonPatterHolder 的静态变量 singletonPattern，这个变量是我们需要的那个单例，即外部类 SingletonPattern 的对象，就是那个我们需要的唯一的对象。

当我们调用 SingletonPattern.getInstance() 时，内部类  SingletonPatternHolder 才会初始化，静态变量 singletonPattern 被创建出来。

> 这个实现思路中最主要的一点就是利用类中静态变量的唯一性。

这种方式的优点是：
1. 不用 synchronized ，节省时间（虽然synchronized 浪费那个时间根本不算什么时间。唉！时间就是生命，听说不用synchronized 会快100倍，哈哈！）；
2. 调用 getInstance() 的时候才会创建对象，不调用不创建，节省空间，这有点像传说中的懒汉式。

刚开始我还有点疑惑，内部类 SingletonPatternHolder 是静态的，那么外部类 SingletonPattern 加载的时候，内部类 SingletonPatternHolder 会被加载，后来想起来，静态内部类与外部类没有什么大的关系，外部类加载的时候，内部类不会被加载，静态内部类只是调用的时候用了外部类的名字而已。

最后想了想，还是把静态饿汉式单例模式也写出来（线程安全），做个比较。

```
public class SingletonPattern {

    private static final SingletonPattern singletonPattern = new SingletonPattern();

    private SingletonPattern() {
    }

    public static SingletonPattern getInstance() {
        return singletonPattern;
    }
}
```

这个写法也是利用类的静态变量的唯一性，跟上面的静态内部类有异曲同工之妙，不过这种方式有一点不足，就是类加载的时候单例对象也会跟着加载，拖延类加载速度，有时候没用到这个类的单例对象的话，会浪费空间。有点较真哈。

《本文完》