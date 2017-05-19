---
title: 利用canvas画chrome logo
date: 2016-06-25 14:13
tags: canvas
---
上一篇写了几个简单的lodingview，这篇详细的写下如何使用canvas 画chrome logo，只需要四步。看图就明白
[代码下载](https://github.com/ldoublem/LoadingView)

![chromelogo.png](http://upload-images.jianshu.io/upload_images/1194532-08f7a346a7c30de0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![chromelogo1.png](http://upload-images.jianshu.io/upload_images/1194532-1bfc87168741431f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

几乎到达了预计的效果。具体的步骤如下
1先画三个120度的扇形
```
 private void drawSector(Canvas canvas)//将圆分成三个扇形
    {
        RectF rectF = new RectF(mPadding, mPadding,
                mWidth - mPadding, mWidth - mPadding);
        canvas.drawArc(rectF, -30, 120
                , true, mPaintYellow);
        canvas.drawArc(rectF, 90, 120
                , true, mPaintGreen);
        canvas.drawArc(rectF, 210, 120
                , true, mPaintRed);
    }
```
2第二步画三个等边三角形，正好是内切三角形和扇形交接处的阴影效果
```
private void drawTriangle(Canvas canvas) {//画三个等边三角形组成的大三角形正好是内切三角形
        Point point1 = getPoint((mWidth / 2 - mPadding) / 2, 90);
        Point point2 = getPoint((mWidth / 2 - mPadding), 150);
        Point point3 = getPoint((mWidth / 2 - mPadding) / 2, 210);
        Point point4 = getPoint((mWidth / 2 - mPadding), 270);
        Point point5 = getPoint((mWidth / 2 - mPadding) / 2, 330);
        Point point6 = getPoint((mWidth / 2 - mPadding), 30);
        Path pathYellow = new Path();
        pathYellow.moveTo(mWidth / 2 - point1.x, mWidth / 2 - point1.y);
        pathYellow.lineTo(mWidth / 2 - point2.x, mWidth / 2 - point2.y);
        pathYellow.lineTo(mWidth / 2 - point3.x, mWidth / 2 - point3.y);
        pathYellow.close();
        Path pathGreen = new Path();
        pathGreen.moveTo(mWidth / 2 - point3.x, mWidth / 2 - point3.y);
        pathGreen.lineTo(mWidth / 2 - point4.x, mWidth / 2 - point4.y);
        pathGreen.lineTo(mWidth / 2 - point5.x, mWidth / 2 - point5.y);
        pathGreen.close();
        Path pathRed = new Path();
        pathRed.moveTo(mWidth / 2 - point5.x, mWidth / 2 - point5.y);
        pathRed.lineTo(mWidth / 2 - point6.x, mWidth / 2 - point6.y);
        pathRed.lineTo(mWidth / 2 - point1.x, mWidth / 2 - point1.y);
        pathRed.close();
        canvas.drawPath(pathGreen, mPaintGreen);
        canvas.drawPath(pathRed, mPaintRed);
        canvas.drawPath(pathYellow, mPaintYellow);

        //扇形交接处隐形效果
        for (int i = 0; i < Math.abs(mWidth / 2 - point2.y) / 2f; i++) {

            int fraction = 35 - i;
            if (fraction > 0) {
                lineColor = (Integer) evaluator.evaluate(fraction / 100f, startYellowColor, endColor);
                mPaintLine.setColor(lineColor);
            } else {
                mPaintLine.setColor(Color.argb(0, 0, 0, 0));
            }

            canvas.drawLine(mWidth / 2, point2.y + i,
                    mWidth / 2 - point2.x * 8f / 10f, mWidth / 2 - point2.y
                    , mPaintLine);

        }


        for (int i = 0; i < Math.abs(point3.x) / 2f; i++) {
            int fraction = 35 - i;
            if (fraction > 0) {
                lineColor = (Integer) evaluator.evaluate(fraction / 100f, startGreenColor, endColor);
                mPaintLine.setColor(lineColor);
            } else

                mPaintLine.setColor(Color.argb(0, 0, 0, 0));

            canvas.drawLine(mWidth / 2 - point3.x - i, mWidth / 2 - point3.y,
                    mWidth / 2 - point4.x, mWidth / 2 - point4.y
                    , mPaintLine);


        }


        for (int i = 0; i < Math.abs(mWidth / 2 - point5.x) / 2f; i++) {
            int fraction = 30 - i;
            if (fraction > 0) {
                lineColor = (Integer) evaluator.evaluate(fraction / 100f, startRedColor, endColor);
                mPaintLine.setColor(lineColor);
            } else

                mPaintLine.setColor(Color.argb(0, 0, 0, 0));
            canvas.drawLine(mWidth / 2 - point5.x + i, mWidth / 2 - point5.y,
                    mWidth / 2 - point6.x, mWidth / 2 - point6.y
                    , mPaintLine);

        }

    }
```
3最后画中心的圆，logo就出现了
```
  private void drawCircle(Canvas canvas) {//画中心的圆覆盖
        canvas.drawCircle(mWidth / 2, mWidth / 2, (mWidth / 2 - mPadding) / 2, mPaintWhite);
        canvas.drawCircle(mWidth / 2, mWidth / 2, (mWidth / 2 - mPadding) / 2 / 6 * 5, mPaintBulue);
    }
```

利用三角函数计算圆周上的点
```
    private Point getPoint(float radius, float angle) {
        float x = (float) ((radius) * Math.cos(angle * Math.PI / 180f));
        float y = (float) ((radius) * Math.sin(angle * Math.PI / 180f));
        Point p = new Point(x, y);
        return p;
    }
```
[代码下载](https://github.com/ldoublem/LoadingView)