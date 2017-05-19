---
title: 一只贱贱的小鬼by canvas
date: 2016-06-30 22:22
tags: canvas
---
一只贱贱的小鬼，信誓旦旦地冲去，灰头土脸地回来。
[代码下载](https://github.com/ldoublem/LoadingView)
灵感来于dribbble，主要是利用贝塞尔曲线画裙边，先上图


![效果.png](http://upload-images.jianshu.io/upload_images/1194532-ee7e6c097f1001cd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
![效果.gif](http://upload-images.jianshu.io/upload_images/1194532-024433c385d63a95.gif?imageMogr2/auto-orient/strip)

主要步骤
```
        drawShadow(canvas);//画阴影
        drawHead(canvas);//画头部
        drawBody(canvas);//画身体
        drawHand(canvas);//画小手
```
重点是画带有裙边的身体，利用**path.quadTo**,至于二阶贝塞尔曲线网上一搜一大把...
```
private void drawBody(Canvas canvas) {
        path.reset();

        float x = (float) ((rectFGhost.width() / 2 - 15) * Math.cos(5 * Math.PI / 180f));
        float y = (float) ((rectFGhost.width() / 2 - 15) * Math.sin(5 * Math.PI / 180f));

        float x2 = (float) ((rectFGhost.width() / 2 - 15) * Math.cos(175 * Math.PI / 180f));
        float y2 = (float) ((rectFGhost.width() / 2 - 15) * Math.sin(175 * Math.PI / 180f));


        path.moveTo(rectFGhost.left + rectFGhost.width() / 2 - x, rectFGhost.width() / 2 - y + rectFGhost.top);
        path.lineTo(rectFGhost.left + rectFGhost.width() / 2 - x2, rectFGhost.width() / 2 - y2 + rectFGhost.top);
        path.quadTo(rectFGhost.right + wspace / 2, rectFGhost.bottom
                , rectFGhost.right - wspace, rectFGhost.bottom - hspace);


        float a = mskirtH;//(mskirtH/2);

        float m = (rectFGhost.width() - 2 * wspace) / 7f;//波动7次

        for (int i = 0; i < 7; i++) {
            if (i % 2 == 0) {
                path.quadTo(rectFGhost.right - wspace - m * i - (m / 2), rectFGhost.bottom - hspace - a
                        , rectFGhost.right - wspace - (m * (i + 1)), rectFGhost.bottom - hspace);
            } else {
                path.quadTo(rectFGhost.right - wspace - m * i - (m / 2), rectFGhost.bottom - hspace + a
                        , rectFGhost.right - wspace - (m * (i + 1)), rectFGhost.bottom - hspace);

            }
        }

        path.quadTo(rectFGhost.left - 5, rectFGhost.bottom
                , rectFGhost.left + rectFGhost.width() / 2 - x, rectFGhost.width() / 2 - y + rectFGhost.top);


        path.close();
        canvas.drawPath(path, mPaint);


    }

```
最后加入动画效果。控制裙边的张和，运动的轨迹以及小手的位置。
[代码下载](https://github.com/ldoublem/LoadingView)