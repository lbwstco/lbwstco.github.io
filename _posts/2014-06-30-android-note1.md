---
layout: post
title: 自定义AndroidTab
description: 
category: android
keywords: android
---


###Tab形状的drawable文件:###


#####未选中状态--head_nav_corner.xml，色值为#F7F7F7:#####
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android" >
	<solid android:color="#F7F7F7" />    
	<corners android:topLeftRadius="10dp"   
		android:topRightRadius="10dp"    
		android:bottomRightRadius="10dp"   
		android:bottomLeftRadius="10dp"/>
</shape>
```


#####选中状态--head_nav_selected_corner.xml，色值为#FF6F84:#####
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android" >
	<solid android:color="#FF6F84" />    
	<corners android:topLeftRadius="10dp"   
		android:topRightRadius="10dp"    
		android:bottomRightRadius="10dp"   
		android:bottomLeftRadius="10dp"/>
</shape>
```


通过**corners**构造圆角形状，并通过**solid**填充颜色。



###控件整体布局：###


```xml
<FrameLayout
	android:id="@+id/headNav"
	android:layout_width="150dp"
	android:layout_height="30dp"
	android:layout_centerInParent="true"
	android:background="@drawable/head_nav_corner"
	android:padding="2dp" >

	<LinearLayout
		android:id="@+id/outNavBg"
		android:layout_width="146dp"
		android:layout_height="26dp"
		android:layout_gravity="left"
		android:orientation="horizontal" >

		<TextView
			android:id="@+id/inNavBg"
			android:layout_width="73dp"
			android:layout_height="26dp"
			android:layout_gravity="center"
			android:background="@drawable/head_nav_selected_corner" />
	</LinearLayout>

	<LinearLayout
		android:layout_width="146dp"
		android:layout_height="26dp"
		android:layout_gravity="left|center_vertical"
		android:orientation="horizontal" >

		<TextView
			android:id="@+id/nav1"
			android:layout_width="0dp"
			android:layout_height="26dp"
			android:layout_weight="1"
			android:gravity="center"
			android:text="最新"
			android:textColor="#fff" />

		<TextView
			android:id="@+id/nav2"
			android:layout_width="0dp"
			android:layout_height="26dp"
			android:layout_weight="1"
			android:gravity="center"
			android:text="热度"
			android:textColor="#FF6F84" />
	</LinearLayout>
</FrameLayout>
```


以FrameLayout布局控件构造层叠布局，底层以选中状态的drawable为背景，上层用TextView控件构造两个Tab。


###代码控制动画：###


```java
private void headNavAnimation(int type) {
	float fromX, toX;
	if (type == 1) {
		fromX = 0;
		toX = 1.0f;
	} else {
		fromX = 1.0f;
		toX = 0;
	}
	TranslateAnimation translateAnimation = new TranslateAnimation(
			Animation.RELATIVE_TO_SELF, fromX, Animation.RELATIVE_TO_SELF,
			toX, Animation.RELATIVE_TO_SELF, 0, Animation.RELATIVE_TO_SELF,
			0);
	translateAnimation.setDuration(150);
	translateAnimation.setFillAfter(true);
	translateAnimation.setFillEnabled(true);
	navBg.startAnimation(translateAnimation);
}
```


navBg为控件布局中底层LinearLayout中的TextView，通过设置TranslateAnimation动画控制TextView的水平移动，type参数控制移动的方向。


设置上层两个TextView控件的点击事件中调用headNavAnimation()函数，并且控制页面主体的改变。


