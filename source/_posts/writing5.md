---
title: 如何在Fragment获得onActivityResult
date: 2016-05-07 11:13
tags: android
---
在activity中常常要用到onActivityResult回调获得相关数据，而有些操作是在Fragment里面的，这个时候需要在activity中先重写的onActivityResult方法，再将数据传给对应的Fragment。
```
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
       Fragment f = fragmentManager.findFragmentByTag(curFragmentTag);
       /*然后在碎片中调用重写的onActivityResult方法*/
       f.onActivityResult(requestCode, resultCode, data);
    }
```
最后在对应的Fragment重写onActivityResult。

这个方式和6.0权限管理一个道理，现在activity中重写onRequestPermissionsResult，然后再在对应Fragment中重写即可。