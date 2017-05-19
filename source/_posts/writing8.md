---
title: android 6.0导航栏 NavigationBar影响视图解决办法
date: 2016-05-11 13:41
tags:
---
在开发app的时候会遇到有些测试手机没有物理按钮，比如最近在做的一个app在小米手机上运行显示效果很好，但是在华为P7手机上显示就乱了，底部的NavigationBar直接覆盖在主视图上，导致按钮无法触发。
![正常效果.jpg](http://upload-images.jianshu.io/upload_images/1194532-c49bb9a15e360da5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![异常效果.jpg](http://upload-images.jianshu.io/upload_images/1194532-c34d8a006fd69084.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

解决的方法就是先判断手机是否有物理按钮，然后计算底部的NavigationBar高度，最后设置试图边距。
```
public int getNavigationBarHeight() {

        boolean hasMenuKey = ViewConfiguration.get(this).hasPermanentMenuKey();
		boolean hasBackKey = KeyCharacterMap.deviceHasKey(KeyEvent.KEYCODE_BACK);
           if (!hasMenuKey && !hasBackKey) {
                Resources resources = getResources();
		        int resourceId = resources.getIdentifier("navigation_bar_height", "dimen", "android");
		        //获取NavigationBar的高度  
		        int height = resources.getDimensionPixelSize(resourceId);
		        return height;
           }
           else{
                return 0;
          }
}
```
```
 getWindow().getDecorView().findViewById(android.R.id.content).setPadding(0, 0, 0, getNavigationBarHeight());
```
如果设置android.R.id.content的边距，底部是白色背景或者黑色（这个和你用的theme有关，黑色还能接受，白色显得app很不搭）
```
 <item name="android:windowBackground">@android:color/white</item>
```

![low.jpg](http://upload-images.jianshu.io/upload_images/1194532-6076d489630a9a38.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果想换其他的颜色，又不想修改windowBackground。那么可以在布局文件中的父试图设置你想要的颜色，然后再显示的时候设置该控件的padding属性。
