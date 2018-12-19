---
title: React Native项目打包-Android
tags:
  - react-native
url: 161.html
id: 161
categories:
  - 移动开发
date: 2016-10-12 19:09:44
---

1、生成签名秘钥

keytool -genkey -v -keystore my-release-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000

按照提示输入各项 完成后会在本目录下生成一个 .keystore 文件，将文件复制到项目的 android/app 目录下 2、gradle 文件配置 打开 android/app 目录下的 build.gradle 文件，添加以下内容

...
android {
    ...
    defaultConfig { ... }
    signingConfigs {
        release {
            storeFile file(MYAPP\_RELEASE\_STORE_FILE)
            storePassword MYAPP\_RELEASE\_STORE_PASSWORD
            keyAlias MYAPP\_RELEASE\_KEY_ALIAS
            keyPassword MYAPP\_RELEASE\_KEY_PASSWORD
        }
    }
    buildTypes {
        release {
            ...
            signingConfig signingConfigs.release
        }
    }
}
...

MYAPP\_RELEASE\_STORE_FILE 等变量在 android 目录下的 gradle.properties 文件中配置

MYAPP\_RELEASE\_STORE_FILE=my-release-key.keystore
MYAPP\_RELEASE\_KEY_ALIAS=my-key-alias
MYAPP\_RELEASE\_STORE_PASSWORD=*****
MYAPP\_RELEASE\_KEY_PASSWORD=*****

3、打包 在 android/app/src/main 目录下创建 assets 目录，然后回到项目的根目录，执行

react-native bundle --platform android --dev false --entry-file index.android.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res/

然后回到 android 目录下，执行

./gradlew assembleRelease

4、APK文件 文件在 android/app/build/outputs/apk 目录下 参考： [https://facebook.github.io/react-native/docs/signed-apk-android.html](https://facebook.github.io/react-native/docs/signed-apk-android.html) [http://www.jianshu.com/p/61e27d9b02f2](http://www.jianshu.com/p/61e27d9b02f2)