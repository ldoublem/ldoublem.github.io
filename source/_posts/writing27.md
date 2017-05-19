---
title: Android studio发布包到 Bintray 远程仓库
date: 2016-07-12 13:42
tags: 实用
---
以下用到的工具和命令都是Mac系统下的
1，如果没有bintray帐号先去 https://bintray.com 注册
2，创建签名
下载 [gpgtool](http://gpgtools.org/)
创建证书     
![创建证书.png](http://upload-images.jianshu.io/upload_images/1194532-9c679501cef72a23.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在终端输入命令,获得公钥 ID
```gpg --list-keys```
![获得公钥.png](http://upload-images.jianshu.io/upload_images/1194532-f595496982223892.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
上传公钥到服务器，继续在终端输入命令
```gpg --keyserver hkp://pool.sks-keyservers.net --send-keys  证书公钥```
生成公钥和私钥文件，来配置bintray的 public key 和 private key
在终端输入
![email@your-mailbox.com.png](http://upload-images.jianshu.io/upload_images/1194532-c71a28402d714bcb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```gpg -a --export  email@your-mailbox.com > public_key_sender.asc```
``` gpg -a --export-secret-key  
email@your-mailbox.com > private_key_sender.asc```
把命令行输出的证书记录下来然后打开https://bintray.com/profile/edit
进行配置。
![配置.png](http://upload-images.jianshu.io/upload_images/1194532-101185f2a4b1ca77.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
设置 bintray maven 包自动签名
![自动签名1.png](http://upload-images.jianshu.io/upload_images/1194532-571be7072d6114c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![自动签名2.png](http://upload-images.jianshu.io/upload_images/1194532-faa5363df66b7f57.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3，新建一个maven仓库，这里的name到时候要用到的
![新建maven.png](http://upload-images.jianshu.io/upload_images/1194532-b73a5b7981f6f09d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
到此Bintray的配置就好了，以后就可以直接使用了。然后就要转到Android studio创建项目了
4，创建并配置library项目
![新建library.png](http://upload-images.jianshu.io/upload_images/1194532-a275cbf4442e260f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在project的build.gradle配置
![build.gradle.png](http://upload-images.jianshu.io/upload_images/1194532-1022fab2e5512325.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
``` 
classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.0'
classpath 'com.github.dcendents:android-maven-gradle-plugin:1.3'```
5，配置library module的build.gradle
添加插件
```apply plugin: 'com.github.dcendents.android-maven'```
```apply plugin: 'com.jfrog.bintray'```
添加版本号
```version = "0.0.1"```
添加项目地址
```def siteUrl = 'https://github.com/ldoublem/LoadingView' ```
```def gitUrl = 'https://github.com/ldoublem/LoadingView.git' ```
定义group，要唯一，一般是用包名，可以去https://bintray.com/bintray/jcenter 查询
```group = "com.ldoublem.loadingview"```
定义pom并打包aar，javadoc jar和source jar
```
install {
    repositories.mavenInstaller {
        // This generates POM.xml with proper parameters
        pom {
            project {
                packaging 'aar'
                name 'code For Android'//描述信息
                url siteUrl
                licenses {
                    license {
                        name 'The Apache software License, Version 2.0'
                        url 'http://www.apache.org/licenses/LICENSE-2.0.txt'
                    }
                }
                developers {
                    developer {//开发者信息
                        id ''
                        name 'ldoublem'//你的名字和邮箱
                        email '122710260@qq.com'
                    }
                }
                scm {
                    connection gitUrl
                    developerConnection gitUrl
                    url siteUrl
                }
            }
        }
    }
}
task sourcesJar(type: Jar) {
    from android.sourceSets.main.java.srcDirs
    classifier = 'sources'
}

task javadoc(type: Javadoc) {
    source = android.sourceSets.main.java.srcDirs
    classpath += project.files(android.getBootClasspath().join(File.pathSeparator))
}

task javadocJar(type: Jar, dependsOn: javadoc) {
    classifier = 'javadoc'
    from javadoc.destinationDir
}

javadoc {
    options{
        encoding 'UTF-8'
        charSet 'UTF-8'
        author true
    }
}

artifacts {
    archives javadocJar
    archives sourcesJar
}
```
6，设置local.properties的user和apikey，防止信息泄露，记得使用忽略文件将其忽略提交。
![properties.png](http://upload-images.jianshu.io/upload_images/1194532-283e4852b33807c0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![apikey.png](http://upload-images.jianshu.io/upload_images/1194532-046097d8fe26a137.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
7，在library module的build.gradle配置上传maven仓库，从local.properties读取user和apikey
```
Properties properties = new Properties()
properties.load(project.rootProject.file('local.properties').newDataInputStream())
bintray {
    user = properties.getProperty("bintray.user")
    key = properties.getProperty("bintray.apikey")
    configurations = ['archives']
    pkg {
        repo = "maven"
        name = "loadingviewlib"  // project name in maven
        websiteUrl = siteUrl
        vcsUrl = gitUrl
        licenses = ["Apache-2.0"]
        publish = true
    }
}
```
8，打开Android studio Terminal命令行执行
```./gradlew bintrayUpload  ```
如果成功会有提示，如图

![bintrayUpload.png](http://upload-images.jianshu.io/upload_images/1194532-d5647b28b9f2e5fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
9，最后到bintray的项目页面提交审核，一般工作时间半个小时左右。
10，成功后就可以使用gradle获取网络库了
```compile 'com.ldoublem.loadingview:loadingviewlib:0.0.1'
```
group+ name+版本号
11，升级只要将build.gradle版本号version提高一个版本，然后再次执行bintrayUnload。