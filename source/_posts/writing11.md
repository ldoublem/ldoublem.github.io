---
title: 模仿简书ios版app个人信息界面（自定义控件）
date: 2016-05-17 15:17
tags: 自定义控件
---
上一篇是利用监听listview滑动的距离来实现要到达的效果，这篇文章来讲下自定义控件的思路。因为需要嵌套一个ViewPager，所以一定会遇到手势冲突的问题。


![布局.png](http://upload-images.jianshu.io/upload_images/1194532-e026a36de43545a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
大致的思路，自定义一个view继承LinearLayout，将高度设定为测量的高度+滑动超出屏幕的高度。
```
  @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        View  mHeadView = getChildAt(0);
        measureChildWithMargins(mHeadView, widthMeasureSpec, 0, MeasureSpec.UNSPECIFIED, 0);
        mHeadHeight = mHeadView.getMeasuredHeight();
        maxY = mHeadHeight;
        //让测量高度加上头部的高度
        super.onMeasure(widthMeasureSpec, MeasureSpec.makeMeasureSpec(MeasureSpec.getSize(heightMeasureSpec) + mHeadHeight, MeasureSpec.EXACTLY));
    }
```
然后重写dispatchTouchEvent方法，在这个里面控制滑动事件。如果滑动的高度在maxY范围内，fragment里面的listview也在顶部时，该view调用刷新。最后将事件传递给子View，让子View自己去处理事件，super.dispatchTouchEvent(ev);
重写computeScroll方法，确保手势快速上划滚动时的让布局看起来滚动连贯，下滑的时候内部View(listview)已经滚动到顶了，需要滚动外层的View。
重写scrollBy对滑动范围做限制，控制在0到maxY之间。
最后重写scrollTo，里面回调滑动的距离占mHeadView的高度的多少。这个只是为了显示头像大小的时候用到。

![效果.gif](http://upload-images.jianshu.io/upload_images/1194532-501584455d44fca4.gif?imageMogr2/auto-orient/strip)