# Logger

一个用于Log打印的第三方库

### 1. GitHub

https://github.com/orhanobut/logger

### 2. 添加依赖

```xml
implementation 'com.orhanobut:logger:2.2.0'
```

### 3. 初始化

一般将初始化放在Application中

初始化方法可能各个版本有所不同，详情请见GitHub

```kotlin
Logger.addLogAdapter(new AndroidLogAdapter());
```

### 4. 使用

```kotlin
Logger.d("hello");
```

