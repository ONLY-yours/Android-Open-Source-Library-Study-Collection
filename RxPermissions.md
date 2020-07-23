# RxPermissions

### GitHub：

https://github.com/tbruyelle/RxPermissions

### 1.介绍

简单点来说这个库是用于快速添加权限的库

This library allows the usage of RxJava with the new Android M permission model.
 即：  这个库支持RxJava与新的Android M版本权限模型一起使用。

一般来说rxpermission配合rxjava使用可以达到最好的效果

### 2. 添加依赖

To use this library your `minSdkVersion` must be >= 14。

```groovy
allprojects {
    repositories {
        ...
        maven { url 'https://jitpack.io' }
    }
}

dependencies {
    //添加rxjava
    implementation  'io.reactivex.rxjava2:rxjava:2.0.1'
    implementation  'io.reactivex.rxjava2:rxandroid:2.0.1'
    //rxpermission（配套rxjava使用的）
    implementation 'com.tbruyelle.rxpermissions2:rxpermissions:0.9.5@aar'
	
    //单独使用使用下面的rxpermission（建议使用上面的rxjava+rxpermission）
	implementation 'com.github.tbruyelle:rxpermissions:0.12'

}
```

### 3. 如何在项目中使用

1. 权限申请

```xml
	<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_CALENDAR" />
```

2. 再合适的地方使用

```
String permissions = "android.permission.SIGNAL_PERSISTENT_PROCESSES";

RxPermissions rxPermissions = new RxPermissions(MainActivity.this);
rxPermissions.requestEach(permissions)
        .subscribe(new Consumer<Permission>() {
            @Override
            public void accept(Permission permission) throws Exception {
                if (permission.granted) {
                    // 用户已经同意该权限
                    //result.agree(permission);
                } else if (permission.shouldShowRequestPermissionRationale) {
                    // 用户拒绝了该权限，没有选中『不再询问』（Never ask again）,那么下次再次启动时，还会提示请求权限的对话框
                    //result.refuse(permission);
                } else {
                    // 用户拒绝了该权限，并且选中『不再询问』，提醒用户手动打开权限
                    //result.noMoreQuestions(permission);
                }
            }
        });
```

