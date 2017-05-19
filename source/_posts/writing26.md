---
title: Android自定义视图 Mac 系列
date: 2016-07-10 08:27
tags: canvas
---
apple products by android canvas 
先上图
[代码下载](https://github.com/ldoublem/AppleView)
![效果.png](http://upload-images.jianshu.io/upload_images/1194532-f8dc0b2a7be3c894.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![效果.gif](http://upload-images.jianshu.io/upload_images/1194532-03ea6c66604cd304.gif?imageMogr2/auto-orient/strip)
用到的知识点主要是**Shader**，[Shader如何使用](http://www.jianshu.com/p/1efcc9c9f286),三阶贝塞尔曲线**Path.cubicTo**，iMac的底座就是用三阶贝塞尔曲线画的，Mac pro的阴影是用二阶贝塞尔曲线。
1,iMac
iMac底部支架,阴影效果，弯曲视觉感受。
```
 private void drawSupport(Canvas canvas) {
        pathSupport.reset();
        rectSupport.top = rectSreeen.bottom;
        rectSupport.left = rectSreeen.centerX() - rectSreeen.width() / 9f;
        rectSupport.right = rectSreeen.centerX() + rectSreeen.width() / 9f;
        rectSupport.bottom = rectSreeen.bottom + mHigth / 5 * 0.8f;
        pathSupport.moveTo(rectSupport.left + 10,
                rectSupport.top);
        pathSupport.cubicTo(

                rectSupport.left + rectSupport.width() / 18f,
                rectSupport.top + rectSupport.height() * 2.5f / 3f,

                rectSupport.left + rectSupport.width() / 16f,
                rectSupport.top + rectSupport.height() * 2f / 3f,

                rectSupport.left - 30,
                rectSupport.bottom - 20);
        pathSupport.lineTo(rectSupport.right + 30, rectSupport.bottom - 20
        );
        pathSupport.cubicTo(

                rectSupport.right - rectSupport.width() / 16f,
                rectSupport.top + rectSupport.height() * 2f / 3f,

                rectSupport.right - rectSupport.width() / 18f,
                rectSupport.top + rectSupport.height() * 2.5f / 3f,

                rectSupport.right - 10,
                rectSupport.top);


        rectBottom.top = rectSupport.bottom - 20;
        rectBottom.bottom = rectSupport.bottom - 20 + mHigth / 5 * 0.8f / 15f;
        rectBottom.left = rectSupport.left - 30;
        rectBottom.right = rectSupport.right + 30;
        pathSupport.close();


        mShader = new LinearGradient(rectSupport.centerX(), rectSupport.top,
                rectSupport.centerX(), rectSupport.bottom,
                new int[]{Color.rgb(190, 190, 190),
                        Color.rgb(245, 245, 245),
                        Color.rgb(245, 245, 245),
                        Color.rgb(190, 190, 190),
                        Color.rgb(245, 245, 245)

                }, new float[]{0f, 0.5f, 0.55f, 0.65f, 1f},
                Shader.TileMode.CLAMP);


        mPaint.setColor(Color.rgb(249, 249, 249));
        mPaint.setShader(mShader);
        canvas.drawPath(pathSupport, mPaint);
        mPaint.setShader(null);


    }
```
iMac渐变阴影效果
```
private void drawComputerShadow(Canvas canvas) {
        pathComputerShadow.reset();
        pathComputerShadow.moveTo(rectBottom.left + 10, rectBottom.bottom);

        pathComputerShadow.quadTo(rectBottom.left + 30, rectBottom.bottom,

                rectBottom.left + 30, mHigth

        );
        pathComputerShadow.lineTo(rectBottom.right - 30, mHigth);
        pathComputerShadow.quadTo(rectBottom.right - 30, rectBottom.bottom,

                rectBottom.right - 10, rectBottom.bottom

        );

        mShader = new LinearGradient(rectBottom.centerX(), rectBottom.bottom,
                rectBottom.centerX(), mHigth,
                new int[]{
                        Color.rgb(235, 235, 235),
                        Color.rgb(255, 255, 255)
                }, new float[]{0f, 1f},
                Shader.TileMode.CLAMP);

        mPaint.setColor(Color.rgb(245, 245, 245));
        mPaint.setShader(mShader);

        canvas.drawPath(pathComputerShadow, mPaint);
        mPaint.setShader(null);
    }
```
2,Mac pro
  触摸板
```
private void drawKeyboardTouch(Canvas canvas) {
        pathKeyboardTouch.reset();
        pathKeyboardTouch.moveTo((rectKeyboard.centerX() - rectBg.width() / 6f / 3f * 1.2f),
                rectKeyboard.top);
        pathKeyboardTouch.quadTo((rectKeyboard.centerX() - rectBg.width() / 6f / 3f * 1.2f),
                rectKeyboard.top + rectKeyboard.height() / 3f * 2f
                , (rectKeyboard.centerX() - rectBg.width() / 6f / 3f * 1.2f) + rectKeyboard.height()
                , rectKeyboard.top + rectKeyboard.height() / 3f * 2f


        );
        pathKeyboardTouch.lineTo((rectKeyboard.centerX() + rectBg.width() / 6f / 3f * 1.2f) - rectKeyboard.height(),
                rectKeyboard.top + rectKeyboard.height() / 3f * 2f);


        pathKeyboardTouch.quadTo(
                (rectKeyboard.centerX() + rectBg.width() / 6f / 3f * 1.2f),
                rectKeyboard.top + rectKeyboard.height() / 3f * 2f,


                (rectKeyboard.centerX() + rectBg.width() / 6f / 3f * 1.2f),
                rectKeyboard.top);


        pathKeyboardTouch.close();
        mPaint.setColor(coloKeyboardTouch);
        canvas.drawPath(pathKeyboardTouch, mPaint);
    }

```

有时间就把剩下的苹果系列产品补齐...毕竟我是个开发Android的果粉。
[代码下载](https://github.com/ldoublem/AppleView)