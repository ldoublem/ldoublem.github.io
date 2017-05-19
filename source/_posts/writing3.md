---
title: android第一次启动白屏或者黑屏
date: 2016-05-05 09:56
tags: android
---
在很多时候启动一个app的时候会白屏几秒钟，如果想解决这样子的问题其实很简单，只要在style中修改android:windowBackground即可
```
<item name="android:windowBackground">图片或者颜色</item>
```
或者直接设置背景透明色
```
<item name="android:windowIsTranslucent">true</item>
```
如果在手机运动应用很多的时候内存很少的时候设置透明色，点击app启动会有几秒无反应，原因就是设置了透明色，用户视觉上没有感受到交互。
