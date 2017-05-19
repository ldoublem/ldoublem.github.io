---
title: android 6.0中的FloatingActionButton，TextInputLayout，Snackbar
date: 2016-05-10 20:40
tags: 
---
必须说下6.0里面的新控件
#1 FloatingActionButton
```
<android.support.design.widget.FloatingActionButton
        android:id="@+id/fab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom|end"
        android:layout_margin="@dimen/fab_margin"
        android:src="@android:drawable/ic_dialog_email" />
```
默认背景颜色是AppTheme中colorAccent的值，如果要修改背景可以用
```
 app:backgroundTint="@color/colorPrimary"

```
使用android:background="@color/colorPrimary"会报错，在API>19无阴影效果
加入
```
app:borderWidth="0dp"
```


#2 TextInputLayout
![device-2016-05-10-202419.png](http://upload-images.jianshu.io/upload_images/1194532-271e88c857cdc5b1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

TextInputLayout继承了LinearLayout，所有要在布局里面加入EditText。下划线的颜色依然是AppTheme中colorAccent的值

```
  <android.support.design.widget.TextInputLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content">
                <EditText
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:hint="@string/prompt_password"
                    android:inputType="textPassword"
                    android:maxLines="1"
                    android:singleLine="true" /> 
   </android.support.design.widget.TextInputLayout>
```

#3 Snackbar
```
  Snackbar.make(view, "Replace with your own action", Snackbar.LENGTH_LONG)
                        .setAction("Action", null).show();
```
使用方式和Toast一样，但是效果好看多了。