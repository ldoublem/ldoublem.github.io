---
title: Android view之点赞容易，取消不易
date: 2016-08-04 16:37
tags: 自定义控件
---
可以去[dribbble](https://dribbble.com/shots/2661577-Like-Unlike-microinteraction-for-Loliful-io)上看看原生效果。
[代码下载](https://github.com/ldoublem/ThumbUp)
好的app在功能完善的基础上，从细节上吸引用户。虽然点赞这个功能点已经很普遍了，但是千篇一律的生硬效果让这么神圣的操作显得很黯淡(扯淡了，不就是赞赞赞么...)，当然也有非常炫酷的，忍不住多点几次赞的效果。比如twitter的点赞。就不码字扯淡了，上图上源码

![like.png](http://upload-images.jianshu.io/upload_images/1194532-aab7348c03f1c05e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![shot.gif](http://upload-images.jianshu.io/upload_images/1194532-1e907d3359cd544c.gif?imageMogr2/auto-orient/strip)
使用**Path**画爱心
```
        Path path = new Path();
        path.moveTo((float) (0.5 * realWidth) + rectFlove.left, (float) (0.17 * realHeight) + rectFlove.top);
        path.cubicTo((float) (0.15 * realWidth) + rectFlove.left, (float) (-0.35 * realHeight + rectFlove.top),
                (float) (-0.4 * realWidth) + rectFlove.left, (float) (0.45 * realHeight) + rectFlove.top,
                (float) (0.5 * realWidth) + rectFlove.left, (float) (realHeight * 0.8 + rectFlove.top));
//        path.moveTo( (float) (0.5 * realWidth) + rectFlove.left, (float) (realHeight * 0.8 + rectFlove.top));
        path.cubicTo((float) (realWidth + 0.4 * realWidth) + rectFlove.left, (float) (0.45 * realHeight) + rectFlove.top,
                (float) (realWidth - 0.15 * realWidth) + rectFlove.left, (float) (-0.35 * realHeight) + rectFlove.top,
                (float) (0.5 * realWidth) + rectFlove.left, (float) (0.17 * realHeight) + rectFlove.top);
        path.close();
```
取消时候爱心出现裂痕然后分成两半分别向左右倾斜,使用** canvas**
rotate来旋转达到倾斜效果
```
        mBitmapBrokenLeftLove = Bitmap.createBitmap(getMeasuredWidth(), (int) lastY, Bitmap.Config.ARGB_8888);
        canvas = new Canvas(mBitmapBrokenLeftLove);
        canvas.rotate(-1*mBrokenAngle * mAnimatedBrokenValue, lastX, lastY);

        Path path = new Path();
        path.moveTo((float) (0.5 * realWidth) + rectFlove.left, (float) (0.17 * realHeight) + rectFlove.top);
        path.cubicTo((float) (0.15 * realWidth) + rectFlove.left, (float) (-0.35 * realHeight + rectFlove.top),
                (float) (-0.4 * realWidth) + rectFlove.left, (float) (0.45 * realHeight) + rectFlove.top,
                (float) (0.5 * realWidth) + rectFlove.left, (float) (realHeight * 0.8 + rectFlove.top));
        path.lineTo(thirdX, thirdY);
        path.lineTo(secondX, secondY);
        path.close();
        canvas.drawPath(path, mPaintLike);
```
Gradle
```
compile 'com.ldoublem.thumbUplib:ThumbUplib:0.2'
```
Usage xml
```
  <com.ldoublem.thumbUplib.ThumbUpView
            android:id="@+id/tpv"
            android:layout_width="50dp"
            android:layout_height="50dp"
            app:cracksColor="#33475f"
            app:edgeColor="#9d55b8"
            app:fillColor="#ea8010"
            app:unlikeType="1"
            />
```
``` mThumbUpView.setUnLikeType(ThumbUpView.LikeType.broken);
        mThumbUpView.setCracksColor(Color.rgb(22, 33, 44));
        mThumbUpView.setFillColor(Color.rgb(11, 200, 77));
        mThumbUpView.setEdgeColor(Color.rgb(33, 3, 219));
        mThumbUpView.setOnThumbUp(new ThumbUpView.OnThumbUp() {
            @Override
            public void like(boolean like) {
            }
        });
        mThumbUpView.Like();
        mThumbUpView.UnLike();
```



如果觉得还可以，给颗小星星^^
[代码下载](https://github.com/ldoublem/ThumbUp)