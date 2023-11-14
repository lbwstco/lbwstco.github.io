---
layout: post
title: onSaveInstanceState和onRestoreInstanceState
category: android
description: Android状态管理
keywords: 临时
---


通常onSaveInstanceState()只适合用于保存一些临时性的状态，而onPause()适合用于数据的持久化保存。
####onSaveInstanceState####
关于onSaveInstanceState ()，是在函数里面保存一些View有用的数据到一个Parcelable对象并返回。在Activity的onSaveInstanceState(Bundle outState)中调用View的onSaveInstanceState ()，返回Parcelable对象，接着用Bundle的putParcelable方法保存在Bundle的savedInstanceState中。
####onRestoreInstanceState####
当系统调用Activity的的onRestoreInstanceState(Bundle savedInstanceState)时， 同过Bundle的getParcelable方法得到Parcelable对象，然后把该Parcelable对象传给View的onRestoreInstanceState (Parcelable state)。在的View的onRestoreInstanceState中从Parcelable读取保存的数据以便View使用。
