---
title: 碎纸机for android
date: 2016-07-19 14:19
tags: 自定义控件
---
点子出自[dribbble](https://dribbble.com/shots/2125581-Deleting-AE-Freebie)
[swift版本](https://github.com/Fnoz/FNPaperShredder)
[android版本](https://github.com/ldoublem/PaperShredder)
以下是效果展示


![shot.png](http://upload-images.jianshu.io/upload_images/1194532-a6b5050e237780f0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![shot1.gif](http://upload-images.jianshu.io/upload_images/1194532-ddb339e4790e4b23.gif?imageMogr2/auto-orient/strip)

![shot2.gif](http://upload-images.jianshu.io/upload_images/1194532-d79058e887f16c8b.gif?imageMogr2/auto-orient/strip)


![shot3.gif](http://upload-images.jianshu.io/upload_images/1194532-8884aa38dcda7436.gif?imageMogr2/auto-orient/strip)




纸条模式的代码
```
private void drawPaperSlip(Canvas canvas) {
        Path PaperSlip = new Path();
        float paperSlipwidth = rectFPaper.width() / paperSlipCount;
        float paperSlipspace = paperSlipwidth / 7f;

        float animatedValue = mAnimatedValue;
        mPaint.setColor(paperColor);
        for (int i = 0; i < randomQuadHighs.size(); i++) {
            PaperSlip.reset();
            PaperSlip.moveTo(rectFPaper.left + i * paperSlipwidth + paperSlipspace,
                    rectFPaper.bottom
            );
            PaperSlip.lineTo(rectFPaper.left + (i + 1) * paperSlipwidth - paperSlipspace,
                    rectFPaper.bottom
            );

            float paperSlipHigh = rectFPaper.height() * 2 / 3f
                    + rectFPaper.height() / 3f * randomHighs.get(i);
            float randomQuadWidth = this.randomQuadWidths.get(i) * paperSlipspace * 2.5f * animatedValue;

            if (i % 2 == 0) {
                randomQuadWidth = randomQuadWidth * -1;

            }
            float randomQuadHigh = this.randomQuadHighs.get(i) * paperSlipHigh * animatedValue;


            PaperSlip.quadTo(
                    rectFPaper.left + (i + 1) * paperSlipwidth - paperSlipspace -
                            randomQuadWidth,
                    rectFPaper.bottom + randomQuadHigh,

                    rectFPaper.left + (i + 1) * paperSlipwidth - paperSlipspace,
                    rectFPaper.bottom + paperSlipHigh
            );


            PaperSlip.lineTo(rectFPaper.left + (i) * paperSlipwidth + paperSlipspace,
                    rectFPaper.bottom + paperSlipHigh
            );
            PaperSlip.quadTo(
                    rectFPaper.left + i * paperSlipwidth + paperSlipspace -
                            randomQuadWidth,
                    rectFPaper.bottom + randomQuadHigh,


                    rectFPaper.left + i * paperSlipwidth + paperSlipspace,
                    rectFPaper.bottom
            );
            PaperSlip.close();
            canvas.drawPath(PaperSlip, mPaint);

        }

    }
```
纸片模式
```
 private void drawPaperPiece(Canvas canvas) {


        if (mAnimatedValue == 0)
            return;


        float paperPiecewidth = rectFPaper.width() / paperSlipCount;

        Path paperPiece = new Path();


        for (int i = 0; i < PaperPiecerandAngle.size(); i++) {

            int angle = (int) (PaperPiecerandAngle.get(i) * 360);

            angle = (int) (angle + 180 * mAnimatedValue);
            float radius = paperPiecewidth * 0.8f;
            if (PaperPiecerandRadius.get(i) < 0.3f) {
                PaperPiecerandRadius.set(i,0.3f);

            }
            radius = radius * PaperPiecerandRadius.get(i);

            float centerX = mPaddingLR * 3 + (getMeasuredWidth() - 6 * mPaddingLR) * PaperPiecerandX.get(i);
            float centerY = getMeasuredHeight() / 3 * PaperPiecerandY.get(i) + getMeasuredHeight() / 3

                    + mAnimatedValue * getMeasuredHeight() / 3;
            if (PaperPiecerandRadius.get(i) >= 0.3f && PaperPiecerandRadius.get(i) <= 0.4f) {
                centerY = getMeasuredHeight() / 3 * PaperPiecerandY.get(i) + getMeasuredHeight() / 3

                        + mAnimatedValue * getMeasuredHeight() / 3 * 2.8f;
            } else if (PaperPiecerandRadius.get(i) > 0.4f && PaperPiecerandRadius.get(i) <= 0.5f) {
                centerY = getMeasuredHeight() / 3 * PaperPiecerandY.get(i) + getMeasuredHeight() / 3

                        + mAnimatedValue * getMeasuredHeight() / 3 * 2.6f;
            } else if (PaperPiecerandRadius.get(i) > 0.5f && PaperPiecerandRadius.get(i) <= 0.6f) {
                centerY = getMeasuredHeight() / 3 * PaperPiecerandY.get(i) + getMeasuredHeight() / 3

                        + mAnimatedValue * getMeasuredHeight() / 3 * 2.4f;
            } else if (PaperPiecerandRadius.get(i) > 0.6f && PaperPiecerandRadius.get(i) <= 0.7f) {
                centerY = getMeasuredHeight() / 3 * PaperPiecerandY.get(i) + getMeasuredHeight() / 3

                        + mAnimatedValue * getMeasuredHeight() / 3 * 2.2f;
            } else if (PaperPiecerandRadius.get(i) > 0.7f && PaperPiecerandRadius.get(i) <= 0.8f) {
                centerY = getMeasuredHeight() / 3 * PaperPiecerandY.get(i) + getMeasuredHeight() / 3

                        + mAnimatedValue * getMeasuredHeight() / 3 * 2.4f;
            } else if (PaperPiecerandRadius.get(i) > 0.8f && PaperPiecerandRadius.get(i) <= 0.9f) {
                centerY = getMeasuredHeight() / 3 * PaperPiecerandY.get(i) + getMeasuredHeight() / 3

                        + mAnimatedValue * getMeasuredHeight() / 3 * 2.6f;
            } else if (PaperPiecerandRadius.get(i) > 0.9f) {
                centerY = getMeasuredHeight() / 3 * PaperPiecerandY.get(i) + getMeasuredHeight() / 3

                        + mAnimatedValue * getMeasuredHeight() / 3 * 2.8f;
            }


            float x = (float) ((radius) * Math.cos(angle * Math.PI / 180f));
            float y = (float) ((radius) * Math.sin(angle * Math.PI / 180f));

            paperPiece.reset();
            paperPiece.moveTo(centerX - x,
                    centerY - y

            );


            int angleBig = 0;
            if (PaperPiecerandX.get(i) > 0.7f) {
                angleBig = (int) (180 * 0.7f);
            } else if (PaperPiecerandX.get(i) < 0.3f) {
                angleBig = (int) (180 * 0.3f);
            } else {
                angleBig = (int) (180 * PaperPiecerandX.get(i));
            }
            int angleSmall = 180 - angleBig;

            angle = angle + angleBig;
            x = (float) ((radius) * Math.cos(angle * Math.PI / 180f));
            y = (float) ((radius) * Math.sin(angle * Math.PI / 180f));
            paperPiece.lineTo(centerX - x,
                    centerY - y

            );


            angle = angle + angleSmall;
            x = (float) ((radius) * Math.cos(angle * Math.PI / 180f));
            y = (float) ((radius) * Math.sin(angle * Math.PI / 180f));
            paperPiece.lineTo(centerX - x,
                    centerY - y

            );

            angle = angle + angleBig;
            x = (float) ((radius) * Math.cos(angle * Math.PI / 180f));
            y = (float) ((radius) * Math.sin(angle * Math.PI / 180f));
            paperPiece.lineTo(centerX - x,
                    centerY - y

            );
            angle = angle + angleSmall;
            x = (float) ((radius) * Math.cos(angle * Math.PI / 180f));
            y = (float) ((radius) * Math.sin(angle * Math.PI / 180f));
            paperPiece.lineTo(centerX - x,
                    centerY - y

            );




            paperPiece.close();

            int color = (Integer) evaluator.evaluate(PaperPiecerandRadius.get(i),
                    Color.rgb(180, 180, 180), paperColor
            );

            mPaint.setColor(color);
            canvas.drawPath(paperPiece, mPaint);


        }
        mPaint.setColor(paperColor);
    }

```
使用方式xml
```
<com.ldoublem.PaperShredderlib.PaperShredderView
        android:layout_width="200dp"
        android:id="@+id/ps_delete2"
        android:layout_height="220dp"
        paper:sherderBgColor="#f4c600"
        paper:sherderText="清理中"
        paper:sherderType="0"
        paper:sherderPaperEnterColor="#56abe4"
        paper:sherderTextShadow="false"
        paper:sherderTextColor="#99101010"
        paper:sherderPaperColor="#dbdbdb"
        paper:sherderProgress="false"
      />
```
java
```
       mPaperShredderView.setShrededType(PaperShredderView.SHREDEDTYPE.Piece);//纸片效果和纸条效果
       mPaperShredderView.setSherderProgress(false);
       mPaperShredderView.setTitle("清除数据");
       mPaperShredderView.setTextColor(Color.BLACK);
       mPaperShredderView.setPaperColor(Color.BLACK);
       mPaperShredderView.setBgColor(Color.WHITE);
       mPaperShredderView.setTextShadow(false);
       mPaperShredderView.setPaperEnterColor(Color.BLACK);
       mPaperShredderView.startAnim(1000);
       mPaperShredderView.stopAnim();
```
如果觉得还不错，去github给我点颗星吧^^
[代码下载](https://github.com/ldoublem/PaperShredder)
