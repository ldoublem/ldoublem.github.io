---
title: 一个酷炫的礼物卡片控件android
date: 2016-07-15 15:36
tags: 自定义控件
---
灵感来自[dribbble](https://dribbble.com/shots/2045026-Gift-Card)
[代码下载](https://github.com/ldoublem/GiftCard)
效果如下：



![shot.gif](http://upload-images.jianshu.io/upload_images/1194532-13dab8258292ed73.gif?imageMogr2/auto-orient/strip)


![shot1.png](http://upload-images.jianshu.io/upload_images/1194532-1a81a7e2275d1c28.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![shot2.png](http://upload-images.jianshu.io/upload_images/1194532-f18b35654ea4c653.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


核心代码主要是打包礼物按钮的动画，分解下有三步
1，背景有四个三角形将按钮不同方向围住
2，丝带不同方向连接到一起
3，蝴蝶结显示，中间小圆有小变大
具体的代码就不贴了，可以下载源码看下
[代码下载](https://github.com/ldoublem/GiftCard)
使用方式
```
compile 'com.ldoublem.GiftCard:giftcardlib:0.3'


```
```
 <com.ldoblem.giftcardlib.GiftCardView
                android:layout_width="300dp"
                android:layout_height="200dp"
                card:bgStartColor="#11cd6e"
                card:buyButtonColor="#11cd6e"
                card:cardBgColor="#33475f"
                card:buttonByText="购买"
                card:cardGiftTitle="礼物卡"
                card:cardGiftLogo="@mipmap/ic_launcher"
                card:buttonCheckText="确定"
                card:checkButtonColor="#2c2c2c"
                card:bgPackColor="#56abe4"
                card:priceTextColor="#fff"/>
```
```
  mGiftCardView = (GiftCardView) findViewById(R.id.gc_shop);
        mGiftCardView.setMTitle("苹果礼券");
        mGiftCardView.setMPrice(188);
        mGiftCardView.setButtonBuyText("买");
        mGiftCardView.setButtonCheckText("确定");
        mGiftCardView.setCardTip("请检查你的购物单");
        mGiftCardView.setCardBgColor(Color.BLACK);
        mGiftCardView.setGiftLogo(R.mipmap.ic_launcher);
        mGiftCardView.setBgStartColor(Color.BLACK);
        mGiftCardView.setBgEndColor(Color.BLACK);
        mGiftCardView.setBuyButtonColor(Color.BLACK);
        mGiftCardView.setCheckButtonColor(Color.BLACK);
        mGiftCardView.setPriceTextColor(Color.BLACK);
        mGiftCardView.setBgPackBgColor(Color.BLACK);
        mGiftCardView.setOnCheckOut(new GiftCardView.Buyer("陆先生", "中国浙江省",
                        "杭州市,西湖区,南山路100号", "有效期:3天"),
                new GiftCardView.OnCheckOut() {
                    @Override
                    public void Ok(int vid) {

                    }
                });
```
[代码下载](https://github.com/ldoublem/GiftCard)
如果觉得还不错，就在github给我颗星星吧^^