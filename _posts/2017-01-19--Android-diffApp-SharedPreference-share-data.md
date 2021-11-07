---
layout: post
title:  "【Android】不同App之间通过SharedPreference共享数据"
date:   2017/1/19 13:03:42
categories: Android
---


Android中不同App之间共享数据可以用SharedPreference、ContentProvider，也可以通过sharedUserId。

今天具体来说下怎么通过SharedPreference（以下简称SP）在不同App之间共享数据。


<!-- more -->

比如SharedApp是共享数据的App，ReceiverApp是来接收数据的App；SharedApp中创建一个SP1，把共享数据存进去；在ReceiverApp中创建一个SP2（需要用到SharedApp的包名）,这个SP2就包含共享数据。

关键代码：

```
Context sharedAppContext = null;
try {
    sharedAppContext = createPackageContext("com.clipperl.sharedpreferenceprovider2", 0);
} catch (PackageManager.NameNotFoundException e) {
    e.printStackTrace();
 }
```

以上代码就是通过SharedApp的包名拿到SharedApp中的SP的数据。

接下来把全部代码展示出来。

SharedApp的Activity：

```
package com.clipperl.sharedpreferenceprovider2;

import android.content.SharedPreferences;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

public class MainActivity extends AppCompatActivity {

    public static final String PREFS_READ = "PREFS_READ";
    public static final String KEY_READ = "KEY_READ";
    private SharedPreferences sharedPreferences;

    EditText etShare;
    Button btnShare;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        etShare = (EditText) findViewById(R.id.et_share);
        btnShare = (Button) findViewById(R.id.btn_share);
        btnShare.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                sharedPreferences = getSharedPreferences(PREFS_READ, MODE_WORLD_READABLE);
                SharedPreferences.Editor prefsReadEditor = sharedPreferences.edit();
                prefsReadEditor.putString(KEY_READ, String.valueOf(etShare.getText()));
                prefsReadEditor.commit();
            }
        });
    }
}
```
它的布局：
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    android:id="@+id/activity_main"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <EditText
        android:id="@+id/et_share"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="请输入可读数据"/>

    <Button
        android:id="@+id/btn_share"
        android:text="分享可读数据"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>
</LinearLayout>
```

接下来是ReceiverApp的Activity：
```
package com.clipperl.sharedpreferencereceiver2;

import android.content.Context;
import android.content.SharedPreferences;
import android.content.pm.PackageManager;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

public class MainActivity extends AppCompatActivity {

    public static final String PREFS_READ = "PREFS_READ";
    public static final String KEY_READ = "KEY_READ";

    private SharedPreferences sharedPreferences;

    private EditText etReceiver;
    private Button btnReceiver;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        etReceiver = (EditText) findViewById(R.id.et_receiver);
        btnReceiver = (Button) findViewById(R.id.btn_receiver);
        btnReceiver.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Context sharedAppContext = null;
                try {
                    sharedAppContext = createPackageContext("com.clipperl.sharedpreferenceprovider2", 0);
                } catch (PackageManager.NameNotFoundException e) {
                    e.printStackTrace();
                }
                sharedPreferences = sharedAppContext.getSharedPreferences(PREFS_READ, MODE_WORLD_READABLE);
                etReceiver.setText(sharedPreferences.getString(KEY_READ, "读取失败"));
                sharedPreferences = null;
            }
        });
    }
}
```

它对应的布局：
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    android:id="@+id/activity_main"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <EditText
        android:id="@+id/et_receiver"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="接收其他app的数据"/>

    <Button
        android:id="@+id/btn_receiver"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="接收其他app的数据"/>

</LinearLayout>
```

操作方法：
1、在SharedApp中的EditText中输入数据，点击Button保存到SP中；
2、在ReceiverApp中点击Button，拿到SP中保存的数据，并展示在EditText中。