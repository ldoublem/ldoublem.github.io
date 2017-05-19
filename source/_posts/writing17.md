---
title: 一个很酷的登录转场动画控件
date: 2016-06-03 16:58
tags: 自定义控件
---
最近开始在学习swift，看到了一个很酷的控件。登录加载进度，成功后显示效果按钮转场动画。
![图片.png](http://upload-images.jianshu.io/upload_images/1194532-dd941347593325f8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![目标效果.gif](http://upload-images.jianshu.io/upload_images/1194532-56d47a82fa125e22.gif?imageMogr2/auto-orient/strip)


android 实现的效果如下
![效果.gif](http://upload-images.jianshu.io/upload_images/1194532-37b326409c9822e8.gif?imageMogr2/auto-orient/strip)

主要有三个动画，一个是圆角按钮渐变成圆形，然后进度图按中心点不限循环
```
        mProgerssRotateAnim.setRepeatCount(-1);//无限循环
        mProgerssRotateAnim.setInterpolator(new LinearInterpolator());//不停顿
        mProgerssRotateAnim.setFillAfter(true);//停在最后
```
最后一个就是按钮放大动画。

[android的代码下载地址](https://github.com/ldoublem/ProgressButton)
