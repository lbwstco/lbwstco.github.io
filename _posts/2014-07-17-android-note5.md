---
layout: post
title: 修改User-Agent 
description: 根据项目需求，修改WebView及Http请求中的User-Agent
category: android
keywords: android, user-agent
---


####WebView中User-Agent####
```java
String userAgent = webview.getSettings().getUserAgentString();
webview.getSettings().setUserAgentString(userAgent + "need to be added~");
```
首先获取当前的userAgent,然后拼接上需要添加的内容。


####Http请求中添加User-Agent
```java
WebSettings settings = new WebView(getApplicationContext()).getSettings();
String defaultUserAgent = settings.getUserAgentString();
headers.put("User-Agent", defaultUserAgent + "need to be added~");
```
项目里使用的Volley，只需要把headers这个参数returen到getHeader()。
