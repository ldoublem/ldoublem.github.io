---
title: 模仿微信运动统计表
date: 2016-05-19 18:34
tags: 自定义控件
---
微信运动每天十点准时一发，每次走上个两万步心中暗暗窃喜总可以占第一位了，现实有时候就是那么...的处处有惊吓
下面是模仿微信运动的图
![效果图.png](http://upload-images.jianshu.io/upload_images/1194532-4b6a4d19e6e9c199.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
简单的继承view,然后计算高度和宽度，将每个点所在的**RectF**记录下来，以便在**onTouch**事件中监听到。
```
 @Override
    public boolean onTouch(View arg0, MotionEvent event) {

        if (event.getAction() == MotionEvent.ACTION_UP) {
            float x = event.getX();
            float y = event.getY();
            for (int i = 0; i < listRectF.size(); i++) {
                if (listRectF.get(i).contains(x, y)) {
                  //点击了某块区域
                    break;
                }
            }
        }
        return true;
    }
```
背景有一个颜色的渐变效果，用到了**Shader**
```
        mShader = new LinearGradient(0, 0, 0, getHeight(), 
                new int[]{Color.argb(200, 255, 255, 255),
                getResources().getColor(R.color.transparency)}, null, Shader.TileMode.CLAMP);
        paint.setShader(mShader);
```
![效果.gif](http://upload-images.jianshu.io/upload_images/1194532-8dc11da17378d358.gif?imageMogr2/auto-orient/strip)


[下载代码](https://github.com/ldoublem/wxsportstatistical)
