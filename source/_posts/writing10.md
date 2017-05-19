---
title: 模仿简书ios版app个人信息界面
date: 2016-05-16 14:18
tags: view 
---
android版本简书app的个人信息界面没有像ios版本滑动个人信息头像随着滑动的距离改变大小。于是乎就模仿着ios版写了简单的例子。

![效果.gif](http://upload-images.jianshu.io/upload_images/1194532-8720410205132bca.gif?imageMogr2/auto-orient/strip)


我是直接使用了listview，然后根据HeaderView滑动的距离控制头像的大小和标签按钮的显示。
![布局.png](http://upload-images.jianshu.io/upload_images/1194532-52bd16ff95865083.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
 listview.setOnScrollListener(new OnScrollListener() {

            @Override
            public void onScrollStateChanged(AbsListView view, int scrollState) {
                // TODO Auto-generated method stub

            }

            @Override
            public void onScroll(AbsListView absListView, int firstVisibleItem,
                                 int visibleItemCount, int totalItemCount) {
                if (firstVisibleItem == 0) {
                  //headerview在屏幕范围内将标签栏隐藏
                  int scrollY=getScrollY();
                 //计算控件放大缩小的比例
                float f = 1.0f-(float) (scrollY * 1.0 / (头像的原始高度));
                if (f < 0.45f) {
                        f = 0.45f;//最小显示比例
                    } else if (f > 0.85f) {
                        f = 0.85f;//最大显示比例
                    }
                 int w = (int) (头像的原始高度  * f);
                 ly_logo.setLayoutParams(new LinearLayout.LayoutParams(w, w));//设置头像父类容器的高和宽
                 if (ly_logo.getChildCount() > 1) {
                        ly_logo.removeViewAt(1);//将上一次add过的view删除
                  }
                    iv_photo = new RoundedImageView(ActivityTest.this);
                    iv_photo.setImageResource(R.mipmap.icon);
                    ly_logo.addView(iv_photo);

                } else {
                  //headerview超出屏幕范围将标签栏显示
                }


            }
        });
```
获得滑动的高度
```
public int getScrollY() {

        View v = listview.getChildAt(0);

        int top = 0;

        if (v == null) {
            return 0;
        } else {
            top = v.getTop();

        }
        return top;
    }
```