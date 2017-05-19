---
title: 带有动画效果的注册控件
date: 2016-07-05 11:59
tags: 自定义控件
---
带有动画效果的注册控件
[代码下载](https://github.com/ldoublem/PassWordInput)




![signup.png](http://upload-images.jianshu.io/upload_images/1194532-78a4856386289498.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![signup.gif](http://upload-images.jianshu.io/upload_images/1194532-bbbfa0ba540f3f17.gif?imageMogr2/auto-orient/strip)



一共有4个动画效果
1，点击sign up转变成输入框
2，点击下一步
3，最后一步欢迎文字渐变
4，输入框非法抖动效果
canvas 画板
1，drawBg(canvas);
2，drawTitleAndText(canvas);
3，drawGoButton(canvas);
因为是继承了EditText，所以要监听**onTextChanged**,来重新绘制。如果文字超出view的宽度要将背景进行平移，如果文字在view宽度内，随文字长度显示宽度
```
        textWidth = getPaint().measureText(getText().toString().trim());
        float marLeft = 0f;


        if (mWidth - 2 * mPadding < textWidth) {
            marLeft = textWidth - mWidth;
            bgRectF.left = mPadding + marLeft;
            bgRectF.top = mPadding;
            bgRectF.right = mWidth - mPadding + marLeft;
            bgRectF.bottom = mHeight - mPadding;
            bgPaint.setStyle(Paint.Style.FILL);
            bgPaint.setColor(bgColor);
            canvas.drawRoundRect(bgRectF, bgRectF.height() / 2, bgRectF.height() / 2, bgPaint);
            bgPaint.setStyle(Paint.Style.STROKE);
            if (inputError)
                bgPaint.setColor(errorColor);
            else
                bgPaint.setColor(Color.WHITE);

            canvas.drawRoundRect(bgRectF, bgRectF.height() / 2, bgRectF.height() / 2, bgPaint);
        } else if (mWidth / 2 >= textWidth) {

            bgRectF.left = mWidth / 2 - mWidth / 4;
            bgRectF.right = mWidth / 2 + mWidth / 4;
            float w = 0f;
            if (mStep == 1) {
                w = (bgRectF.width() - bgRectF.height()) / 2 * (interpolatedTimeFirst);
                bgRectF.left = mWidth / 2 - mWidth / 4 + w;
                bgRectF.right = mWidth / 2 + mWidth / 4 - w;

            } else {//if (mStep == 2) {
                bgRectF.left = mWidth / 2 - mWidth / 4 - textWidth / 2f;
                bgRectF.right = mWidth / 2 + mWidth / 4 + textWidth / 2f;

            }

            bgRectF.top = mPadding;
            bgRectF.bottom = mHeight - mPadding;
            bgPaint.setStyle(Paint.Style.FILL);
            bgPaint.setColor(bgColor);
            canvas.drawRoundRect(bgRectF, bgRectF.height() / 2, bgRectF.height() / 2, bgPaint);
            bgPaint.setStyle(Paint.Style.STROKE);
            if (inputError)
                bgPaint.setColor(errorColor);
            else
                bgPaint.setColor(Color.WHITE);
            canvas.drawRoundRect(bgRectF, bgRectF.height() / 2, bgRectF.height() / 2, bgPaint);


        } else {
            marLeft = 0;
            bgRectF.left = mPadding + marLeft;
            bgRectF.top = mPadding;
            bgRectF.right = mWidth - mPadding + marLeft;
            bgRectF.bottom = mHeight - mPadding;
            bgPaint.setStyle(Paint.Style.FILL);
            bgPaint.setColor(bgColor);
            canvas.drawRoundRect(bgRectF, bgRectF.height() / 2, bgRectF.height() / 2, bgPaint);
            bgPaint.setStyle(Paint.Style.STROKE);

            if (inputError)
                bgPaint.setColor(errorColor);
            else
                bgPaint.setColor(Color.WHITE);
            canvas.drawRoundRect(bgRectF, bgRectF.height() / 2, bgRectF.height() / 2, bgPaint);
        }


```
如果文字超出最长宽度的距离，只显示能容纳最大字数的字符串后几位。
```
        canvastextlength = bgRectF.width() - bgRectF.height() * 2 - 10;
            if (canvastextlength < textWidth) {
            int count = (int) (textcount * canvastextlength / textWidth);
            canvasText = getText().toString().trim().substring(textcount - count);
            if (MInputType == InputType.EDITTEXT) {
                canvas.drawText(canvasText, bgRectF.centerX() - getFontlength(textPaint, canvasText) / 2f,
                        bgRectF.centerY() + getFontHeight(textPaint, canvasText) / 2, textPaint);
            } }
        } 
```
简单的抖动动画
```
           TranslateAnimation errorAnim = new TranslateAnimation(-3,
                    3, 0, 0);
            errorAnim.setInterpolator(new CycleInterpolator(2f));
            errorAnim.setDuration(400);
```
使用方式
```
        mSignUpInputView.setBgPaintColor(Color.BLACK);
        mSignUpInputView.setTextPaintColor(Color.WHITE);
        mSignUpInputView.setTitlePaintColor(Color.GRAY);
        mSignUpInputView.setSetpIcon(R.mipmap.user, R.mipmap.email, R.mipmap.pwd);//set icon
        mSignUpInputView.setSetpName("Name", "Email", "PassWord");//set title
        mSignUpInputView.setmButtonText("Sign up");//button text
        mSignUpInputView.setVerifyTypeStep(SignUpInputView.VerifyType.NULL,
                SignUpInputView.VerifyType.EMAIL,
                SignUpInputView.VerifyType.PASSWORD);
         mSignUpInputView.setOnGetStepInfo(new SignUpInputView.GetStepAndText() {
            @Override
            public void GetInfo(int step, String stepName, String text) {

            }
        });
```


[代码下载](https://github.com/ldoublem/PassWordInput)