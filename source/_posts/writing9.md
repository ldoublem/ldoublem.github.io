---
title: 模仿android版instagram双击点赞效果
date: 2016-05-13 12:15
tags: view 
---
先上效果图
![效果图.gif](http://upload-images.jianshu.io/upload_images/1194532-5454c2399960860a.gif?imageMogr2/auto-orient/strip)

实现双击的效果，然后将点赞的动画体现出来。
#1 双击效果
Android sdk给我们提供了*GestureDetecto*这个类，GestureDetector这个类对外提供了两个接口：*OnGestureListener*，*OnDoubleTapListener*，还有一个内部类*SimpleOnGestureListener*。

```
public class OnDoubleClick extends GestureDetector.SimpleOnGestureListener {

		@Override
		public boolean onDoubleTap(MotionEvent e) {
			//双击
			return false;
		}

		public boolean onSingleTapConfirmed(MotionEvent e) {
            //单击
			return false;
		}

	}

```
实例化GestureDetector
```
GestureDetector mGestureDetector = new GestureDetector(this, new OnDoubleClick());
```
将view的onTouch监听捕捉到
```
view.setOnTouchListener(new OnTouchListener() {

			@Override
			public boolean onTouch(View view, MotionEvent event) {
				return mGestureDetector.onTouchEvent(event);
			}
		});
```
双击操作捕捉到后就开始动画效果，这里用简单的ScaleAnimation
```
ScaleAnimation animation_ScaleAnimation = new ScaleAnimation(1.0f, 1.5f, 1.0f, 1.5f,
			Animation.RELATIVE_TO_SELF, 0.5f, Animation.RELATIVE_TO_SELF, 0.5f);
animation_ScaleAnimation.setDuration(600);
```