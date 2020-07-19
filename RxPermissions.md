# RxPermissions

### GitHub：

https://github.com/tbruyelle/RxPermissions

### 1.介绍

简单点来说这个库是用于快速添加权限的库

This library allows the usage of RxJava with the new Android M permission model.
 即：  这个库支持RxJava与新的Android M版本权限模型一起使用。

### 2. 添加依赖

To use this library your `minSdkVersion` must be >= 14。

```java
allprojects {
    repositories {
        ...
        maven { url 'https://jitpack.io' }
    }
}

dependencies {
    implementation 'com.github.tbruyelle:rxpermissions:0.12'
}
```

### 3. 如何在项目中使用



