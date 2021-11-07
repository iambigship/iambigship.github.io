---
layout: post
title:  "【Android】两种抽屉写法（NavigationView和Toolbar互动）"
date:   2016/12/10 11:03:42
categories: Android
---

其实这两种的效果差不多，只是第一种的抽屉比较高，把Toolbar都盖住了，看不到Toolbar上三道杆的动画；另一种嘛，就是抽屉在Toolbar下方，三道杆变成箭头的动画可以完整展现。来先看效果图。

<!-- more -->

## 第一种：
![这里写图片描述](http://img.blog.csdn.net/20161126204320984)

## 第二种：
![这里写图片描述](http://img.blog.csdn.net/20161126204337953)

第一种写法非常简单，不用写一行代码，用studio可以直接创建，所以你才会看到抽屉中还有布局，而在第二种写法中，我偷懒，什么也没写，一片空白。

## 下面分别介绍下两种抽屉的写法。

----------

### 第一种写法的详细介绍：

捡重点的说，创建工程时，默认会创建Empty Activity，现在只需要改为Navigation Drawer Activity，请看图：

![这里写图片描述](http://img.blog.csdn.net/20161126202451008)

然后一路Next，工程创好，run一下，效果就出来了。

这里有一个细节，直接创建的项目中会多出一个menu菜单和一个悬浮按钮FloatingActionButton，结合自身情况，决定是删是留，如图：

![这里写图片描述](http://img.blog.csdn.net/20161126203806426)


----------

### 第二种写法的详细介绍：

布局文件：

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    >

    <android.support.v7.widget.Toolbar
        android:id="@+id/toolbar_main"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@color/colorPrimary"
        android:theme="@style/GalaxyZooThemeToolbarDarkOverflow"/>

    <android.support.v4.widget.DrawerLayout
        android:id="@+id/drawer_layout"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:fitsSystemWindows="true"
        tools:openDrawer="start">

        <TextView
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:text="Hello World"/>

        <android.support.design.widget.NavigationView
            android:id="@+id/nav_view"
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:layout_gravity="start"
            android:fitsSystemWindows="true"/>

    </android.support.v4.widget.DrawerLayout>

</LinearLayout>
```

注意，我在toolbar上增加了一个style（改变Toobar上图标和文字的颜色）

```
android:theme="@style/GalaxyZooThemeToolbarDarkOverflow"
```

需要在styles.xml中定义：

```
<style name="GalaxyZooThemeToolbarDarkOverflow" parent="Theme.AppCompat.NoActionBar">
        <!--这个是toolbar文字的颜色-->
        <item name="android:textColorPrimary">#fff</item>
        <!--这个是toolbar左边三道杆的颜色-->
        <item name="android:textColorSecondary">#fff</item>
    </style>
```


## 接下来是MainActivity：

ActionBarDrawerToggle，我用的是v7包的，它是用来让NavigationView和Toolbar上三道杆互动的。

```
import android.os.Bundle;
import android.support.v4.view.GravityCompat;
import android.support.v4.widget.DrawerLayout;
import android.support.v7.app.ActionBarDrawerToggle;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar_main);
        setSupportActionBar(toolbar);

        DrawerLayout drawer = (DrawerLayout) findViewById(R.id.drawer_layout);
        ActionBarDrawerToggle toggle = new ActionBarDrawerToggle(
                this, drawer, toolbar, 0, 0);
        drawer.setDrawerListener(toggle);
        toggle.syncState();
    }

    /**
     * 重写返回键方法
     * 若抽屉在打开状态，点返回键，只关抽屉，不退出程序。
     */
    @Override
    public void onBackPressed() {
        DrawerLayout drawer = (DrawerLayout) findViewById(R.id.drawer_layout);
        if (drawer.isDrawerOpen(GravityCompat.START)) {
            drawer.closeDrawer(GravityCompat.START);
        } else {
            super.onBackPressed();
        }
    }
}
```

本文完。