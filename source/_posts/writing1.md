---
title: 使用Gson解析报错
date: 2016-05-03 10:55
tags: gson 
---
如果解析一个json数据成一个List，一般的使用方式是不会报错的。

public  List<class> parse(String string) {

List<class> data =new ArrayList<class>();

Gson gson =newGson();

data = gson.fromJson(string,new TypeToken<ArrayList<class>>() {

}.getType());

return data;

}

但是如果使用泛型的方式就会报如下错误 

java.lang.ClassCastException: com.google.gson.internal.LinkedTreeMap cannot be cast to xxx

正确的使用方式是

public List parse(String s,Class clazz) {

T[] arr =newGson().fromJson(s,clazz);

return Arrays.asList(arr);

}