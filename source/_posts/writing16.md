---
title: 模仿支付宝芝麻信用进度圆环
date: 2016-05-27 22:34
tags: 自定义控件
---
上篇写了自定义圆环进度条，在上篇的基础上自定义了稍微复杂一点的控件，模仿支付宝芝麻信用进度圆环。

----

[代码下载](https://github.com/ldoublem/AlpayRing)
效果图如下
![效果图](http://upload-images.jianshu.io/upload_images/1194532-0a66d2b21f7a9640.PNG)


画扇形
```
   RectF rectF=new RectF(left,top,right,bottom)
   canvas.drawArc(rectF, startAngle, sweepAngle, false, ringPaint);//第三个参数是否显示半径
```
将文字沿着圆环内测显示
```
   path.addOval(rectF, Path.Direction.CW);//Path.Direction.CCW逆时针
   canvas.drawTextOnPath(text,path,hOffset,vOffset,paint);
```
带有阴影的圆点
```
    pointPaint.setShadowLayer(pointShadowLayer, x, y, Color.WHITE);//设置阴影
    canvas.drawCircle(xPoint, yPoint, pointPaintSize, pointPaint);
```
动画依旧使用Animation，重写**applyTransformation**方法
![gif.gif](http://upload-images.jianshu.io/upload_images/1194532-9dfffb96ba01feb1.gif?imageMogr2/auto-orient/strip)

[代码下载](https://github.com/ldoublem/AlpayRing)
