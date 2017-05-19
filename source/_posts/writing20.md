---
title: textview文字处理，类似微博话题，@，超链接
date: 2016-06-12 13:18
tags: TextView
---
先上效果图
![效果图.gif](http://upload-images.jianshu.io/upload_images/1194532-06481c526bb3d32e.gif?imageMogr2/auto-orient/strip)

利用**Linkify**类的**addLinks**方法加入正则表达式。以网址为例
```
  String scheme = "http://";
  Linkify.addLinks(spanableInfo_temp,
                Pattern.compile("http://[a-zA-Z0-9+&@#/%?=~_\\-|!:,\\.;]*[a-zA-Z0-9+&@#/%=~_|]"),
                scheme);
```
**SpannableStringBuilder**的**setSpan**方法设置点击效果
```
 URLSpan[] urlSpans = spanableInfo_temp.getSpans(0, spanableInfo_temp.length(),
                URLSpan.class);
        for (URLSpan urlSpan : urlSpans) {
            int start = spanableInfo_temp.getSpanStart(urlSpan);
            int end = spanableInfo_temp.getSpanEnd(urlSpan);

            spanableInfo.setSpan(new Clickable(c, urlSpan.getURL(), color,mOnClickString), start, end,
                    Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);

        }
```
设置点击文字监听
```
textView.setMovementMethod(new LinkTouchMovementMethod());
```
完整的代码如下：
```
package com.ldoublem.texturlutil;
import java.util.regex.Pattern;
import android.content.Context;
import android.text.Layout;
import android.text.Selection;
import android.text.Spannable;
import android.text.SpannableString;
import android.text.SpannableStringBuilder;
import android.text.Spanned;
import android.text.TextPaint;
import android.text.method.LinkMovementMethod;
import android.text.style.ClickableSpan;
import android.text.style.URLSpan;
import android.text.util.Linkify;
import android.view.MotionEvent;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.TextView;


public class TextUrlUtil {

    public interface OnClickString {
        void OnClick(String s);
    }

    public static void dealContent(Spanned content, TextView textView, int color, OnClickString mOnClickString) {

        textView.setText(getClickableSpan(textView.getContext(), content, color,mOnClickString));
        textView.setMovementMethod(new LinkTouchMovementMethod());
    }


    private static SpannableStringBuilder getClickableSpan(Context c, Spanned content, int color,OnClickString mOnClickString) {

        SpannableStringBuilder spanableInfo = new SpannableStringBuilder(content);

        SpannableString spanableInfo_temp = new SpannableString(content);

        // 网页链接
        String scheme = "http://";
        Linkify.addLinks(spanableInfo_temp,
                Pattern.compile("http://[a-zA-Z0-9+&@#/%?=~_\\-|!:,\\.;]*[a-zA-Z0-9+&@#/%=~_|]"),
                scheme);

        // 网页链接
        String scheme2 = "https://";
        Linkify.addLinks(spanableInfo_temp,
                Pattern.compile("https://[a-zA-Z0-9+&@#/%?=~_\\-|!:,\\.;]*[a-zA-Z0-9+&@#/%=~_|]"),
                scheme2);

        String schemephone = "phone";
        Linkify.addLinks(spanableInfo_temp, Pattern.compile("\\d{11}"), schemephone);
        String schemeuser = "user";
        Linkify.addLinks(spanableInfo_temp, Pattern.compile("@[\\w\\p{InCJKUnifiedIdeographs}-]{1,26}"), schemeuser);

        String subject = "subject";
        Linkify.addLinks(spanableInfo_temp, Pattern.compile("#[^#]+#"), subject);


        //国内号码
        String schemephone2 = "fixphone";
        Linkify.addLinks(spanableInfo_temp, Pattern.compile("\\d{3}-\\d{8}|\\d{4}-\\d{8}"), schemephone2);

        URLSpan[] urlSpans = spanableInfo_temp.getSpans(0, spanableInfo_temp.length(),
                URLSpan.class);
        for (URLSpan urlSpan : urlSpans) {
            int start = spanableInfo_temp.getSpanStart(urlSpan);
            int end = spanableInfo_temp.getSpanEnd(urlSpan);

            spanableInfo.setSpan(new Clickable(c, urlSpan.getURL(), color,mOnClickString), start, end,
                    Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);

        }

        return spanableInfo;
    }

}

class Clickable extends ClickableSpan implements OnClickListener {
    Context cont;
    String s;

    public int textcolor = 0xffeeeeee;

    private boolean mIsPressed;
    TextUrlUtil.OnClickString mOnClickString;
    public Clickable(Context c, String string, int color,TextUrlUtil.OnClickString mOnClickString) {
        cont = c;
        s = string;
        this.mOnClickString=mOnClickString;
        if (color != -100)
            textcolor = cont.getResources().getColor(color);
        else
            textcolor = cont.getResources().getColor(R.color.colorPrimary);
    }

    public void setPressed(boolean isSelected) {
        mIsPressed = isSelected;
    }

    @Override
    public void onClick(View v) {
        v.setTag("false");
        mOnClickString.OnClick(s);
    }

    @Override
    public void updateDrawState(TextPaint tp) {
        // TODO Auto-generated method stub
        tp.setColor(mIsPressed ? cont.getResources().getColor(android.R.color.black) : textcolor);
        tp.bgColor = mIsPressed ? 0xffeeeeee : cont.getResources().getColor(android.R.color.transparent);
        tp.setUnderlineText(false);//设置下划线
        tp.clearShadowLayer();
    }
}

class LinkTouchMovementMethod extends LinkMovementMethod {
    private Clickable mPressedSpan;


    @Override
    public boolean onTouchEvent(TextView textView, Spannable spannable, MotionEvent event) {
        if (event.getAction() == MotionEvent.ACTION_DOWN) {
            mPressedSpan = getPressedSpan(textView, spannable, event);
            if (mPressedSpan != null) {
                mPressedSpan.setPressed(true);
                Selection.setSelection(spannable, spannable.getSpanStart(mPressedSpan),
                        spannable.getSpanEnd(mPressedSpan));
            }
        } else if (event.getAction() == MotionEvent.ACTION_MOVE) {
            Clickable touchedSpan = getPressedSpan(textView, spannable, event);
            if (mPressedSpan != null && touchedSpan != mPressedSpan) {
                mPressedSpan.setPressed(false);
                mPressedSpan = null;
                Selection.removeSelection(spannable);
            }
        } else {
            if (mPressedSpan != null) {
                mPressedSpan.setPressed(false);
                super.onTouchEvent(textView, spannable, event);
            }
            mPressedSpan = null;
            Selection.removeSelection(spannable);
        }
        return true;
    }

    private Clickable getPressedSpan(TextView textView, Spannable spannable, MotionEvent event) {

        int x = (int) event.getX();
        int y = (int) event.getY();

        x -= textView.getTotalPaddingLeft();
        y -= textView.getTotalPaddingTop();

        x += textView.getScrollX();
        y += textView.getScrollY();

        Layout layout = textView.getLayout();
        int line = layout.getLineForVertical(y);
        int off = layout.getOffsetForHorizontal(line, x);

        Clickable[] link = spannable.getSpans(off, off, Clickable.class);
        Clickable touchedSpan = null;
        if (link.length > 0) {
            touchedSpan = link[0];
        }
        return touchedSpan;

    }


}


```
使用方式
```
 SpannableString s = new SpannableString("hello my name is @ldoublem,my phone is 13511111111 and 05718888888," +
                "\nmy Sina Blog is https://weibo.com/u/1791039083 " +
                "\n这是个话题" +
                "#hello world#" +
                ""+"#你好# #世界#");
 TextUrlUtil.dealContent(s, tv_text, R.color.colorPrimaryDark, new TextUrlUtil.OnClickString() {
            @Override
            public void OnClick(String s) {
                //  处理不同的点击操作
            }
        });
```