---
layout: post
title:  "【Android】Android中WebView实现Java与JS交互"
date:   2017/1/25 13:03:42
categories: Android
---


现在混合式开发是大趋势，H5不断蚕食移动互联网的份额，有的公司甚至只用H5就搞了一个APP，我们搞Android的不说会点H5，至少要懂怎么和H5（和JavaScript）交互，费话不多说。


<!-- more -->

# 一、先看效果：

![](http://upload-images.jianshu.io/upload_images/782269-fff57f1f59bd173b.gif?imageMogr2/auto-orient/strip)

# 二、此效果图实现了以下4个功能：
1. Java调用JS中的无参函数；
2. Java调用JS中的有参函数，参数是从Java中传入的；
3. JS调用Java的无参函数；
4. JS调用Java中的有参函数，参数是从JS中传入的；

# 三、说几个重点的地方：

#### 1、调用本地html的方法（html放在assets文件夹中，当然实战中会是一个网址，html一般是H5开发人员来写）

```
//打开从本地assets中的html文件
mWebView.loadUrl("file:///android_asset/androidAndJs.html");
```

#### 2、在Android中声明一个接口名字，供JS调用（  addJavascriptInterface()的第二个参数  ）

```
//提供给js的接口名称
mWebView.addJavascriptInterface(mContext, "android");
```

#### 3、调用WebView的addJavascriptInterface()，需要在Activity的onCreate()方法上加上注解：@SuppressLint("JavascriptInterface")

```
@SuppressLint("JavascriptInterface")
@Override
public void onCreate(Bundle savedInstanceState) {
    //省略
}
```

#### 4、供JS调用的方法要加上注解：@JavascriptInterface

```
    //供JS调用的方法
    @JavascriptInterface
    public void jsCallJava(){
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                Toast.makeText(mContext, "js调用java的代码", Toast.LENGTH_SHORT).show();
            }
        });
    }
```

#### 5、Java调用JS代码

__5.1 调用JS无参函数

```
mWebView.loadUrl("javascript:javaCallJs()");
```

__5.2  如果要传参数，字符串用单引号包裹！

```
mWebView.loadUrl("javascript:javaCallJsWithArgs('我是从java过来的参数')");
```

#### 6、不要忘记网络权限

```
<uses-permission android:name="android.permission.INTERNET" />  
```

# 四、接下来是完整代码：

布局文件：
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/colorPrimary"
    android:orientation="vertical">

    <WebView
        android:id="@+id/webview"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"/>

    <TextView
        android:id="@+id/tv_show"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Android页面"
        android:textColor="#FFFFFF"/>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:gravity="center"
        android:orientation="horizontal">

        <Button
            android:id="@+id/btn_no_args"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_margin="5dp"
            android:layout_weight="1"
            android:background="@color/colorAccent"
            android:text="调用js的函数"/>

        <Button
            android:id="@+id/btn_with_args"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_margin="5dp"
            android:layout_weight="1"
            android:background="@color/colorAccent"
            android:text="向js传参数"/>
    </LinearLayout>
</LinearLayout>
```

MainActivity代码：

```
package com.clipperl.androidjsdemo2;

import android.annotation.SuppressLint;
import android.content.Context;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.view.View;
import android.webkit.JavascriptInterface;
import android.webkit.WebView;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    private Context mContext = this;

    private WebView mWebView;
    private TextView mTvShow;
    private Button mBtnNoArgs, mBtnWithArgs;

    @SuppressLint("JavascriptInterface")
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        initView();

        //支持javascript
        mWebView.getSettings().setJavaScriptEnabled(true);
        //打开从本地assets中的html文件
        mWebView.loadUrl("file:///android_asset/androidAndJs.html");
        //提供给js的接口名称
        mWebView.addJavascriptInterface(mContext, "android");

    }

    private void initView() {
        mWebView = (WebView) findViewById(R.id.webview);
        mTvShow = (TextView) findViewById(R.id.tv_show);
        mBtnNoArgs = (Button) findViewById(R.id.btn_no_args);
        mBtnWithArgs = (Button) findViewById(R.id.btn_with_args);

        mBtnNoArgs.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                mWebView.loadUrl("javascript:javaCallJs()");
            }
        });

        mBtnWithArgs.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                mWebView.loadUrl("javascript:javaCallJsWithArgs('我是从java过来的参数')");
            }
        });
    }
    //供JS调用的方法
    @JavascriptInterface
    public void jsCallJava(){
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                Toast.makeText(mContext, "js调用java的代码", Toast.LENGTH_SHORT).show();
            }
        });
    }
    //供JS调用的方法
    @JavascriptInterface
    public void jsCallJavaWithArgs(final String args){
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                Toast.makeText(mContext, "js调用了java的代码,并传入以下参数：\n" + args, Toast.LENGTH_SHORT).show();
            }
        });
    }
}

```

最后是html文件：

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<script type="text/javascript">
			function javaCallJs(){
				document.getElementById("content").innerHTML = "java刚刚调用了js的无参函数javaCallJs";
			}
			function javaCallJsWithArgs(args){
				document.getElementById("content").innerHTML = "java调用了js的有参函数，传入的参数为：<br>" + args;
			}
		</script>
	</head>
	<body>
		html网页<br />
		<input type="button" value="js调用java的无参函数" onclick="window.android.jsCallJava()" />
		<input type="button" value="js调用java的有参函数，并向java中传递参数" onclick="window.android.jsCallJavaWithArgs('我是从js中来的参数')"/>
		<div id="content"></div>
	</body>
</html>
```