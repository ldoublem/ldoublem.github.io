---
title: android6.0权限管理
date: 2016-05-04 09:50
tags: android
---
在开发中很多用到手机拍照选择图片，在低版本中只要在AndroidManifest.xml中声明


```
<uses-permissionandroid:name="android.permission.CAMERA" />
```
即可，但是在6.0手机上运行就会直接挂掉，提示
```
Permission Denial: starting Intent {act=android.media.action.IMAGE_CAPTURE.....with revoked permission android.permission.CAMER
```
只要在代码中进行权限判断就好了，可以直接在主activity中或者在具体用到权限的地方。

```

if(ContextCompat.checkSelfPermission(
 activity,Manifest.permission.CAMERA)!=                                                            PackageManager.PERMISSION_GRANTED) 
{ActivityCompat.requestPermissions(activity,
new String[]{Manifest.permission.CAMERA},
1);
}

```

在activity中重写onRequestPermissionsResult方法，具体如下

```
@Override

public void onRequestPermissionsResult
(int requestCode,String[] permissions, int[] grantResults) {

if(requestCode == 1) {

      if(grantResults[0] == PackageManager.PERMISSION_GRANTED) {

      //权限获取成功

       }else{

       //权限被拒绝

       }

  }
```
