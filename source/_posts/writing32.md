---
title: Android view 之自定义progress bar
date: 2016-08-25 18:00
tags: 自定义控件
---
灵感来于apple watch里面健康记录的圆环统计，效果很赞。网上也有很优秀的开源，但是之前用过一个在加载阴影效果的时候会很卡，于是乎决定自己写一个以庆祝下G20放假12天。下面是效果图
![shot1.jpeg](http://upload-images.jianshu.io/upload_images/1194532-3a6cc90298ce525d.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![shot2.gif](http://upload-images.jianshu.io/upload_images/1194532-a6c560ebada33b8d.gif?imageMogr2/auto-orient/strip)

附加一个倒计时的功能
![倒计时.gif](http://upload-images.jianshu.io/upload_images/1194532-3703d2734fa930a7.gif?imageMogr2/auto-orient/strip)
[代码下载](https://github.com/ldoublem/RingProgress)
接下来说说实现的具体步骤
1，画颜色渐变的背景，使用canvas画图片然后再加载到view，在刷新的时候就可以直接使用该图片，减少资源避免卡顿。
```
private void setmBitmapBg(Paint paint, Bitmap mBitmapBg) {
        Canvas canvas = null;
        canvas = new Canvas(mBitmapBg);

        for (int i = 0; i < mListRing.size(); i++) {
            paint.reset();
            paint.setAntiAlias(true);
            paint.setStrokeWidth(ringWidth);
            paint.setStyle(Paint.Style.STROKE);
            if (isCorner) {
                paint.setStrokeCap(Paint.Cap.ROUND);
                paint.setStrokeJoin(Paint.Join.ROUND);
            }

            int red = (bgColor & 0xff0000) >> 16;
            int green = (bgColor & 0x00ff00) >> 8;
            int blue = (bgColor & 0x0000ff);
            int colorvaluer = red + (255 - red) / mListRing.size() * i;
            int colorvalueg = green + (255 - green) / mListRing.size() * i;
            int colorvalueb = blue + (255 - blue) / mListRing.size() * i;

            paint.setColor(Color.rgb(colorvaluer, colorvalueg, colorvalueb));


            Path pathBg = new Path();
            RectF r = new RectF();
            r.top = rectFBg.top + ringWidth * i;
            r.bottom = rectFBg.bottom - ringWidth * i;
            r.left = rectFBg.left + ringWidth * i;
            r.right = rectFBg.right - ringWidth * i;
            mListRing.get(i).setRectFRing(r);

            pathBg.addArc(r, 0, SweepAngle);

            if (i == 0 && isDrawBgShadow) {
                paint.setShadowLayer(ringWidth / 3,
                        0 - ringWidth / 4,
                        0, bgShadowColor);
            }
            if (isDrawBg)
                canvas.drawPath(pathBg, paint);

        }
        bgChange = false;

    }
```
设置** paint**的** setStrokeCap**达到圆环圆角效果
```
  paint.setStrokeCap(Paint.Cap.ROUND);
```
**Path**的**addArc**画弧线
```
pathBg.addArc(r, 0, SweepAngle);
```
设置阴影效果
```
 paint.setShadowLayer(ringWidth / 3,
                        0 - ringWidth / 4,
                        0, bgShadowColor);
```
2，主要功能点，根据model类是值计算进度并显示出来
```
 private void drawProgress(Canvas canvas, Paint paint) {


        for (int i = 0; i < mListRing.size(); i++) {

            paint.reset();
            paint.setAntiAlias(true);
            paint.setStrokeWidth(ringWidth);
            paint.setStyle(Paint.Style.STROKE);
            Path pathProgress = new Path();
            pathProgress.addArc(mListRing.get(i).getRectFRing(), 0,

                    (int) (SweepAngle / 100f * mListRing.get(i).getProgress() * mAnimatedValue)

            );

            Shader mShader = new LinearGradient(mListRing.get(i).getRectFRing().left, mListRing.get(i).getRectFRing().top,
                    mListRing.get(i).getRectFRing().left, mListRing.get(i).getRectFRing().bottom,
                    new int[]{
                            mListRing.get(i).getStartColor(),
                            mListRing.get(i).getEndColor()

                    }, new float[]{0f, 1f},
                    Shader.TileMode.CLAMP);

            paint.setShader(mShader);
            if (isCorner) {
                paint.setStrokeCap(Paint.Cap.ROUND);
                paint.setStrokeJoin(Paint.Join.ROUND);
            }
            canvas.drawPath(pathProgress, paint);
            paint.setShader(null);


            mPaintText.setTextSize(mPaint.getStrokeWidth() / 2);


            String textvalue = String.valueOf(mListRing.get(i).getValue());

            float arc_length = (float) (Math.PI * mListRing.get(i).getRectFRing().width()
                    * (mListRing.get(i).getProgress() / 100f))
                    * (SweepAngle / 360f);

            float textvalue_length = getFontlength(mPaintText, textvalue);

            if (mAnimatedValue == 1) {


                if (arc_length - textvalue_length * 1.5f <= 0) {


                    float textvalue_length_one = textvalue_length * 1.0f / textvalue.length();


                    int textvalue_size = (int) (arc_length / textvalue_length_one);

                    if (textvalue_size >= textvalue.length())

                    {
                        canvas.drawTextOnPath(textvalue
                                ,
                                pathProgress,
                                10,
                                getFontHeight(mPaintText) / 3,
                                mPaintText);
                    } else {
                        String text = textvalue.substring(0, 1);
                        for (int j = 0; j < textvalue_size; j++) {
                            text = text + ".";
                        }

                        canvas.drawTextOnPath(text
                                ,
                                pathProgress,
                                10,
                                getFontHeight(mPaintText) / 3,
                                mPaintText);


                    }
                } else {

                    canvas.drawTextOnPath(textvalue
                            ,
                            pathProgress,
                            (float) (arc_length
                                    - textvalue_length * 1.5f),
                            getFontHeight(mPaintText) / 3,
                            mPaintText

                    );
                }
            }


            String text = String.valueOf(mListRing.get(i).getName());


            float textlength = getFontlength(mPaintText, text);

            float textlength_one = textlength * 1.0f / text.length();

            float showtextlength = (float) (arc_length
                    - textvalue_length * 1.8f);
            if (showtextlength < 0)
                showtextlength = 0;

            float textsize = showtextlength / textlength_one;

            if (textsize > text.length()) {
                textsize = text.length();
            } else if (textsize < 1) {
                textsize = 0;
            }

            canvas.drawTextOnPath(text.substring(0, (int) textsize)
                    ,
                    pathProgress,

                    10, getFontHeight(mPaintText) / 3,
                    mPaintText

            );


        }

    }
```
主要是计算，计算进度，计算文字的长度的控制。
3，在**onTouchEvent**时候注意因为有一个值是旋转的角度，在计算的时候不要忘记。

4，使用方式 xml
```
<com.ldoublem.ringPregressLibrary.RingProgress
        android:id="@+id/ring_progress"
        android:layout_width="200dp"
        android:layout_height="200dp"
        app:bgColor="#707070"
        app:bgShadowColor="#ffffff"
        app:ringSweepAngle="360"
        app:ringWidthScale="0.75"
        app:rotate="270"
        app:showBackground="true"
        app:showBackgroundShadow="true"
        app:showRingCorner="true"
        >
  </com.ldoublem.ringPregressLibrary.RingProgress>
```
bgColor：背景颜色
bgShadowColor：阴影的颜色
ringSweepAngle：圆环弧度值 360就是一个圆
ringWidthScale：每个进度条的宽度比例，0是最大，1是最小
rotate：选择角度
showBackground：是否显示背景
showBackgroundShadow：是否显示背景的阴影
showRingCorner：进度条圆角处理

java
```
      mRingProgress.setSweepAngle(360f);
      mRingProgress.setDrawBg(true, Color.rgb(168, 168, 168));
      mRingProgress.setDrawBgShadow(true, Color.argb(100, 235, 79, 56));
      mRingProgress.setCorner(true);
      mRingProgress.setOnSelectRing(new OnSelectRing() {
          @Override
          public void Selected(Ring r) {

          }
      });
       Ring r = new Ring(progress,text,title,startColor,endColor);
       List<Ring> mlistRing = new ArrayList<>();
       mlistRing.add(r);
       mRingProgress.setData(mlistRing, 1000);// if >0 animation ==0 null

```


[代码下载](https://github.com/ldoublem/RingProgress)感受下吧，如果觉得还可以就给颗星星吧^^