---
layout: post
title:  "【Android】OkHttp和Retrofit拿到的json数据是Unicode，我要的是UTF-8呀"
date:   2017/6/24 22:52:42
categories: Android
---

不管是用HttpClient还是用OkHttp，都可以用来请求网络，然后拿到后台返回的json数据，然后按需要一步步解析。

当然，我们有时候需要先确认返回的json数据中某个字段有没有数据，我们拿到json字符串，然后用工具把它格式化，就可以人性化地看json数据。

json数据中的数字和字母一般都不会因为编码发生变化，但是汉字就比较特殊，会在GBK、UTF-8中有不同的表现形式。

之前我用的是AsyncHttp（一个古老的用HttpClient封装的网络请求框架），返回的数据是一个回调接口，接口中有个参数：JSONObject response，然后就可以一层层拿到response中需要的数据。

也可以把response的全部数据以字符串形式拿出来，放在json格式化工具中展示。

后来我在使用Retrofit时，拿到的回调数据是：Response<ResponseBody> response，然后使用response.body().string()拿到后台返回的json数据，可是当我拿到格式化工具中查看时，我傻眼了，里边没有一个汉字，全部是Unicode编码，不是乱码，不是乱码，不是乱码。

我在想是不是Retrofit处理了我的返回数据，又用Okhttp试了下，还是那样，难道后台返回的数据是Unicode，经确认，后台返回的数据是UTF-8格式的，这就奇怪了，为什么AsyncHttp拿到的是UTF-8的，难道Okhttp对数据做了手脚？

简单地看了下，没找到OkHttp有修改编码的地方，那就只能想办法修改数据编码格式了。

### 方法一：

在网上搜了下，有方法可以将UTF-8 转换成 Unicode，[http://blog.csdn.net/xyw_blog/article/details/8510740](http://blog.csdn.net/xyw_blog/article/details/8510740)，不过看起来好长。

```
utf-8转unicode
/**
  * utf-8 转换成 unicode
  * @author fanhui
  * 2007-3-15
  * @param inStr
  * @return
  */
 public static String utf8ToUnicode(String inStr) {
        char[] myBuffer = inStr.toCharArray();
        
        StringBuffer sb = new StringBuffer();
        for (int i = 0; i < inStr.length(); i++) {
         UnicodeBlock ub = UnicodeBlock.of(myBuffer[i]);
            if(ub == UnicodeBlock.BASIC_LATIN){
             //英文及数字等
             sb.append(myBuffer[i]);
            }else if(ub == UnicodeBlock.HALFWIDTH_AND_FULLWIDTH_FORMS){
             //全角半角字符
             int j = (int) myBuffer[i] - 65248;
             sb.append((char)j);
            }else{
             //汉字
             short s = (short) myBuffer[i];
                String hexS = Integer.toHexString(s);
                String unicode = "\\u"+hexS;
             sb.append(unicode.toLowerCase());
            }
        }
        return sb.toString();
    }
 
 /**
  * unicode 转换成 utf-8
  * @author fanhui
  * 2007-3-15
  * @param theString
  * @return
  */
 public static String unicodeToUtf8(String theString) {
  char aChar;
  int len = theString.length();
  StringBuffer outBuffer = new StringBuffer(len);
  for (int x = 0; x < len;) {
   aChar = theString.charAt(x++);
   if (aChar == '\\') {
    aChar = theString.charAt(x++);
    if (aChar == 'u') {
     // Read the xxxx
     int value = 0;
     for (int i = 0; i < 4; i++) {
      aChar = theString.charAt(x++);
      switch (aChar) {
      case '0':
      case '1':
      case '2':
      case '3':
      case '4':
      case '5':
      case '6':
      case '7':
      case '8':
      case '9':
       value = (value << 4) + aChar - '0';
       break;
      case 'a':
      case 'b':
      case 'c':
      case 'd':
      case 'e':
      case 'f':
       value = (value << 4) + 10 + aChar - 'a';
       break;
      case 'A':
      case 'B':
      case 'C':
      case 'D':
      case 'E':
      case 'F':
       value = (value << 4) + 10 + aChar - 'A';
       break;
      default:
       throw new IllegalArgumentException(
         "Malformed   \\uxxxx   encoding.");
      }
     }
     outBuffer.append((char) value);
    } else {
     if (aChar == 't')
      aChar = '\t';
     else if (aChar == 'r')
      aChar = '\r';
     else if (aChar == 'n')
      aChar = '\n';
     else if (aChar == 'f')
      aChar = '\f';
     outBuffer.append(aChar);
    }
   } else
    outBuffer.append(aChar);
  }
  return outBuffer.toString();
 }
```
### 方法二：
然后又去找其他方法，之前用过Retrofit和Gson的配合，然后gradle中导入Gson和Retrofit的Gson转换包，这一次拿到response.body().string()的数据后，放进格式化工具中一看，汉字出来了。

有点小激动，看来Gson默认是UTF-8格式，成功了，但是又一想，为了这个目的，连导了两个包，这不划算呀。

### 方法三：
继续思考，受Gson和AsyncHttp中JsonObject的启发，我尝试创建了一个对象，
```
JsonObject jsonObject = new JsonObject(response.body().string());
```
然后调用
```
jsonObject.toString();
```
把得到的数据放入格式化工具中，汉字出现了。

## 总结

这样比较下来，方法三还是比较简单方便的。