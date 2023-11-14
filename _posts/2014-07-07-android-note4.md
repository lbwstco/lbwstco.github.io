---
layout: post
title: service开机启动
description: 实现服务的开机启动，用于监听推送等
category: android
keywords: android, boot
---


####修改JPush关的广播接收器JPushBroadCastReceiver####
+ 在Manifest文件中修改对应注册的receiver，并定义Intent-Filter，添加Action如下：


	```xml
	<action android:name="android.intent.action.BOOT_COMPLETED" />
	```


+ 添加权限如下：


	```xml
	<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
	```


+ 重写OnReceive函数，捕捉开机启动Action：


	```java
	if (intent.getAction().equals("android.intent.action.BOOT_COMPLETED")) {
		Intent tintent = new Intent("com.ci123.m_raisechildren.tool.pushservice");
		context.startService(tintent);
	}
	```



####定义Service用以启动JPush相关服务####
+ 在onStart函数里检查当前JPush服务是否启动，未启动则启动


	```java
	for (RunningServiceInfo service : manager.getRunningServices(Integer.MAX_VALUE)) {
		if ("com.ci123.m_raisechildren.smallaty.JPushBroadcast".equals(service.service.getClassName()))	{
			isServiceRunning = true;
		}
	}
	if (!isServiceRunning) {
		Intent i = new Intent("com.ci123.m_raisechildren.smallaty.JPushBroadcast");
		startService(i);
	}
	```


注：manager为```(ActivityManager) this.getSystemService(Context.ACTIVITY_SERVICE);```


+ 在onDestroy函数里启动重新启动service（感觉有点流氓了）


	```java
	stopForeground(true);
	Intent intent = new Intent("com.ci123.m_raisechildren.smallaty.JPushBroadcast");
	startService(intent);
	super.onDestroy();	
	```
