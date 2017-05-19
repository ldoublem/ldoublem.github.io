---
title: android 利用xml绘制图形
date: 2016-05-09 15:50
tags: xml 
---
在项目中有时候要用到类似圆角按钮，触发时候颜色渐变的需求，下面介绍下使用xml绘图。


![demo1.gif](http://upload-images.jianshu.io/upload_images/1194532-754f584c4983e400.gif?imageMogr2/auto-orient/strip)
#1 虚线
```
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="line" >
    <stroke
        android:dashGap="3dp"
        android:dashWidth="6dp"
        android:width="1dp"
        android:color="@android:color/black" />
    <!-- 虚线的高度 -->
    <size android:height="1dp" />
</shape>
```
注意：如果在android 6.0上没有效果可以在代码中写
```
line.setLayerType(View.LAYER_TYPE_SOFTWARE, null);
```
或者在布局文件中加入
```
android:layerType="software"
```
#2 画三角形
```
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item>
        <rotate
            android:fromDegrees="45"
            android:toDegrees="45"
            android:pivotX="-40%"
            android:pivotY="87%">
            <shape android:shape="rectangle">
                <solid android:color="@android:color/black" />
                <stroke
                    android:width="0.5dp"
                    android:color="@android:color/black" />
            </shape>
        </rotate>
    </item>
</layer-list>
```
#3 圆形
```
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="oval"
    android:useLevel="false" >
    <solid android:color="@android:color/transparent" />
    <stroke
        android:dashGap="2dp"
        android:dashWidth="2dp"
        android:width="0.5dp"
        android:color="@android:color/black" />
</shape>

```
#4 圆角矩形
```
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android" >

    <!-- 实心 -->
    <solid android:color="@android:color/transparent" />
    <!-- 渐变 -->
    <gradient
        android:angle="90"
        android:endColor="@android:color/black"
        android:startColor="@android:color/white" />
   <!--   描边  -->
     <stroke
        android:width="1dp"
        android:color="@android:color/black" />
    <corners android:radius="5dp" />
    <!--可以设置边距
       <padding
        android:bottom="10dp"
        android:left="10dp"
        android:right="10dp"
        android:top="10dp" />
    -->
</shape>
```
#5 按钮触发效果
```
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">

  <item
      android:color="@android:color/holo_blue_bright"
      android:state_focused="true"
      android:state_pressed="true"
      />
  <item
      android:color="@android:color/holo_blue_bright"
      android:state_focused="false"
      android:state_pressed="true"
      />
  <item
      android:color="@android:color/holo_blue_bright"
      android:state_focused="true"
      />
  <item
      android:color="@android:color/holo_blue_dark"
      android:state_focused="false"
      android:state_pressed="false"
      />

</selector>

```
这里以颜色为事例，也可以是图片
```
android:drawable="@mipmap/xxx"
```


#6 简单的动画效果

```
<animation-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:drawable="@mipmap/loading_01"
        android:duration="200" />
    <item
        android:drawable="@mipmap/loading_02"
        android:duration="200" />
    <item
        android:drawable="@mipmap/loading_03"
        android:duration="200" />
    <item
        android:drawable="@mipmap/loading_04"
        android:duration="200" />
    <item
        android:drawable="@mipmap/loading_05"
        android:duration="200" />
    <item
        android:drawable="@mipmap/loading_06"
        android:duration="200" />
    <item
        android:drawable="@mipmap/loading_07"
        android:duration="200" />
    <item
        android:drawable="@mipmap/loading_08"
        android:duration="200" />
    <item
        android:drawable="@mipmap/loading_09"
        android:duration="200" />
    <item
        android:drawable="@mipmap/loading_10"
        android:duration="200" />
    <item
        android:drawable="@mipmap/loading_11"
        android:duration="200" />
    <item
        android:drawable="@mipmap/loading_12"
        android:duration="200" />
</animation-list>
```
在代码中写入
```
  AnimationDrawable an = (AnimationDrawable) loding.getBackground();
  an.start();
```
