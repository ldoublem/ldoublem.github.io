---
title: 航班机票座位选择-android
date: 2016-07-29 10:01
tags: 自定义控件
---
点子出自[dribbble](https://dribbble.com/shots/2817989-Aircraft-zoom-interaction-for-travel-app-product-by-fantasy)
在这个基础上加入了缩略图
[下载代码](https://github.com/ldoublem/FlightSeat)

![seat_png.png](http://upload-images.jianshu.io/upload_images/1194532-42f542a2fdb7f76c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![seat_gif.gif](http://upload-images.jianshu.io/upload_images/1194532-a61f72c0e0263bff.gif?imageMogr2/auto-orient/strip)
定义枚举把座位分为已选，要选，未选。
座位分左中右
舱位分头等，二等，经济，末尾。
```
   enum SeatState {
        Normal, Selected, Selecting
    }

   enum SeatType {
        Left, Middle, Right
    }
  
   enum CabinType {
        Frist, Second, Tourist, Last
    }
```
定义**HashMap**来存放位置信息
```
HashMap<String, RectF> mSeats= new HashMap<>();
```
HashMap的key是位置的序号，比如4排5坐   「4#5」未做唯一标识。
value的类型是RectF来判断点是否在改区域中，来控制选中和未选中的区分。
```
RectF.contains(x, y)
```
利用**Matrix**对机舱和机翼放缩和平移
机舱和机翼都是利用贝赛尔曲线画的，这个就不具体贴代码了
```
 matrix.postScale(sx,sy);
 matrix.postTranslate(dx,dy);
 canvas.drawBitmap(getBitmapFuselage(rectFCabinWidth), matrix, mPaint);
```
```
public Bitmap getBitmapFuselage(float rectFCabinWidth) {
        Canvas canvas = null;

        int w = getMeasuredWidth();
        int h = getMeasuredHeight();


        if (mBitmapFuselage == null) {
            mBitmapFuselage = Bitmap.createBitmap(w, h, Bitmap.Config.ARGB_8888);
            canvas = new Canvas(mBitmapFuselage);
            pathFuselage.moveTo(w / 2f - rectFCabinWidth / 2f - dip2px(2),
                    rectFCabin.top + rectFCabinWidth / 2f);
            pathFuselage.cubicTo(
                    w / 2f - rectFCabinWidth / 4f,

                    rectFCabin.top - rectFCabinWidth * 1.2f,
                    w / 2f + rectFCabinWidth / 4f,
                    rectFCabin.top - rectFCabinWidth * 1.2f,
                    w / 2f + rectFCabinWidth / 2f + dip2px(2),
                    rectFCabin.top + rectFCabinWidth / 2f
            );


            rectFCabin.top = rectFCabin.top + dip2px(10);//机翼向下平移距离
            pathFuselage.lineTo(w / 2f + rectFCabinWidth / 2f + dip2px(2)
                    , rectFCabin.top + rectFCabin.height() / 3f);

            pathFuselage.lineTo(w
                    , rectFCabin.top + rectFCabin.height() * 0.55f);

            pathFuselage.lineTo(w
                    , rectFCabin.top + rectFCabin.height() * 0.55f + rectFCabin.width() * 0.8f);

            pathFuselage.lineTo(rectFCabin.right + rectFCabin.width() / 2 * 1.5f
                    , rectFCabin.top + rectFCabin.height() / 2f + rectFCabin.height() / 6f / 2f);

            pathFuselage.lineTo(w / 2f + rectFCabinWidth / 2f + dip2px(2)
                    , rectFCabin.top + rectFCabin.height() / 2f + rectFCabin.height() / 6f / 2f);
//
            pathFuselage.lineTo(w / 2f + rectFCabinWidth / 2f + dip2px(2)
                    , rectFCabin.bottom - rectFCabinWidth / 2f);


            pathFuselage.cubicTo(
                    w / 2f + rectFCabinWidth / 4f,

                    rectFCabin.bottom + rectFCabinWidth * 2.5f,
                    w / 2f - rectFCabinWidth / 4f,
                    rectFCabin.bottom + rectFCabinWidth * 2.5f,
                    w / 2f - rectFCabinWidth / 2f - dip2px(2)
                    , rectFCabin.bottom - rectFCabinWidth / 2f
            );

            pathFuselage.lineTo(w / 2f - rectFCabinWidth / 2f - dip2px(2)
                    , rectFCabin.top + rectFCabin.height() / 2f + rectFCabin.height() / 6f / 2f);
            pathFuselage.lineTo(rectFCabin.left - rectFCabin.width() / 2 * 1.5f
                    , rectFCabin.top + rectFCabin.height() / 2f + rectFCabin.height() / 6f / 2f);
            pathFuselage.lineTo(0
                    , rectFCabin.top + rectFCabin.height() * 0.55f + rectFCabin.width() * 0.8f);

            pathFuselage.lineTo(0
                    , rectFCabin.top + rectFCabin.height() * 0.55f);

            pathFuselage.lineTo(w / 2f - rectFCabinWidth / 2f - dip2px(2)
                    , rectFCabin.top + rectFCabin.height() / 3f);
            pathFuselage.close();
            mPaint.setColor(Color.WHITE);
            mPaint.setAlpha(150);
            canvas.drawPath(pathFuselage, mPaint);
        }

        return mBitmapFuselage;
    }
```
自定义手势滑动监听
```
public abstract class MoveListerner implements OnTouchListener,
        OnGestureListener {
    public static final int MOVE_TO_LEFT = 0;
    public static final int MOVE_TO_RIGHT = MOVE_TO_LEFT + 1;
    public static final int MOVE_TO_UP = MOVE_TO_RIGHT + 1;
    public static final int MOVE_TO_DOWN = MOVE_TO_UP + 1;

    private static final int FLING_MIN_DISTANCE = 150;
    private static final int FLING_MIN_VELOCITY = 50;
    private boolean isScorllStart = false;
    private boolean isUpAndDown = false;
    GestureDetector mGestureDetector;
    float x1 = 0;
    float x2 = 0;
    float y1 = 0;
    float y2 = 0;

    public MoveListerner(Activity context) {
        super();
        mGestureDetector = new GestureDetector(context, this);
    }

    float startX = 0;
    float startY = 0;


    @Override
    public boolean onTouch(View v, MotionEvent event) {
        if (mGestureDetector.onTouchEvent(event)) {
            if (MotionEvent.ACTION_DOWN == event.getAction()) {
//                Touch(event.getX(), event.getY());
                startX = event.getX();
                startY = event.getY();
                return true;
            } else if (MotionEvent.ACTION_UP == event.getAction()) {
                if (Math.abs(event.getX() - startX) < 5 &&
                        Math.abs(event.getY() - startY) < 5) {
                    Touch(event.getX(), event.getY());
                    return true;

                }

            }
        }
        // 处理手势结束
        switch (event.getAction() & MotionEvent.ACTION_MASK) {
            case MotionEvent.ACTION_UP:
                endGesture();
                break;
        }
        return false;
    }

    private void endGesture() {
        isScorllStart = false;
        isUpAndDown = false;
//        Log.e("a", "AA:over");
        moveOver();
    }

    public abstract void moveDirection(View v, int direction, float distanceX, float distanceY);

    public abstract void moveUpAndDownDistance(MotionEvent event, int distance, int distanceY);

    public abstract void moveOver();

    public abstract void Touch(float x, float y);


    @Override
    public boolean onDown(MotionEvent e) {
        // TODO Auto-generated method stub
        return true;
    }

    @Override
    public void onShowPress(MotionEvent e) {
        // TODO Auto-generated method stub

    }

    @Override
    public boolean onSingleTapUp(MotionEvent e) {
        // TODO Auto-generated method stub
        return true;
    }

    @Override
    public void onLongPress(MotionEvent e) {
        // TODO Auto-generated method stub

    }

    @Override
    public boolean onScroll(MotionEvent e1, MotionEvent e2, float distanceX,
                            float distanceY) {
        float mOldY = e1.getY();
        int y = (int) e2.getRawY();
        if (!isScorllStart) {
            if (Math.abs(distanceX) / Math.abs(distanceY) > 2) {
                // 左右滑动
                isUpAndDown = false;
                isScorllStart = true;
            } else if (Math.abs(distanceY) / Math.abs(distanceX) > 3) {
                // 上下滑动
                isUpAndDown = true;
                isScorllStart = true;
            } else {
                isScorllStart = false;
            }
        } else {
            // 算滑动速度的问题了
            if (isUpAndDown) {
                // 是上下滑动，关闭左右检测
                if (mOldY + 5 < y) {
                    moveUpAndDownDistance(e2, -3, (int) distanceY);
                } else if (mOldY + 5 > y) {
                    moveUpAndDownDistance(e2, 3, (int) distanceY);
                }
            }
        }
        return true;
    }

    @Override
    public boolean onFling(MotionEvent e1, MotionEvent e2, float velocityX,
                           float velocityY) {
//        Log.e("a", "AA:A" + velocityX + ":" + velocityY);
        if (isUpAndDown)
            return false;
        if (e1.getX() - e2.getX() > FLING_MIN_DISTANCE
                && Math.abs(velocityX) > FLING_MIN_VELOCITY) {
            // Fling left
            moveDirection(null, MOVE_TO_LEFT, e1.getX() - e2.getX(), e1.getY() - e2.getY());
        } else if (e2.getX() - e1.getX() > FLING_MIN_DISTANCE
                && Math.abs(velocityX) > FLING_MIN_VELOCITY) {
            // Fling right
            moveDirection(null, MOVE_TO_RIGHT, e2.getX() - e1.getX(), e2.getY() - e1.getY());
        } else if (e1.getY() - e2.getY() > FLING_MIN_DISTANCE
                && Math.abs(velocityY) > FLING_MIN_VELOCITY) {
            // Fling up
            moveDirection(null, MOVE_TO_UP, 0, e1.getY() - e2.getY());
        } else {
            // Fling down
            moveDirection(null, MOVE_TO_DOWN, 0, e2.getY() - e1.getY());
        }
        return false;
    }
}
```
#Usage
```
        mFlightSeatView.startAnim(true);// false zoom in,true zoom out
        mFlightSeatView.setEmptySelecting();//clear 
        mFlightSeatView.setSeatSelected(row, column);
```
如果觉得还不错，就给我打赏颗星星吧^^
[代码下载](https://github.com/ldoublem/FlightSeat)
