---
layout: post
title:  "【Android】TextView设置段落间距"
date:   2017/9/15 23:30:06
categories: Android
---
TextView只提供设置行距的方法，没有提供段落间距的方法，但是提供了一个 SpannableString 类来给TextView设置各种效果，
如图：

![](http://upload-images.jianshu.io/upload_images/782269-9b6cf62806221083.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


其中一个给文字替换为图片的效果给我带来了灵感，

  ![](http://upload-images.jianshu.io/upload_images/782269-06d358a7342e1252.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我可以用一个图片（最后换成一个宽1px，指定高度的透明长方形,xml中画出来的）来模拟段落间距。

> 注意画出来的高度，不能使用 用尺子直接量的值，而要比这个高度要小。
为什么呢，我也不清楚，不过我还是要上个图，我估计应该是两行文字之间的line gap的原因。


![](http://upload-images.jianshu.io/upload_images/782269-984afa71718f49e1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


后台给我们返回的json中段落区分符是“\n”，我们先把"\n"替换为"\n\r"

“\n”用来换行，“\r”就是那个将要被替换的字符了（为什么要用\r呢，阴差阳错啦，后来发现\r挺合适的，也不会重复）；最后就是用SpannableString来处理文字了。


```
/**
	 * 设置TextView段落间距
	 *
	 * @param context          上下文
	 * @param tv               给谁设置段距，就传谁
	 * @param content          文字内容
	 * @param paragraphSpacing 请输入段落间距（单位dp）
	 * @param lineSpacingExtra xml中设置的的行距（单位dp）
	 */
	public static void setParagraphSpacing(Context context, TextView tv, String content, int paragraphSpacing, int lineSpacingExtra) {
		if (!content.contains("\n")) {
			tv.setText(content);
			return;
		}
		content = content.replace("\n", "\n\r");

		int previousIndex = content.indexOf("\n\r");
		//记录每个段落开始的index，第一段没有，从第二段开始
		List<Integer> nextParagraphBeginIndexes = new ArrayList<>();
		nextParagraphBeginIndexes.add(previousIndex);
		while (previousIndex != -1) {
			int nextIndex = content.indexOf("\n\r", previousIndex + 2);
			previousIndex = nextIndex;
			if (previousIndex != -1) {
				nextParagraphBeginIndexes.add(previousIndex);
			}
		}
		//获取行高（包含文字高度和行距）
		float lineHeight = tv.getLineHeight();

		//把\r替换成透明长方形（宽:1px，高：字高+段距）
		SpannableString spanString = new SpannableString(content);
		Drawable d = ContextCompat.getDrawable(context, R.drawable.paragraph_space);
		float density = context.getResources().getDisplayMetrics().density;
		//int强转部分为：行高 - 行距 + 段距
		d.setBounds(0, 0, 1, (int) ((lineHeight - lineSpacingExtra * density) / 1.2 + (paragraphSpacing - lineSpacingExtra) * density));

		for (int index : nextParagraphBeginIndexes) {
			// \r在String中占一个index
			spanString.setSpan(new ImageSpan(d), index + 1, index + 2, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
		}
		tv.setText(spanString);
	}
```

那个透明的长方形很简单，随手一画就有了。

```
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android" android:shape="rectangle">
    <solid android:color="@color/transparent"/>
</shape>
```



下边这个就是替换后的效果，只不过这时候为了展示替换效果，那个长方形还是用了个orange色。

![](http://upload-images.jianshu.io/upload_images/782269-f08394cc3043560c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)