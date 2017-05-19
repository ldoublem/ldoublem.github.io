---
title: android修改应用字体
date: 2016-05-06 09:32
tags: android 
---
在有些需求里面需要修改默认的字体，可以直接在Application里面写如下代码

```
FontsOverride.setDefaultFont(this, "DEFAULT", "fonts/xxx.ttf");//修改系统字体
FontsOverride.setDefaultFont(this, "MONOSPACE", "fonts/yyy.ttf");
```
FontsOverride类如下

```
public final class FontsOverride {
 
 public static void setDefaultFont(Context context,
            String staticTypefaceFieldName, String fontAssetName) {
        final Typeface regular = Typeface.createFromAsset(context.getAssets(),
                fontAssetName);
        replaceFont(staticTypefaceFieldName, regular);
    }
 
    protected static void replaceFont(String staticTypefaceFieldName,
            final Typeface newTypeface) {
        try {
            final Field staticField = Typeface.class
                    .getDeclaredField(staticTypefaceFieldName);
            staticField.setAccessible(true);
            staticField.set(null, newTypeface);
        } catch (NoSuchFieldException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        }
    }
}
```