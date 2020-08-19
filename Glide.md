# Glide

一个Google官方加载网络图片的第三方库

### 1.GitHub地址

https://github.com/bumptech/glide

### 2.加入依赖

使用Glide：

```groovy
repositories {
  google()
  jcenter()
}

dependencies {
  implementation 'com.github.bumptech.glide:glide:4.11.0'
  annotationProcessor 'com.github.bumptech.glide:compiler:4.11.0'
}
```

或者使用Maven：

```xml
<dependency>
  <groupId>com.github.bumptech.glide</groupId>
  <artifactId>glide</artifactId>
  <version>4.11.0</version>
</dependency>
<dependency>
  <groupId>com.github.bumptech.glide</groupId>
  <artifactId>compiler</artifactId>
  <version>4.11.0</version>
  <optional>true</optional>
</dependency>
```

### 3.加入网络允许

- <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
- <uses-permission android:name="android.permission.INTERNET" />
- 有些情况下需要指定Application的 android:networkSecurityConfig="@xml/network_security_config" 允许http网络明文传输

### 4.在Activity中添加需要的代码

```java
// For a simple view:
@Override public void onCreate(Bundle savedInstanceState) {
  ...
  ImageView imageView = (ImageView) findViewById(R.id.my_image_view);

  Glide.with(this).load("http://goo.gl/gEgYUd").into(imageView);
}

// For a simple image list:
@Override public View getView(int position, View recycled, ViewGroup container) {
  final ImageView myImageView;
  if (recycled == null) {
    myImageView = (ImageView) inflater.inflate(R.layout.my_image_view, container, false);
  } else {
    myImageView = (ImageView) recycled;
  }

  String url = myUrls.get(position);

  Glide
    .with(myFragment)
    .load(url)
    .centerCrop()
    .placeholder(R.drawable.loading_spinner)
    .into(myImageView);

  return myImageView;
}
```