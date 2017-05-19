---
title: android自定义进度条，类似微信视频加载
date: 2016-05-26 12:59
tags: 自定义控件
---
要实现的效果如下
![效果.png](http://upload-images.jianshu.io/upload_images/1194532-71caa5fb6bc06d02.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![效果.gif](http://upload-images.jianshu.io/upload_images/1194532-00f57cf2ba19daee.gif?imageMogr2/auto-orient/strip)

简单的步骤利用canvas画三个不同大小的圆，相互叠加，显示成为进度条，然后利用**Animation**重写**applyTransformation**来控制进度条。
下面的代码
```
public class LodingCircleView extends View {


    private Paint paintBgCircle;


    private Paint paintCircle;

    private Paint paintProgressCircle;


    private float startAngle = -90f;//开始角度

    private float sweepAngle = 0;//结束

    private int progressCirclePadding = 0;//进度圆与背景圆的间距


    private boolean fillIn = false;//进度圆是否填充

    private int animDuration = 2000;


    private LodingCircleViewAnim mLodingCircleViewAnim;//动画效果


    public LodingCircleView(Context context) {
        super(context);
        init();
    }

    public LodingCircleView(Context context, AttributeSet attrs) {
        super(context, attrs);
        init();
    }

    public LodingCircleView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init();
    }

    public int dip2px(Context context, float dpValue) {
        final float scale = context.getResources().getDisplayMetrics().density;
        return (int) (dpValue * scale + 0.5f);
    }


    private void init() {

        mLodingCircleViewAnim = new LodingCircleViewAnim();
        mLodingCircleViewAnim.setDuration(animDuration);
        progressCirclePadding = dip2px(getContext(), 3);

        paintBgCircle = new Paint();
        paintBgCircle.setAntiAlias(true);
        paintBgCircle.setStyle(Paint.Style.FILL);
        paintBgCircle.setColor(Color.WHITE);


        paintCircle = new Paint();
        paintCircle.setAntiAlias(true);
        paintCircle.setStyle(Paint.Style.FILL);
        paintCircle.setColor(Color.BLACK);


        paintProgressCircle = new Paint();
        paintProgressCircle.setAntiAlias(true);
        paintProgressCircle.setStyle(Paint.Style.FILL);
        paintProgressCircle.setColor(Color.WHITE);

    }


    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        canvas.drawCircle(getMeasuredWidth() / 2, getMeasuredWidth() / 2, getMeasuredWidth() / 2, paintBgCircle);
        canvas.drawCircle(getMeasuredWidth() / 2, getMeasuredWidth() / 2, getMeasuredWidth() / 2 - progressCirclePadding / 2, paintCircle);
        RectF f = new RectF(progressCirclePadding, progressCirclePadding, getMeasuredWidth() - progressCirclePadding, getMeasuredWidth() - progressCirclePadding);
        canvas.drawArc(f, startAngle, sweepAngle, true, paintProgressCircle);
        if (!fillIn)
            canvas.drawCircle(getMeasuredWidth() / 2, getMeasuredWidth() / 2, getMeasuredWidth() / 2 - progressCirclePadding * 2, paintCircle);


    }


    public void startAnimAutomatic(boolean fillIn) {
        this.fillIn = fillIn;
        if (mLodingCircleViewAnim != null)
            clearAnimation();
        startAnimation(mLodingCircleViewAnim);
    }

    public void stopAnimAutomatic() {
        if (mLodingCircleViewAnim != null)
            clearAnimation();
    }


    public void setProgerss(int progerss, boolean fillIn) {
        this.fillIn = fillIn;
        sweepAngle = (float) (360 / 100.0 * progerss);
        invalidate();
    }


    private class LodingCircleViewAnim extends Animation {
        @Override
        protected void applyTransformation(float interpolatedTime, Transformation t) {
            super.applyTransformation(interpolatedTime, t);
            if (interpolatedTime < 1.0f) {
                sweepAngle = 360 * interpolatedTime;
                invalidate();
            } else {
                startAnimAutomatic(fillIn);
            }

        }
    }
}
```
