---
title: 自定义密码控件(九宫格手势，输入框)
date: 2016-06-16 23:20
tags: 自定义控件
---
因为项目需要，配合ui设计写了以下的控件，细节功能后期继续优化。

![效果.png](http://upload-images.jianshu.io/upload_images/1194532-3b2fc5a4033bf48f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![效果.gif](http://upload-images.jianshu.io/upload_images/1194532-b924b15875ed1a18.gif?imageMogr2/auto-orient/strip)
[下载代码](https://github.com/ldoublem/PassWordInput)
1 类似微信支付宝密码输入控件
圆角矩形
```
        RectF rect = new RectF(mPadding, mPadding, getMeasuredWidth() - mPadding, getMeasuredHeight() - mPadding);
        mPaint.setStrokeWidth(0.8f);
        canvas.drawRoundRect(rect, radiusBg, radiusBg, mPaint);
```
设置图片大小
```
         b = Bitmap.createScaledBitmap(bitmap, w, h, true);
```
获得edittext最大输入字符数
```
        public int getMaxLength() {

        int length = 10;

        try {

            InputFilter[] inputFilters = getFilters();

            for (InputFilter filter : inputFilters) {

                Class<?> c = filter.getClass();

                if (c.getName().equals("android.text.InputFilter$LengthFilter")) {

                    Field[] f = c.getDeclaredFields();

                    for (Field field : f) {

                        if (field.getName().equals("mMax")) {

                            field.setAccessible(true);

                            length = (Integer) field.get(filter);

                        }

                    }

                }

            }

        } catch (Exception e) {

            e.printStackTrace();

        }
        return length;

    }
```
重写**onTextChanged**方法
```
  @Override
    protected void onTextChanged(CharSequence text, int start, int lengthBefore, int lengthAfter) {
        super.onTextChanged(text, start, lengthBefore, lengthAfter);
        if (text.toString().length() - this.textLength >= 0) {
            addText = true;
        } else {
            addText = false;
        }

        this.textLength = text.toString().length();
        if (textLength <= getMaxLength()) {
            if (mPaintLastArcAnim != null) {
                clearAnimation();
                startAnimation(mPaintLastArcAnim);
            } else {
                invalidate();
            }
        }
    }
```
2 九宫格密码控件,画3*3的RectF和Circle
```
   @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);

        mPaint.setStyle(Paint.Style.STROKE);
        mPaint.setStrokeWidth(dip2px(1));
        mRectFButtons.clear();
        for (int i = 0; i < 3; i++)//行
        {

            for (int j = 0; j < 3; j++)//列
            {
                RectF fButton = new RectF();
                fButton.left = j * buttonWidth + buttonMarginLeft + buttonPadding;
                fButton.right = (j + 1) * buttonWidth + buttonMarginLeft - buttonPadding;
                fButton.top = i * buttonWidth + buttonMarginTop + buttonPadding;
                fButton.bottom = (i + 1) * buttonWidth + buttonMarginTop - buttonPadding;


                canvas.drawCircle(fButton.centerX(), fButton.centerY(), ArcWidth, mPaint);

                mRectFButtons.add(fButton);

            }


        }
        mPaint.setStyle(Paint.Style.FILL);

        for (int i = 0; i < mPwd.size(); i++) {
            RectF rf = mRectFButtons_select.get(mPwd.get(i));
            canvas.drawCircle(rf.centerX(), rf.centerY(), dip2px(5), mPaint);

//            isDrawLine=false;
            if (isDrawLine) {

                if (i > 0 && i < mPwd.size()) {
                    canvas.drawLine(mRectFButtons_select.get(mPwd.get(i - 1)).centerX(),
                            mRectFButtons_select.get(mPwd.get(i - 1)).centerY(),
                            mRectFButtons_select.get(mPwd.get(i)).centerX(),
                            mRectFButtons_select.get(mPwd.get(i)).centerY(),
                            mPaintLine
                    );
                }
                if (i == mPwd.size() - 1 && i < mRectFButtons.size() - 1) {
                    if (mlastX >= 0 && mlastY >= 0) {
                        canvas.drawLine(
                                mRectFButtons_select.get(mPwd.get(i)).centerX(),
                                mRectFButtons_select.get(mPwd.get(i)).centerY(),
                                mlastX, mlastY,
                                mPaintLine);
                    }
                }
            }

        }
    }
```
在**onTouchEvent**方法中控制路径
```
  @Override
    public boolean onTouchEvent(MotionEvent event) {
        int action = event.getAction();


        switch (action) {
            case MotionEvent.ACTION_DOWN:
                getParent().requestDisallowInterceptTouchEvent(true);//防止与父视图滑动冲突
                addButtonSelect(event.getX(), event.getY());
                postInvalidate();
                break;
            case MotionEvent.ACTION_MOVE:
                addButtonSelect(event.getX(), event.getY());
                postInvalidate();

                break;
            case MotionEvent.ACTION_UP:
            case MotionEvent.ACTION_CANCEL:
                getParent().requestDisallowInterceptTouchEvent(false);
                callGetPwd();
                mRectFButtons_select.clear();
                mRectFButtons.clear();
                mPwd.clear();
                postInvalidate();
                return false;
            default:
                break;
        }

        return true;
    }
```
添加按钮到选中的Map
```
 private void addButtonSelect(float x, float y) {
        mlastX = x;
        mlastY = y;

        for (int i = 0; i < mRectFButtons.size(); i++) {
            if (mRectFButtons.get(i).contains(x, y)) {
                if (getDistance(mRectFButtons.get(i).centerX(), mRectFButtons.get(i).centerY(), x, y) <= ArcWidth) {
                    if (!mRectFButtons_select.containsKey(String.valueOf(i))) {
                        mRectFButtons_select.put(String.valueOf(i), mRectFButtons.get(i));
                        mPwd.add(String.valueOf(i));
                        callGetPwd();
                        return;
                    }
                }
            }
        }
    }
```
错误手势提示错误方向
```
            Path p = new Path();
            p.moveTo(x1, y1);
            p.lineTo(x2, y2);
            canvas.drawTextOnPath("☞", p, getFontHeight(mPaintOrientation, "☞") *3/2,
                    getFontHeight(mPaintOrientation, "☞") / 2,
                    mPaintOrientation);
```
因为要计算手势的方向，这里偷懒下用**drawTextOnPath**将特殊符号作为方向。

新增了几个输入框的样式

![输入框样式.jpg](http://upload-images.jianshu.io/upload_images/1194532-a4c6032b5c2678f3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


[下载代码](https://github.com/ldoublem/PassWordInput)