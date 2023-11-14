---
layout: post
title: Android显示隐藏软键盘
description: 
category: android
keywords: 软键盘
--- 


####显示软键盘####
```java
public static void hideKeyboard(View... views) {
	for (int i = 0; i < views.length; i++) {
		InputMethodManager imm = (InputMethodManager) views[i].getContext().getSystemService(Context.INPUT_METHOD_SERVICE);
		if (imm.isActive()) {
			imm.hideSoftInputFromWindow(views[i].getWindowToken(), 0);
		}
	}
}
```
传入views数组一般为EditText，遍历数组，去掉EditText的焦点。


####隐藏软键盘####
```java
public static void showKeyboard(View view) {
	InputMethodManager imm = (InputMethodManager) view.getContext().getSystemService(Context.INPUT_METHOD_SERVICE);
	imm.showSoftInput(view, 0);
}
```
传入需要获取焦点的EditText，弹出软键盘。
