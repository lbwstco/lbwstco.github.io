---
layout: post
title: TranslateAnimation实现循环动画
description: 
category: android
keywords: TranslateAnimation, Repeat 
--- 


####实现控件上下循环移动####


```java
private void arrowAnimation() {
	TranslateAnimation translateAnimation = new TranslateAnimation(0, 0, 0, 3.0f);
	translateAnimation.setDuration(150);
	translateAnimation.setFillAfter(true);
	translateAnimation.setFillEnabled(true);
	translateAnimation.setRepeatCount(Animation.INFINITE);
	translateAnimation.setRepeatMode(Animation.REVERSE);
	widget.startAnimation(translateAnimation);
}
```


代码中通过setRepeatCount(Animation.INFINITE)设置动画的循环播放效果。
