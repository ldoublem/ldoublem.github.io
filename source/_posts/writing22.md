---
title: 自定义简单的LodingView
date: 2016-06-21 21:16
tags: 自定义控件
---
在开发中经常会需要自定义一个加载的动画。接下来就讲下如何自定义view来实现一些简单有趣的lodingview。
网上优秀的列子也很多。下面是我写的demo，如有错误或者问题，请多多包涵和用力的吐糟。
已经在github上更新代码，下面讲的只是几个例子。
[下载代码](https://github.com/ldoublem/LoadingView)








![效果.png](http://upload-images.jianshu.io/upload_images/1194532-a23b01311395cc25.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![效果1.gif](http://upload-images.jianshu.io/upload_images/1194532-5b1daea4d86858c5.gif?imageMogr2/auto-orient/strip)















1  **LVCircularCD**
```
  @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        mPaint.setStrokeWidth(2);
        canvas.drawCircle(mWidth / 2, mWidth / 2, mWidth / 2 - mPadding, mPaint);//画背景
        mPaint.setStrokeWidth(3);
        canvas.drawCircle(mWidth / 2, mWidth / 2, mPadding, mPaint);//画中心圆
        mPaint.setStrokeWidth(2);
       // 下面是画弧线
        RectF rectF = new RectF(mWidth / 2 - mWidth / 3, mWidth / 2 - mWidth / 3,
                mWidth / 2 + mWidth / 3, mWidth / 2 + mWidth / 3);
        canvas.drawArc(rectF, 0,80
                , false, mPaint);
        canvas.drawArc(rectF, 180,80
                , false, mPaint);
        RectF rectF2 = new RectF(mWidth / 2 - mWidth / 4, mWidth / 2 - mWidth / 4,
                mWidth / 2 + mWidth / 4, mWidth / 2 + mWidth / 4);
        canvas.drawArc(rectF2, 0, 80
                , false, mPaint);
        canvas.drawArc(rectF2, 180, 80
                , false, mPaint);
    }
      //不停旋转的动画初始化
       RotateAnimation  mProgerssRotateAnim = new RotateAnimation(0f, 360f, android.view.animation.Animation.RELATIVE_TO_SELF,
                0.5f, android.view.animation.Animation.RELATIVE_TO_SELF, 0.5f);
        mProgerssRotateAnim.setRepeatCount(-1);
        mProgerssRotateAnim.setInterpolator(new LinearInterpolator());//不停顿
        mProgerssRotateAnim.setFillAfter(true);//停在最后
```
2 **LVCircularRing** 主要是控制弧线的开始角度，动画中去控制**startAngle = 360 * value;**
```
 @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);

        mPaint.setColor(Color.argb(100, 255, 255, 255));
        canvas.drawCircle(mWidth / 2, mWidth / 2, mWidth / 2 - mPadding, mPaint);
        mPaint.setColor(Color.WHITE);
        RectF rectF = new RectF(mPadding, mPadding, mWidth - mPadding, mWidth - mPadding);
        canvas.drawArc(rectF, startAngle, 100
                , false, mPaint);//第三个参数是否显示半径

    }

    ValueAnimator valueAnimator;

    private ValueAnimator startViewAnim(float startF, final float endF, long time) {
        valueAnimator = ValueAnimator.ofFloat(startF, endF);

        valueAnimator.setDuration(time);
        valueAnimator.setInterpolator(new LinearInterpolator());
        valueAnimator.setRepeatCount(ValueAnimator.INFINITE);//无限循环
        valueAnimator.setRepeatMode(ValueAnimator.RESTART);//

        valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {

                float value = (float) valueAnimator.getAnimatedValue();
                startAngle = 360 * value;

                invalidate();
            }
        });
        valueAnimator.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                super.onAnimationEnd(animation);
            }
        });
        if (!valueAnimator.isRunning()) {
            valueAnimator.start();
        }

        return valueAnimator;
    }
```
3  **LVCircular** 类似1的做法，看代码就明白
4 **LVCircularSmile**  主要是在动画进行一半后显示笑脸。**mAnimatedValue < 0.5**不显示，反之笑脸
```
 @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        RectF rectF = new RectF(mPadding, mPadding, mWidth - mPadding, mWidth - mPadding);
        mPaint.setStyle(Paint.Style.STROKE);

        canvas.drawArc(rectF, startAngle, 180
                , false, mPaint);//第三个参数是否显示半径

        mPaint.setStyle(Paint.Style.FILL);
        if (isSmile) {
            canvas.drawCircle(mPadding + mEyeWidth + mEyeWidth / 2, mWidth / 3, mEyeWidth, mPaint);
            canvas.drawCircle(mWidth - mPadding - mEyeWidth - mEyeWidth / 2, mWidth / 3, mEyeWidth, mPaint);
        }

    }
 float mAnimatedValue = 0f;

    private ValueAnimator startViewAnim(float startF, final float endF, long time) {
        valueAnimator = ValueAnimator.ofFloat(startF, endF);
        valueAnimator.setDuration(time);
        valueAnimator.setInterpolator(new LinearInterpolator());
        valueAnimator.setRepeatCount(ValueAnimator.INFINITE);//无限循环
        valueAnimator.setRepeatMode(ValueAnimator.RESTART);
        valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {

                mAnimatedValue = (float) valueAnimator.getAnimatedValue();
                if (mAnimatedValue < 0.5) {
                    isSmile = false;
                    startAngle = 720 * mAnimatedValue;
                } else {
                    startAngle = 720;
                    isSmile = true;
                }

                invalidate();
            }
        });
        valueAnimator.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                super.onAnimationEnd(animation);

            }

            @Override
            public void onAnimationStart(Animator animation) {
                super.onAnimationStart(animation);
            }

            @Override
            public void onAnimationRepeat(Animator animation) {
                super.onAnimationRepeat(animation);
            }
        });
        if (!valueAnimator.isRunning()) {
            valueAnimator.start();

        }

        return valueAnimator;
    }
```
5  **LVCircularJump**,主要控制第几个圆圈起跳，在一个动画结束后下一个开始。
```
 valueAnimator.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                super.onAnimationEnd(animation);

            }

            @Override
            public void onAnimationStart(Animator animation) {
                super.onAnimationStart(animation);
            }

            @Override
            public void onAnimationRepeat(Animator animation) {
                super.onAnimationRepeat(animation);
                mJumpValue++;
            }
        });
```
6 **LVCircularZoom** 和5思路一致
7  **LVPlayBall** 利用贝塞尔曲线画出弹动的效果
```
 @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);


        Path path = new Path();
        path.moveTo(0 + mRadius * 2 + mPaintStrokeWidth, getMeasuredHeight() / 2);
        path.quadTo(mWidth / 2, quadToStart, mWidth - mRadius * 2 - mPaintStrokeWidth, mHigh / 2);
        mPaint.setStrokeWidth(2);
        canvas.drawPath(path, mPaint);


        mPaintCircle.setStrokeWidth(mPaintStrokeWidth);
        canvas.drawCircle(mRadius + mPaintStrokeWidth, mHigh / 2, mRadius, mPaintCircle);
        canvas.drawCircle(mWidth - mRadius - mPaintStrokeWidth, mHigh / 2, mRadius, mPaintCircle);


        if (ballY - mRadiusBall > mRadiusBall) {
            canvas.drawCircle(mWidth / 2, ballY - mRadiusBall, mRadiusBall, mPaintBall);
        } else {
            canvas.drawCircle(mWidth / 2, mRadiusBall, mRadiusBall, mPaintBall);

        }

    }
  valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {

                float value = (float) valueAnimator.getAnimatedValue();

                if (value > 0.75) {//这里控制反弹的效果


                    quadToStart = mHigh / 2 - (1f - (float) valueAnimator.getAnimatedValue()) * mHigh / 3f;
                } else {
                    quadToStart = mHigh / 2 + (1f - (float) valueAnimator.getAnimatedValue()) * mHigh / 3f;

                }

                if (value > 0.35f) {
                    ballY = mHigh / 2 - (mHigh / 2 * value);
                } else {
                    ballY = mHigh / 2 + (mHigh / 6 * value);
                }


                invalidate();
            }
        });

```
8 **LVLineWithText**
```
  @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);



        String text = mVlaue + "%";
        float textlength = getFontlength(mPaint, text);
        float texthigh = getFontHeight(mPaint, text);

        if (mVlaue == 0) {
            canvas.drawText(text, mPadding, mHigh / 2 + texthigh / 2, mPaint);
            canvas.drawLine(mPadding + textlength, mHigh / 2, mWidth - mPadding, mHigh / 2, mPaint);


        } else if (mVlaue >= 100) {
            canvas.drawText(text, mWidth - mPadding - textlength, mHigh / 2 + texthigh / 2, mPaint);
            canvas.drawLine(mPadding, mHigh / 2, mWidth - mPadding - textlength, mHigh / 2, mPaint);

        } else {
            float w = mWidth - 2 * mPadding - textlength;
            canvas.drawLine(mPadding, mHigh / 2, mPadding + w * mVlaue / 100, mHigh / 2, mPaint);
            canvas.drawLine(mPadding + w * mVlaue / 100 + textlength, mHigh / 2, mWidth - mPadding, mHigh / 2, mPaint);
            canvas.drawText(text, mPadding + w * mVlaue / 100, mHigh / 2 + texthigh / 2, mPaint);


        }

    }
```
9 **LVEatBeans** 简单的吃豆子动画
```
  @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        float eatRightX = mPadding + eatErWidth + eatErPositonX;
        RectF rectF = new RectF(mPadding + eatErPositonX, mHigh / 2 - eatErWidth / 2, eatRightX, mHigh / 2 + eatErWidth / 2);
        canvas.drawArc(rectF, eatErStrtAngle, eatErEndAngle
                , true, mPaint);//画头像


        canvas.drawCircle(mPadding + eatErPositonX + eatErWidth / 2,
                mHigh / 2 - eatErWidth / 4,
                beansWidth / 2, mPaintEye);//画眼睛

        int beansCount = (int) ((mWidth - mPadding * 2 - eatErWidth) / beansWidth / 2);
        //画豆子
        for (int i = 0; i < beansCount; i++) {

            float x = beansCount * i + beansWidth / 2 + mPadding + eatErWidth;
            if (x > eatRightX) {
                canvas.drawCircle(x,
                        mHigh / 2, beansWidth / 2, mPaint);
            }
        }
    }
 valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {

                float mAnimatedValue = (float) valueAnimator.getAnimatedValue();
                eatErPositonX = (mWidth - 2 * mPadding - eatErWidth) * mAnimatedValue;
              //一次动画进行四次嘴巴张开关闭的效果
                eatErStrtAngle = mAngle * (1 - (mAnimatedValue * eatSpeed - (int) (mAnimatedValue * eatSpeed)));
                eatErEndAngle = 360 - eatErStrtAngle * 2;
                invalidate();
            }
        });
```



[下载代码](https://github.com/ldoublem/LoadingView)